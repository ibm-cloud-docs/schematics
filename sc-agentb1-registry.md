---

copyright:
  years: 2017, 2024
lastupdated: "2024-05-16"

keywords: schematics agent proxy server, proxy server, agent proxy, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Configuring {{site.data.keyword.bpshort}} agents to use a private registry
{: #agent-registry-overview}

Agents support the use of custom Terraform providers that are sourced from a private Terraform registry with {{site.data.keyword.bpshort}} Terraform jobs. The support to use custom providers is not available in the shared multi-tenant {{site.data.keyword.bpshort}} service. It is only available with agents. agents do not include a local or private provider registry. Additionally users can configure the registry on the users private network accessible to the agents.  
{: shortdesc}

By default, when {{site.data.keyword.bpshort}} jobs run, the Terraform CLI downloads the required Terraform provider plug-ins or Terraform modules from the public Terraform registry through the internet or public network. When an Agent is deployed on a private network, security policies dictate that a proxy, or mirror site must be used for downloading and caching provider plug-ins. Additionally it may be desired to host custom-developed Terraform providers in a private registry to configure environment-specific resources.

For these use cases, Terraform allows configuration of provider download from alternative provider registries by using a `provider_installation` block in the Terraform CLI configuration. This allows customization of the Terraform default installation behavior. Review the Terraform documentation for [provider installation](https://developer.hashicorp.com/terraform/cli/config/config-file#provider-installation){: external} for more detail on configuring provider download. 

In agents, the following two workspace environment variables can be used to configure the Terraform CLI to refer to an alternative repository and select providers by name and namespace from this registry.  


- The `TF_NETWORK_MIRROR_URL` Terraform private repository, website, or Artifactory instance where custom Terraform providers are hosted.
- The `TF_NETWORK_MIRROR_PROVIDER_NAME` name and namespace of the provider that is to be downloaded from the custom location. Refer to the Terraform documentation for [provider naming and namespaces](https://developer.hashicorp.com/terraform/language/providers/requirements#names-and-addresses){: external}. If not specified it defaults to all providers in all namespaces `"*/*"` to be downloaded from the private registry. 

{{site.data.keyword.bpshort}} auto generates the following Terraform CLI configuration file parameters. During the job execution, Terraform can use the private registry for a few custom providers you intend to use or alternatively for all providers. 

```json
provider_installation {
  network_mirror {
    url = "${TF_NETWORK_MIRROR_URL}"
    include = ["${TF_NETWORK_MIRROR_PROVIDER_NAME}"]
  }
  direct {
     exclude = ["TF_NETWORK_MIRROR_PROVIDER_NAME"]
  }
}
```
{: pre}

## Setting the credentials to access a private provider registry
{: #agent-registry-tf-creds}


For private registries, Terraform must be configured with the access tokens for the target registry. In agents these are defined at a workspace level by using the `TF_TOKEN_` environment variable. See the Terraform [Environment Variable Credentials](https://developer.hashicorp.com/terraform/cli/config/config-file#environment-variable-credentials){: external} documentation for more detail on configuring the passing of credential tokens.  

## Using Artifactory as a provider registry
{: #agent-registry-artifactory}

[Artifactory](https://jfrog.com/artifactory/){: external} provides an alternative solution for sourcing of Terraform providers and fully supports the [Terraform provider registry protocol](https://developer.hashicorp.com/terraform/internals/provider-registry-protocol){: external}. It supports, remote, local, and virtual repositories that aggregate the first two types with a defined search order.   

Local repositories are physical, user managed local repositories. The repositories act as a Terraform private registry where you can host custom-developed providers and manually upload and save public providers to eliminate the need for public network access. Or limit the public providers made available to Terraform users. 

Remote repositories can serve as a caching proxy for both private Terraform registries and the public Terraform registry. Implementing a remote repository still requires public internet access. Here public network access is through Artifactory and not Terraform. Typically many organizations have existing Artifactory installations, with network monitoring and network access rules in place to allow secure public access through Artifactory.    

A virtual Terraform repository, combining a local repo with a remote proxy repo, allows for hosting of custom providers locally along with secure access to any additional public Terraform providers. 


### Configuring a local Artifactory provider registry  
{: #agent-registry-artifactory_1}


A local Artifactory registry can be used to host custom-developed providers for use with agents on a users private network. Artifactory access is configured by using the following workspace environment variables to configure the Terraform CLI to refer to the local repository and select providers by name and namespace from this registry.  

`TF_TOKEN_name.artifactory.user.com:<artifactory_local_registry_token>`
`TF_NETWORK_MIRROR_PROVIDER_NAME:"user_namespace/provider_name"`
`TF_NETWORK_MIRROR_URL=https://name.artifactory.user.com/artifactory/api/terraform/user-terraform-virtual/providers/`

Refer to the Artifactory documentation and UI to source the values for the bearer token and URL of the local registry.  

The following example shows that the {{site.data.keyword.bpshort}} generates a Terraform CLI configuration. 

```json
provider_installation {
                network_mirror {
                        url = "https://name.artifactory.user.com/artifactory/api/terraform/user-terraform-local/providers/"
                        include = ["user_namespace/provider_name"]
                }
                direct {
                        exclude = ["user_namespace/provider_name"]
                }
        }
```
{: pre}

### Configuring a remote Artifactory provider registry 
{: #agent-registry-artifactory_2}


A remote Artifactory registry can be used to cache public providers for use by Terraform, without giving Terraform public network access. Artifactory access is configured by using the following workspace environment variables to configure the Terraform CLI to refer to the remote repository and retrieve all providers by using this proxy registry.  

`TF_TOKEN_name.artifactory.user.com:<artifactory_remote_registry_token>`
`TF_NETWORK_MIRROR_URL=https://name.artifactory.user.com/artifactory/api/terraform/user-terraform-remote/providers/`

Refer to the Artifactory documentation and UI to source the values for the bearer token and URL of the remote registry.  

The following example shows that the {{site.data.keyword.bpshort}} generates a Terraform CLI configuration.

```json
provider_installation {
                network_mirror {
                        url = "https://name.artifactory.user.com/artifactory/api/terraform/user-terraform-remote/providers/"
                        include = ["*/*"]
                }
                direct {
                        exclude = ["*/*"]
                }
        }
```
{: pre}

### Configuring a virtual Artifactory provider registry
{: #agent-registry-artifactory_3}


A virtual Artifactory registry can be used to combine the hosting of custom providers with the caching of public providers for use by Terraform. Artifactory access is configured by using the following workspace environment variables to configure the Terraform CLI to refer to the virtual repository and retrieve all providers by using this proxy registry.  

`TF_TOKEN_name.artifactory.user.com:<artifactory_virtual_registry_token>`
`TF_NETWORK_MIRROR_URL=https://name.artifactory.user.com>/artifactory/api/terraform/<user-terraform-virtual/providers/`

Refer to the Artifactory documentation and UI to source the values for the bearer token and URL of the virtual registry. The virtual repository must be configured as an aggregate of a local and remote registry as discussed in the previous sections.    

The following example shows that the {{site.data.keyword.bpshort}} generates a Terraform CLI configuration.

```json
provider_installation {
                network_mirror {
                        url = "https://name.artifactory.user.com/artifactory/api/terraform/user-terraform-virtual/providers/"
                        include = ["*/*"]
                }
                direct {
                        exclude = ["*/*"]
                }
        }
```
{: codeblock}
