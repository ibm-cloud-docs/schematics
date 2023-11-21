---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-20"

keywords: schematics faqs, schematics agents faq, agents faq, agents, artifactory, provider 

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bplong_notm}} Agent beta-1 and beta-2 delivers a simplified agent installation process and policy for agent assignment. You can review the [beta-1 release](/docs/schematics?topic=schematics-schematics-relnotes&interface=cli#schematics-mar2223) documentation and explore. 
{: attention}

{{site.data.keyword.bpshort}} Agent are a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta1-limitations) that are available for evaluation and testing purposes. It is not intended for production usage.
{: beta}

# Agent
{: #faqs-agent}

Answers to common questions about the Agent for {{site.data.keyword.bplong_notm}}.
{: shortdesc}

## What are the updates in the agent beta-1 release?
{: #faqs-agent-update}
{: faq}
{: support}

The following are the features in Agent beta-1 release.
- Improvements to the agent deployment experience through CLI.
- Support to run Ansible playbooks on the agent.
- Dynamic assignment of workspace or action jobs to the agent.

## What are the costs of installing and using Agents?
{: #faqs-agent-cost}
{: faq}
{: support}

The following are the cost break-down for the {{site.data.keyword.bpshort}} Agent.

Pre-requisite: Agent infrastructure
- Cost of VPC infrastructure elements such as, subnet, public gateways.
- Cost of IBM Kubernetes Service (cluster) on VPC, with three-node worker pool.
- Cost of IBM Cloud Object Storage

Agent beta-1 service
- There is no cost involved in running the agent service. 
- Post beta, the agent feature may be a priced service.

## Can I install more than one Agent on a cluster?
{: #faqs-agent-install}
{: faq}
{: support}

You can install only one agent on a Kubernetes cluster on {{site.data.keyword.containerlong_notm}}. You can install additional agents on different clusters.

You cannot install more than one agent in a single Kubernetes cluster. You will get a failure with namespace conflict error.

## What type of Schematics jobs can I run in my Agent?
{: #faqs-agent-jobs}
{: faq}
{: support}

You can run {{site.data.keyword.bpshort}} Workspace Terraform jobs on an Agent. You can also run {{site.data.keyword.bpshort}} Action jobs, Ansible playbooks on an Agent. 

## How can I see the {{site.data.keyword.bpshort}} job results and logs, for the workloads running on an agent?
{: #faqs-agent-workload}
{: faq}
{: support}

The workspace job or action job logs are available in {{site.data.keyword.bpshort}} UI console. You can also access these job logs using the {{site.data.keyword.bpshort}} Workspace API, or CLI.

## How many {{site.data.keyword.bpshort}} jobs can run in parallel in the Agent?
{: #faqs-agent-parallel}
{: faq}
{: support}

Currently, an agent can run three {{site.data.keyword.bpshort}} jobs in parallel. Any additional jobs are queued and will execute when prior jobs complete execution. 

In future, you will be able to customize an agent to increase the number of job Pods, to increase the number of jobs that can run concurrently. 

## What is the minimum cluster configuration required in Agent release?
{: #faqs-agent-min-cluster}
{: faq}
{: support}

The agent needs {{site.data.keyword.containerlong_notm}} Service with minimum three worker nodes, with a flavor of `b4x16` or higher.

## How many workspaces can be assigned to an agent?
{: #faqs-agent-min-wks}
{: faq}
{: support}

Currently, you can assign any number of workspaces to an agent. The workspace jobs are queued to run on the agent, based on the agent assignment policy.
The agent periodically polls {{site.data.keyword.bpshort}} for jobs to run, with a polling interval of one minute. By default, the agent runs only three jobs in parallel. The remaining jobs are queued.

## How many jobs can run in parallel on an agent?
{: #faqs-agent-min-job}
{: faq}
{: support}

- Schematics Agent can perform three Git download jobs in parallel.
- Schematics Agent can run three Workspace jobs (Terraform commands), in parallel.
- Schematics Agent can run three Action jobs (Ansible playbook), in parallel.

## What is the default polling interval for Agents?
{: #faqs-agent-poll-interval}
{: faq}
{: support}

{{site.data.keyword.bpshort}} maintains a queue of jobs that will be executed on an agent. The agent will poll for {{site.data.keyword.bpshort}} Jobs, every one minute by default.

## What is the difference between agent-location and location input variable flag in Agent service?
{: #faqs-agent-diff-location}
{: faq}
{: support}

The `--agent-location` parameter is a variable that specifies the region of the cluster where an agent service is deployed. For example, `us-south`. This must match the cluster region. 

The `--location` parameter is a variable that specifies the region supported by {{site.data.keyword.bpshort}} service such as `us-south`, `us-east`, `eu-de`, `eu-gb`. The agent polls {{site.data.keyword.bpshort}} service instance from this location, for workspace or action jobs for processing.

## Can an agent run Workspace jobs belonging to different resource groups?
{: #faqs-agent-diff-rg}
{: faq}
{: support}

Yes, an agent can run {{site.data.keyword.bpshort}} Jobs related to workspace or actions, from all or any resource group, in an account. Agent (assignment) policies are used to assign the execution of jobs, based on resource group, region and user tags to a specific agent. 

## Can an agent run Jobs from multiple {{site.data.keyword.bpshort}} regions?
{: #faqs-agent-diff-region}
{: faq}
{: support}

Agents are associated with {{site.data.keyword.bpshort}} regions and geographies and can only execute jobs for the parent {{site.data.keyword.bpshort}} geography, for example, North America or Europe.  

The Agent periodically polls the regional endpoint of the {{site.data.keyword.bpshort}} service instance, to fetch and run jobs. It can connect to only one regional endpoint (home). For example, if an agent is deployed on a cluster in Sydney and has been configured to use the {{site.data.keyword.bpshort}} `eu-de` regional endpoint as it’s home location. The agent polls for jobs in `eu-de` region. Hence, the workspace or action to deploy resources using the Sydney agent must be created in the `eu-de` region. 

## Can I register an agent with multiple accounts?
{: #faqs-agent-register}
{: faq}
{: support}

No, you cannot register an agent with multiple accounts in the beta release.

## Can jobs for an existing workspace be configured to run on an agent?
{: #faqs-agent-conf}
{: faq}
{: support}

Yes, if your workspace has the right values with the tags, resource-group, location. {{site.data.keyword.bpshort}} uses an `agent-selection-policy` to automatically assign the jobs to run on the target agent.

For example, If you have an existing workspace: `wks-0120` with `tag=dev`, and you want the workspace to run on `Agent-1`. Create an `agent-selection-policy` with the rule to pick `Agent-1` when the `tag == dev`.  Subsequently, the workspace job such as plan, apply, update, and so on will be dynamically routed to `Agent-1`.

## What IAM permissions needed to deploy an agent?
{: #faqs-agent-permission}
{: faq}
{: support}

For information about identity and permissions, see [agent permission](/docs/schematics?topic=schematics-access#agent-permissions).

## Can I use Terraform custom providers or use a proxy registry to download Terraform provider plug-ins?
{: #faqs-agent-cust-providers}
{: faq}
{: support}

Agents supports the use of custom Terraform providers sourced from a private Terraform registry with {{site.data.keyword.bpshort}} Terraform jobs. The support to use custom providers is not available in the shared multi-tenant {{site.data.keyword.bpshort}} service. It is only available with Agents. Agents does not include a local or private provider registry. The registry must be provided and configured by the user on the users private network accessible to the agents.  
{: shortdesc}

By default, {{site.data.keyword.bpshort}} jobs running the Terraform CLI will download Terraform provider plug-ins or Terraform modules from the public Terraform registry via the Internet or public network. When an Agent is deployed on a private network, security policies may dictate that a proxy or mirror site must be used for downloading and caching provider plug-ins. Additionally it may be desired to use custom developed Terraform providers to configure environment specific resources using Terraform.  

For these usecases, Terraform allows configuration of provider download from alternate provider registries via the use of a `provider_installation` block in the Terraform CLI configuration. This allows customization of the Terraform default installation behavior. Review the Terraform documentation for [provider installation](https://developer.hashicorp.com/terraform/cli/config/config-file#provider-installation){: external} for more detail on configuring provider download. 

When using Agents, the following two workspace environment variables, can be used to configure the Terraform CLI to refer to an alternate repository and select providers by name and namespace from this registry.  


- The `TF_NETWORK_MIRROR_URL` Terraform private repository, website or Artifactory instance where custom Terraform providers are hosted.
- The `TF_NETWORK_MIRROR_PROVIDER_NAME` name and namespace of provider that is to be downloaded from the custom location. Refer to the Terraform documentation for [provider naming and namespaces](https://developer.hashicorp.com/terraform/language/providers/requirements#names-and-addresses){: external}. This is an optional variable. If not specified it is defaulted to all providers in all namespaces `"*/*"`.  

{{site.data.keyword.bpshort}} auto generates the following Terraform CLI configuration file parameters which tell Terraform during job execution to use an alternate registry for some or all of the providers you intend to use.

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

## How do I set the credentials to access a private provider registry
{: #faqs-agent-tf-creds}
{: faq}
{: support}

When interacting with private registries, Terraform must be configured with the access tokens for the target registry. With Agents these are defined at a workspace level using the `TF_TOKEN_` environment variable. See the Terraform [Environment Variable Credentials](https://developer.hashicorp.com/terraform/cli/config/config-file#environment-variable-credentials){: external} documentation for more detail on configuring this variable. 

## Using Artifactory as a provider registry
{: #faqs-agent-artifactory}
{: faq}
{: support}

[Artifactory](https://jfrog.com/artifactory/){: external} provides a number of different options for sourcing of Terraform providers and fully supports the [Terraform provider registry protocol](https://developer.hashicorp.com/terraform/internals/provider-registry-protocol){: external}. It supports, remote, local and virtual repositories which aggregate the first two types with a defined search order.   

Local repositories are physical, user managed local repositories acting as a Terraform private registry where you can host custom developed providers and manually upload and save public providers to eliminate the need for public network access. Or limit the public providers made available to Terraform users. 

Remote repositories can serve as a caching proxy for both private Terraform registries and the public Terraform registry. Implementing a remote repository still requires public internet access. Here public network access is via Artifactory and not Terraform. Typically many organizations have existing Artifactory installations, with network monitoring and network access rules in place to allow secure public access from Artifactory.    

A virtual Terraform repository, combining a local repo with a remote proxy repo, allows for hosting of custom providers locally along with secure access to any additional public Terraform providers. 


### Configuring a local Artifactory provider registry  

A local Artifactory registry can be used to host custom developed providers for use with agents in a users private network. Artifactory access is configured using the following workspace environment variables, to configure the Terraform CLI to refer to the the local repository and select providers by name and namespace from this registry.  

`TF_TOKEN_name.artifactory.user.com:<artifactory_local_registry_token>`
`TF_NETWORK_MIRROR_PROVIDER_NAME:"user_namespace/provider_name"`
`TF_NETWORK_MIRROR_URL=https://name.artifactory.user.com/artifactory/api/terraform/user-terraform-virtual/providers/`

Refer to the Artifactory documentation and UI to source the values for the bearer token and URL of the local registry.  

{{site.data.keyword.bpshort}} will generate a Terraform CLI configuration of the form below. 

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

A remote Artifactory registry can be used to cache public providers for use by Terraform, without giving Terraform public network access. Artifactory access is configured using the following workspace environment variables, to configure the Terraform CLI to refer to the the remote repository and retrieve all providers using this proxy registry.  

`TF_TOKEN_name.artifactory.user.com:<artifactory_remote_registry_token>`
`TF_NETWORK_MIRROR_URL=https://name.artifactory.user.com/artifactory/api/terraform/user-terraform-remote/providers/`

Refer to the Artifactory documentation and UI to source the values for the bearer token and URL of the remote registry.  

{{site.data.keyword.bpshort}} will generate a Terraform CLI configuration of the form below. 

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

A virtual Artifactory registry can be used to combine the hosting of custom provides with the caching of public providers for use by Terraform. Artifactory access is configured using the following workspace environment variables, to configure the Terraform CLI to refer to the the virtual repository and retrieve all providers using this proxy registry.  

`TF_TOKEN_name.artifactory.user.com:<artifactory_virtual_registry_token>`
`TF_NETWORK_MIRROR_URL=https://name.artifactory.user.com>/artifactory/api/terraform/<user-terraform-virtual/providers/`

Refer to the Artifactory documentation and UI to source the values for the bearer token and URL of the virtual registry. The virtual repository must be configured as an aggregate of a local and remote registry as discussed in the previous sections.    

{{site.data.keyword.bpshort}} will generate a Terraform CLI configuration of the form below. 

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
{: pre}

## Can I inject self signed or TLS certificates in {{site.data.keyword.containerlong_notm}} pod or container's trusted CA root certificate store during agent runtime?
{: #faqs-agent-certificate}
{: faq}
{: support}

Yes, follow these steps to inject the certificates into an agent runtime.

In the four `.cer` extension filenames ensure you modify to replace the space with underscore.
{: note}

1. Create config map by using `.cer` file as shown in the `kubectrl` command.

    ```sh
    kubectl -n schematics-runtime create configmap bnpp-root —-from-file 2014-2044_BNPP_Root.cer
    ```
    {: pre}

    ```sh
    kubectl -n schematics-runtime create configmap bnpp-authentication —-from-file 2014-2029 BNPP_Users_Authentication.cer
    ```
    {: pre}

    ```sh
    kubectl -n schematics-runtime create configmap bnpp-infrastructure —-from-file 2014-2029 BNPP_Infrastructure.cer
    ```
    {: pre}

2. Mount config map file as a volume in a directory `/etc/ssl/certs/` as file `agent-runtime-deployment-certs.yaml` in a shared `bnpp_agent_deployment_files` directory.

Shared directory `bnpp_agent_deployment_files` has two yaml files named 
    - `agent-runtime-deployment-certs.yaml` and
    - `agent-runtime-deployment.yaml`.

The `agent-runtime-deployment-certs.yaml` file updates the certificates and appends the `agent-runtime-deployment.yaml` file which provides you the desired deployment details to inject the certificates without any additional changes.

## What attributes of Workspaces or Actions are used to dynamically select a target agent for execution
{: #agent-dynamic-attribute}
{: faq}
{: support}

The following attributes of the {{site.data.keyword.bpshort}} Workspace or {{site.data.keyword.bpshort}} Action are used to dynamically select the agent instance.
{: shortdesc}

- Resource group
- Location (region)
- Tags

The [Agent assignment policy](/docs/schematics?topic=schematics-policy-manage) for an agent instance describes how an Agent is selected to run a Workspace job or Action job.

Example:

If your organization has three different network isolation zones (such as `Dev`, `HR-Stage`, and `HR-Prod`) and you have installed three agents (one each, for the three network isolation zones). You have defined an `agent-assignment-policy` for the agent running in `Dev`, with the selector as `tags=dev`. All workspaces that have `tags=dev` automatically are bound to the `Dev` agent. In other words, the `Dev` agent is used to download Terraform templates (from the Git repository) and run Terraform jobs. Similarly, the `agent-assignment-policy` can include other attributes of the workspaces, to define the agent for job execution.

## How can I enable debug mode in an agent?
{: #faqs-agent-debugmode}
{: faq}
{: support}

You can follow these steps to enable or disable the debug mode of an agent.

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Click **Kubernetes** from the left hand navigator pane, then click **Clusters** 
3. On the **Kubernetes Clusters** page, click your **cluster** > **Kubernetes dashboard**.
    - Click the **default** drop down to view the list of **Namespaces**:
        - In the drop down, type the **{{site.data.keyword.bpshort}}-job-runtime** Namespaces.
        - Click **Config Map** from the **Config and Storage**.
        - From the **Config Maps** page. Click the three dots against **schematics-jobrunner-config**.
        - Click **Edit** to view the **Edit a resource** page with the **YAML**, and **JSON** tabs.
        - You can now edit the `JR_LOGGERLEVEL` parameter for job-runner microservice logging. By default the value is `-1` that indicated disable debug, to enable you need to edit `JR_LOGGERLEVEL` as `0`.
        - Click **Update** to apply your edits.

## Can I upgrade an agent beta-0 to agent beta-1?
{: #faqs-agent-upgrade}
{: faq}
{: support}

No, you cannot upgrade agent beta-0 setup to agent beta-1.

## Does agent beta-1 support backward compatibility?
{: #faqs-agent-compatibility}
{: faq}
{: support}

Agents beta-1 does not support backward compatibility. You need to create a new agent by using the [agent beta-1](/docs/schematics?topic=schematics-agentb1-about-intro) setup.


## Are Schematics Agents the same as Terraform Cloud Agents?
{: #faqs-agent-terraform-agent}
{: faq}
{: support}

{{site.data.keyword.bpshort}} Agents perform a similar role to [Terraform Cloud agents](https://developer.hashicorp.com/terraform/cloud-docs/agents){: external}.


## Do the agents run on IBM Cloud cloud resources?
{: #faqs-agent-run}
{: faq}
{: support}

Schematics Agents can run only Terraform and Ansible workloads. For the Beta, the agents are deployed in IBM Cloud IKS Clusters in the user account.

## What are the minimum cluster configuration needed to support 30 jobs on the {{site.data.keyword.bpshort}} agent?
{: #faqs-agent-min-cluster-conf}
{: faq}
{: support}

For the {{site.data.keyword.vsi_is_full}} or {{site.data.keyword.containerlong}} cluster. You need `9` minimum number of nodes, with a `bx2.4x16` flavor, and edit the following agent microservices deployments to have the prescribed replica count.

| Microservice | Number of replicas |
| -- | -- |
| jobrunner | 4 |
| sandbox | 8 |
| runtime-ws | 16 |
{: caption="agent microservice deployments" caption-side="bottom"}

