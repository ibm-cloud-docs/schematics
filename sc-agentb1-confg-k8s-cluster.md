---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-29"

keywords: configuring kubernetes cluster for agent, configure kubernetes cluster, kubernetes cluster

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bplong_notm}} Agent beta-1 delivers a simplified agent installation process. You can review the [beta-1 release](/docs/schematics?topic=schematics-schematics-relnotes&interface=cli#schematics-mar2223) documentation and explore. 
{: attention}

{{site.data.keyword.bpshort}} Agent are a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta1-limitations) that are available for evaluation and testing purposes. It is not intended for production usage.
{: beta}

# Configuring Kubernetes cluster for agent
{: #configure-k8s-cluster}

Agents for {{site.data.keyword.bplong}} extends its ability to work directly with your cloud infrastructure on your private network or in any network isolation zones. The agent is deployed in your Kubernetes cluster, on {{site.data.keyword.cloud}}. 
{: shortdesc}

When an agent is deployed in your cluster, by default the following configurations are automatically applied to the cluster.

## Default network policies
{: #k8s-cluster-network-policy}

The following network policies are configured automatically to the Kubernetes cluster.

| Policy  |	Description |
| --- | --- |
| `Deny-all-sandbox` |	`Namespace:schematics-sandbox`, denies all the `ingress` and `egress` traffic. |
| `Deny-all-runtime` |	`Namespace:schematics-runtime`, denies all the `ingress` and `egress` traffic. |
| `Whitelist-sandbox` |	`Namespace:schematics-sandbox`, allowed list, and needed ports for `ingress = 3000`, and for `egress TCP = 80`, `443`, `5986`, `22`, `53`, or `egress UDP = 53`, `443`. |
| `Whitelist-ingress-job-ports` | `Namespace:schematics-runtime`, allowed and needed ports for `ingress = 3002`. |
| `Whitelist-runtime-gen-ports` |  `Namespace:schematics-runtime`, allowed and needed ports for `ingress = 3002`, and for `egress TCP = 80`, `443`, `5986`, `22`, `53`, `8080`, `10250`, `9092`, `9093`, or `egress UDP = 53`, `443`, `10250`, `9092`, `9093`. |
{: caption="Default network policies" caption-side="top"}

## Default Terraform and Ansible runtime-job
{: #k8s-cluster-runtime-job}

The following are configured by default Kubernetes deployment configuration applied for the Terraform and Ansible runtime-job.

| Parameter	| Description |
| --- | --- |
| `resource-limits` |	Resource limit setting for the Terraform and Ansible jobs are `cpu = 500m`, and `memory = 1Gi`. |
| `replicas` | Number of Terraform and Ansible job pods. `replica = 3`. **Note** when the number of replica is changed, then the `JR_MAXJOBS` settings must also be updated.| 
{: caption="Default Terraform and Ansible runtime-job" caption-side="top"} 

## Default agent sandbox allowed list
{: #agent-sandbox-allowlist}

The following are the default agent sandbox allowlist configuration.

| Parameter |	Description |
| -- | -- |
| `SANDBOX_WHITELISTEXTN` |	From the Terraform Git repositories following are the allowed file extensions. </br> `.tf`, `.tfvars`, `.md`, `.yaml`, `.sh`, `.txt`, `.yml`, `.html`, `.tf.json`, `license`, `.js`, `.pub`, `.service`, `_rsa`, `.py`, `.json`, `.tpl`, `.cfg`, `.ps1`, `.j2`, `.zip`, `.conf`, `.crt`, `.key`, `.der`, `.jacl`, `.properties`, `.cer`, `.pem`, `.tmpl`, `.netrc`. |
| `SANDBOX_ANSIBLEACTIONWHITELISTEXTN` | From the Ansible Git repositories following are the allowed file extensions. </br> `.md`, `.yaml`, `.sh`, `.txt`, `.yml`, `.html`, `license`, `.js`, `.pub`, `.service`, `_rsa`, `.py`, `.json`, `.tpl`, `.cfg`, `.ps1`, `.j2`, `.zip`, `.conf`, `.crt`, `.key`, `.der`, `.cer`, `.pem`, `.bash`, `.tmpl`.|
| `SANDBOX_BLACKLISTEXTN` |	From the Git repositories following are the blocked file extensions. </br> `.php5`, `.pht`, `.phtml`, `.shtml`, `.asa`, `.asax`, `.swf`, `.xap`, `.tfstate`, `.tfstate.backup`, `.exe`.|
| `SANDBOX_IMAGEEXTN` |	From the Git repositories following are the allowed image file extensions. </br> `.tif`, `.tiff`, `.gif`, `.png`, `.bmp`, `.jpg`, `.jpeg`, `.so`. |
| `SANDBOX_MAX_FILE_SIZE` |	Maximum size of a file that is allowed from the Git repositories is 2 MB. (Yet to be implemented) |
{: caption="Default agent sandboc allowlist configuration" caption-side="top"}

## Default agent job-runner configuration
{: #agent-job-runner-config}

The following are the default agent job-runner configuration.

| Parameter	| Description |
| --- | --- |
|  `JR_MAXJOBS`|	Number of parallel jobs the Terraform or Ansible job run by an agent. |
| `resource-limits` | Resource limit setting for the Terraform and Ansible jobs are `cpu = 500m`, and `memory = 1Gi`. |
{: caption="Default agent job-runner" caption-side="top"}

## Default agent runtime configuration for Terraform 
{: #agent-runtime-config-terraform}

The following are the default agent runtime configuration for Terraform runtime.

| Parameter	| Description |
| --- | --- |
| `JOB_WHITELISTEXTN` |	The allowed file extensions from the Git repositories (includes the dependent module repository).</br>`.tf`, `.tfvars`, `.md`, `.yaml`, `.sh`, `.txt`, `.yml`, `.html`, `.tf.json`, `license`, `.js`, `.pub`, `.service`, `_rsa`, `.py`, `.json`, `.tpl`, `.cfg`, `.ps1`, `.j2`, `.zip`, `.conf`, `.crt`, `.key`, `.der`, `.jacl`, `.properties`, `.cer`, `.pem`, `.tmpl`, `.netrc`. |
| `JOB_BLACKLISTEXTN` |	The blocked file extensions from the Git repositories.</br>`.php5`, `.pht`, `.phtml`, `.shtml`, `.asa`, `.asax`, `.swf`, `.xap`, `.tfstate`, `.tfstate.backup`, `.exe`. |
| `JOB_IMAGEEXTN` |	The allowed image file extensions from the Git repositories.</br>`.tif`, `.tiff`, `.gif`, `.png`, `.bmp`, `.jpg`, `.jpeg`, `.so`. |
{: caption="Default agent runtime configuration for Terraform" caption-side="top"}

## Default agent runtime configuration for Ansible
{: #agent-runtime-config-ansible}

The following are the default agent runtime configuration for Ansible runtime.

| Parameter	| Description |
| --- | --- |
| `ANSIBLE_JOB_WHITELISTEXTN` |	The allowed file extensions from the Git repositories that includes the dependent module repository.</br>`.md`, `.yaml`, `.sh`, `.txt`, `.yml`, `.html`, `.tf.json`, `license`, `.js`, `.pub`, `.service`, `_rsa`, `.py`, `.json`, `.tpl`, `.cfg`, `.ps1`, `.j2`, `.zip`, `.conf`, `.crt`, `.key`, `.der`, `.jacl`, `.properties`, `.cer`, `.pem`, `.tmpl`, `.netrc`.|
| `ANSIBLE_JOB_BLACKLISTEXTN` |	The blocked file extensions from the Git repositories.</br>`.php5`, `.pht`, `.phtml`, `.shtml`, `.asa`, `.asax`, `.swf`, `.xap`, `.tfstate`, `.tfstate.backup`, `.exe`.|
| `JOB_IMAGEEXTN` |	The allowed image file extensions from the Git repositories.</br>`.tif`, `.tiff`, `.gif`, `.png`, `.bmp`, `.jpg`, `.jpeg`, `.so`. |
{: caption="Default agent runtime configuration for Ansible" caption-side="top"}
