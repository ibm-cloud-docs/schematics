---

copyright:
  years: 2017, 2024
lastupdated: "2024-08-30"

keywords: schematics, generate plan, schematics workspace generate plan

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# The Terraform script that contains Kubernetes cluster namespaces fails as connection refused?
{: #wks-connection-refuse}

When you `Generate Plan` for the Terraform script that contains cluster, used to work properly three months ago, but now the same Terraform script gets the following error.
{: tsSymptoms}

```text
"Error: Get "http://localhost/api/v1/namespaces/external-secrets": 
dial tcp [::1]:80: connect: connection refused".
```
{: screen}

When compared the logs between successful and failed `Generate Plan` execution. Observed `new versions picked up of providers`, forced the script to the same version just to ensure a version change did not cause the error. Still the error persists.

This issue is from Kubernetes provider, there are many discussion around this error by different Cloud providers, but there is no definite solution.
{: tsCauses}

Check the following references and solutions provided to fix the issue
{: tsResolve}

References
:   - Terraform [don’t use Kubernetes provider with your cluster resource](https://itnext.io/terraform-dont-use-kubernetes-provider-with-your-cluster-resource-d8ec5319d14a){: external}
    - Many [open issues on Kubernetes provider](https://github.com/hashicorp/terraform-provider-kubernetes/issues?q=is%3Aissue+is%3Aopen+http%3A%2F%2Flocalhost%2Fapi%2F){: external}
    - [Stack overflow issue](https://stackoverflow.com/questions/70962800/error-post-http-localhost-api-v1-namespaces-kube-system-configmaps-dial-tc){: external}

Based on previous experience following are the two workaround solution to fix.

Solution 1

If you are using provider authentication to Kubernetes as shown in the codeblock.

```terraform
provider "kubernetes" {
  config_path = data.ibm_container_cluster_config.cluster_config.config_file_path
}
```
{: codeblock}

Change the template as follows.

```terraform
provider "kubernetes" {
  host                   = data.ibm_container_cluster_config.cluster_config.host
  client_certificate     = data.ibm_container_cluster_config.cluster_config.admin_certificate
  client_key             = data.ibm_container_cluster_config.cluster_config.admin_key
  cluster_ca_certificate = data.ibm_container_cluster_config.cluster_config.ca_certificate
}
```
{: codeblock}

Solution 2

You can [remove the state](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-wks_staterm) related to cluster config from the `statefile` by using {{site.data.keyword.bplong_notm}} CLI command and re-run the `Terraform plan` or `Terraform apply` command.

```sh
ibmcloud schematics workspace state rm --id --address <enter the workspace ID and the address of the resource to mark as taint>
```
{: pre}
