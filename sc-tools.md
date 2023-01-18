---

copyright: 
  years: 2017, 2021
lastupdated: "2023-01-18"

keywords: tools and utilities, utilities, tools, runtime tools, schematics tools, schematics utilities

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Runtime environment tools
{: #sch-utilities}

{{site.data.keyword.bpshort}} includes several utilities and executables in its worker job images. 


{{site.data.keyword.bpshort}} deprecates `Python v3.6` support and upgrades the {{site.data.keyword.bpshort}} Job image to support `Python v3.8` from 21 September 2022. For more information, see [{{site.data.keyword.bpshort}} announcement](https://cloud.ibm.com/status/announcement?component=schematics){: external} tools.

Your automation templates are run by {{site.data.keyword.bpshort}}, in a Kubernetes cluster by using a `schematics-runtime-job` image. The `schematics-runtime-job` image embeds the primary Infrastructure as Code (IaC) automation engine, for example, Terraform CLI, Ansible. The `schematics-runtime-job` image also includes more helper software, and tools that are useful in developing an automation.

You cannot install the software or tool in the {{site.data.keyword.bpshort}} runtime. Any software installation in `schematics-runtime-job` pod is considered as vulnerable.
{: note}

The `schematics-runtime-job` image is built by using the [Universal Base Image (UBI-8)](https://catalog.redhat.com/software/containers/ubi8/ubi/5c359854d70cc534b3a3784e){: external}. The following table lists the preinstalled helper software and tools in the `schematics-runtime-job` image:

## Terraform-runtime-job image used by {{site.data.keyword.bpshort}} Workspaces
{: #terraform-runtime-job}

The {{site.data.keyword.bpshort}} Workspaces use the following Terraform versions in the `Terraform-runtime-job` image:
-	Terraform 0.13.x, 0.14.x, 0.15.x
-	Terraform 1.0.x, 1.1.x

The current minor version of the Terraform CLI is used in the {{site.data.keyword.bpshort}}.
{: note}

| Terraform helpers | Description | 
| --- | --- |
| `Ansible 2.9.27`| You can use the [ansible-provisioner](https://github.com/radekg/terraform-provisioner-ansible){: external} for Terraform to include Ansible automation alongside your Terraform template. </br>It is better to use the {{site.data.keyword.bpshort}} Actions to run your Ansible automation. |
| `{{site.data.keyword.cloud_notm}} CLI` |{{site.data.keyword.cloud_notm}} CLI has all the installed plug-in, for example, [{{site.data.keyword.bpshort}} current CLI version plug-in](/docs/schematics?topic=schematics-cli_version-releases). You can use the {{site.data.keyword.cloud_notm}} CLI from within the Terraform automation. </br>It is better to use the Terraform resources from {{site.data.keyword.cloud_notm}}, instead of writing scripts by using [{{site.data.keyword.cloud_notm}} CLI release](https://github.com/IBM-Cloud/ibm-cloud-cli-release/releases){: external}.|
| `JQ v1.6` | You can use the [JSON processor](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_jq) in your Terraform automation. |
| `kubectl` | You can use the Kubernetes command-line interface to work with your [Kubernetes](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_kubectl) clusters. |
| `OpenShift client` | You can use {{site.data.keyword.redhat_openshift_notm}} command-line interface to work with your [{{site.data.keyword.redhat_openshift_notm}} clusters](/docs/openshift?topic=openshift-access_cluster).</br> It is better to use the Terraform resources for [{{site.data.keyword.cloud_notm}}, instead of writing scripts by using {{site.data.keyword.cloud_notm}} CLI. |
| `Python v3.6` | You can use [Python 3](https://github.com/IBM/schematics-python-sdk){: external} and higher with the `netaddr` libraries in your Terraform-runtime automation job.|
{: caption="Helpers in terraform-runtime-job" image caption-side="top"}

## Terraform-runtime-agent-job image used by {{site.data.keyword.bpshort}} Agents
{: #terraform-runtime-agent-job}

The {{site.data.keyword.bpshort}} Agents use `Terraform v1.0.x` and `Terraform v1.1.x` versions in the `Terraform-runtime-agent-job` image:

## Ansible-runtime-job image used by {{site.data.keyword.bpshort}} Actions
{: #Ansible-runtime-job}

The {{site.data.keyword.bpshort}} Actions use the `Ansible v2.9.27` in the `Ansible-runtime-job` image:

The current minor version of the Ansible CLI is used in the {{site.data.keyword.bpshort}}.
{: note}

| Terraform helpers | Description | 
| --- | --- |
| `{{site.data.keyword.cloud_notm}} CLI` | You can use the {{site.data.keyword.cloud_notm}} CLI from the Ansible automation.|
| `JQ v1.6` | You can use the [JSON processor](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_jq) in your Ansible automation. |
| `kubectl` | You can use the Kubernetes command-line interface to work with your [Kubernetes](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_kubectl) clusters. |
| `OpenShift client` | You can use {{site.data.keyword.redhat_openshift_notm}} command-line interface to work with your [{{site.data.keyword.redhat_openshift_notm}} clusters](/docs/openshift?topic=openshift-access_cluster). |
| `Python v3.8` | You can use [Python 3](https://github.com/IBM/schematics-python-sdk){: external} and the following libraries, in your runtime automation job.</br> * `netaddr` </br>* `kubernetes` </br>* `openshift` </br>* `pywinrm` </br>* `boto3` </br>* `boto` </br>* `botocore` </br>* `PyVmomi` |
{: caption="Helpers in ansible-runtime-job" image caption-side="top"}


