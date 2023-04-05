---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-05"

keywords: configuring kubernetes cluster for agent, configure kubernetes cluster, kubernetes cluster

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bplong_notm}} Agent beta-1 delivers a simplified agent installation process and policy for agent assignment.. You can review the [beta-1 release](/docs/schematics?topic=schematics-schematics-relnotes&interface=cli#schematics-mar2223) documentation and explore. 
{: attention}

{{site.data.keyword.bpshort}} Agent are a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta1-limitations) that are available for evaluation and testing purposes. It is not intended for production usage.
{: beta}

# Configuring Kubernetes cluster for agent
{: #configure-k8s-cluster}

Agents for {{site.data.keyword.bplong}} extends its ability to work directly with your cloud infrastructure on your private network or in any isolated network zones. The agent is deployed in a Kubernetes cluster on {{site.data.keyword.cloud}}. 
{: shortdesc}

When an agent is deployed in your cluster, by default the following configurations are automatically applied to the cluster.

## Default network policies
{: #k8s-cluster-network-policy}

The following network policies are configured automatically to the Kubernetes cluster.

| Policy  |	Description |
| --- | --- |
| `deny-all-jobrunner` | `Namespace:schematics-job-runtime`, denies all the `Ingress` and `Egress` traffic. |
| `deny-all-runtime` |	`Namespace:schematics-runtime`, denies all the `Ingress` and `Egress` traffic. |
| `deny-all-sandbox` |	`Namespace:schematics-sandbox`, denies all the `Ingress` and `Egress` traffic. |
| `whitelist-egress-jobrunner` | `Namespce:schematics-job-runtime`, allowed and needed ports for `egress TCP = 443`, `53`, `3000`, `3002`, and for `egress UDP = 443`,`53`.|
| `runtime-ingress-job` | `Namespace:schematics-runtime`, allowed and needed ports for ingress is `3002`. |
| `Whitelist-sandbox` |	`Namespace:schematics-sandbox`, allowed list, and needed ports for `ingress = 3000`, and for `egress TCP = 80`, `443`, `5986`, `22`, `53`, or `egress UDP = 53`, `443`. |
| `Whitelist-runtime-egress-gen-ports` |  `Namespace:schematics-runtime`, allowed and needed ports for `ingress = 3002`, and for `egress TCP = 80`, `443`, `5986`, `22`, `53`, `8080`, `10250`, `9092`, `9093`, or `egress UDP = 53`, `443`, `10250`, `9093`, `9093`. |
{: caption="Network policies" caption-side="top"}

You can customize by following the steps to [edit the default configuration](/docs/schematics?topic=schematics-configure-k8s-cluster#edit-agent-namespace-confg).
{: note}

## Default Terraform and Ansible runtime-job
{: #k8s-cluster-runtime-job}

The following are configured by default Kubernetes deployment configuration applied for the Terraform and Ansible runtime-job.

| Parameter	| Description |
| --- | --- |
| `resource-limits` |	Resource limit setting for the Terraform and Ansible jobs are `cpu = 500m`, and `memory = 1Gi`. |
| `replicas` | Number of Terraform and Ansible job pods. `replica = 3`. **Note** when the number of replica is changed, then the `JR_MAXJOBS` settings must also be updated.| 
{: caption="Terraform and Ansible runtime-job" caption-side="top"} 

You can customize by following the steps to [edit the default configuration](/docs/schematics?topic=schematics-configure-k8s-cluster#edit-agent-namespace-confg).
{: note}

## Agent job-runner configuration
{: #agent-job-runner-config}

The following is the default agent `schematics-job-runner` configuration.

| Parameter	| Description |
| --- | --- |
| `resource-limits` |	Resource limit setting for the jobrunner are `cpu = 500m`, and `memory = 1Gi`. |
| `replicas` | Number of job pods. `replica = 1`. **Note** when the number of replica is changed, then the `JR_MAXJOBS` settings must also be updated.| 
{: caption="Default agent job-runner" caption-side="top"}

You can customize by following the steps to [edit the default configuration](/docs/schematics?topic=schematics-configure-k8s-cluster#edit-agent-namespace-confg).
{: note}

## Sandbox configuration
{: #k8s-cluster-sandbox}

Following resource limits and replicas are the default configuration applied for `schematics-sandbox` namespace.

| Parameter	| Description |
| --- | --- |
| `resource-limits` |	Resource limit setting for the sandbox are `cpu = 500m`, and `memory = 1Gi`. |
| `replicas` | Number of job pods. `replica = 3`. **Note** when the number of replica is changed, then the `JR_MAXJOBS` settings must also be updated.| 
{: caption="Sandbox deployments" caption-side="top"} 

You can customize by following the steps to [edit the default configuration](/docs/schematics?topic=schematics-configure-k8s-cluster#edit-agent-namespace-confg).
{: note}

## {{site.data.keyword.bpshort}} agents controller manager
{: #k8s-cluster-agent-controller-manager}

Following resource limits and replicas are the default configuration applied in the `schematics-agents-observe` namespace.

| Parameter	| Description |
| --- | --- |
| `resource-limits` |	Resource limit setting for the Terraform and Ansible jobs are `cpu = 500m`, and `memory = 25Mi`. |
| `replicas` | Number of job pods. `replica = 1`. **Note** when the number of replica is changed, then the `JR_MAXJOBS` settings must also be updated.| 
{: caption="{{site.data.keyword.bpshort}} agent controller manager deployments" caption-side="top"}

You can customize by following the steps to [edit the default configuration](/docs/schematics?topic=schematics-configure-k8s-cluster#edit-agent-namespace-confg).
{: note}

## Agent sandbox allowed list
{: #agent-sandbox-allowlist}

Following are the default agent sandbox file type and size allowlist configuration.

| Parameter |	Description |
| -- | -- |
| `SANDBOX_WHITELISTEXTN` |	From the Terraform Git repositories following are the allowed file extensions. </br> `.tf`, `.tfvars`, `.md`, `.yaml`, `.sh`, `.txt`, `.yml`, `.html`, `.gitignore`, `.tf.json`, `license`, `.js`, `.pub`, `.service`, `_rsa`, `.py`, `.json`, `.tpl`, `.cfg`, `.ps1`, `.j2`, `.zip`, `.conf`, `.crt`, `.key`, `.der`, `.jacl`, `.properties`, `.cer`, `.pem`, `.tmpl`, `.netrc`. |
| `SANDBOX_ANSIBLEACTIONWHITELISTEXTN` | From the Ansible Git repositories following are the allowed file extensions. </br> `.tf`, `.tfvars`, `.md`, `.yaml`, `.sh`, `.txt`, `.yml`, `.html`, `.gitignore`, `license`, `.js`, `.pub`, `.service`, `_rsa`, `.py`, `.json`, `.tpl`, `.cfg`, `.ps1`, `.j2`, `.zip`, `.conf`, `.crt`, `.key`, `.der`, `.cer`, `.pem`, `.bash`, `.tmpl`.|
| `SANDBOX_BLACKLISTEXTN` |	From the Git repositories following are the blocked file extensions. </br> `.php5`, `.pht`, `.phtml`, `.shtml`, `.asa`, `.asax`, `.swf`, `.xap`, `.tfstate`, `.tfstate.backup`, `.exe`.|
| `SANDBOX_IMAGEEXTN` |	From the Git repositories following are the allowed image file extensions. </br> `.tif`, `.tiff`, `.gif`, `.png`, `.bmp`, `.jpg`, `.jpeg`, `.so`. |
| `SANDBOX_MAX_FILE_SIZE` |	Maximum size of a file that is allowed from the Git repositories is 2 MB. (Yet to be implemented) |
{: caption="Default agent sandboc allowlist configuration" caption-side="top"}

You can customize by following the steps to [edit the default configuration](/docs/schematics?topic=schematics-configure-k8s-cluster#edit-agent-namespace-confg).
{: note}

## Agent runtime configuration for Terraform 
{: #agent-runtime-config-terraform}

The following is the default agent runtime configuration for the Terraform runtime.

| Parameter	| Description |
| --- | --- |
| `JOB_WHITELISTEXTN` |	The allowed file extensions from the Git repositories (includes the dependent module repository).</br>`.tf`, `.tfvars`, `.md`, `.yaml`, `.sh`, `.txt`, `.yml`, `.html`, `.gitignore`, `.tf.json`, `license`, `.js`, `.pub`, `.service`, `_rsa`, `.py`, `.json`, `.tpl`, `.cfg`, `.ps1`, `.j2`, `.zip`, `.conf`, `.crt`, `.key`, `.der`, `.jacl`, `.properties`, `.cer`, `.pem`, `.tmpl`, `.netrc`. |
| `JOB_BLACKLISTEXTN` |	The blocked file extensions from the Git repositories.</br>`.php5`, `.pht`, `.phtml`, `.shtml`, `.asa`, `.asax`, `.swf`, `.xap`, `.tfstate`, `.tfstate.backup`, `.exe`. |
| `JOB_IMAGEEXTN` |	The allowed image file extensions from the Git repositories.</br>`.tif`, `.tiff`, `.gif`, `.png`, `.bmp`, `.jpg`, `.jpeg`, `.so`. |
{: caption="Default agent runtime configuration for Terraform" caption-side="top"}

You can customize by following the steps to [edit the default configuration](/docs/schematics?topic=schematics-configure-k8s-cluster#edit-agent-namespace-confg).
{: note}

## Agent runtime configuration for Ansible
{: #agent-runtime-config-ansible}

The following is the default agent runtime configuration for the Ansible runtime.

| Parameter	| Description |
| --- | --- |
| `ANSIBLE_JOB_WHITELISTEXTN` |	The allowed file extensions from the Git repositories that includes the dependent module repository.</br>`.tf`, `.tfvars`, `.md`, `.yaml`, `.sh`, `.txt`, `.yml`, `.html`, `.gitignore`, `.tf.json`, `license`, `.js`, `.pub`, `.service`, `_rsa`, `.py`, `.json`, `.tpl`, `.cfg`, `.ps1`, `.j2`, `.zip`, `.conf`, `.crt`, `.key`, `.der`, `.jacl`, `.properties`, `.cer`, `.pem`, `.tmpl`, `.netrc`.|
| `ANSIBLE_JOB_BLACKLISTEXTN` |	The blocked file extensions from the Git repositories.</br>`.php5`, `.pht`, `.phtml`, `.shtml`, `.asa`, `.asax`, `.swf`, `.xap`, `.tfstate`, `.tfstate.backup`, `.exe`.|
{: caption="Default agent runtime configuration for Ansible" caption-side="top"}

You can customize by following the steps to [edit the default configuration](/docs/schematics?topic=schematics-configure-k8s-cluster#edit-agent-namespace-confg).
{: note}

## Editing default agent namespace configuration
{: #edit-agent-namespace-confg}

You can follow these steps to edit the default configuration of an agent namespace.

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Click **Kubernetes** from the left hand navigator pane, then click **Clusters** 
3. On the **Kubernetes Clusters** page, click your **cluster** > **Kubernetes dashboard**.
    - Click the **default** drop down to view the list of **Namespaces**:
        - In the drop down, type the **{{site.data.keyword.bpshort}}-runtime** Namespaces to view the Workload Status, Deployments, Pods, Replica sets, and so on.
        - From the **Deployments** panel. Click the three dots against **runtime-ansible-job**.
        - Click **Edit** to view the **Edit a resource** page with the **YAML**, and **JSON** tabs.
        - You can now view the parameters and reconfigure to customize your agent configuration.
        - Click **Update** to apply your edits.
4. Similarly, you can edit the configuration for all the agent namespaces to customize.
