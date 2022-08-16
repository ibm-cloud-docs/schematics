---

copyright: 
  years: 2017, 2022
lastupdated: "2022-08-16"

keywords: tools and utilities, utilities, tools, runtime tools, schematics tools, schematics utilities

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} runtime tools
{: #sch-utilities}

Your automation templates are run by {{site.data.keyword.bpshort}}, in a Kubernetes cluster by using a `schematics-runtime-job` image. The `schematics-runtime-job` image embeds the primary Infrastructure as Code (IaC) automation engine, for example, Terraform CLI, Ansible. The `schematics-runtime-job` image also includes additional helper software, and tools that are useful while developing an automation.

You cannot install the software or tool in the {{site.data.keyword.bpshort}} runtime. Any software installation in `schematics-runtime-job` pod is considered as vulnerable.
{: note}

The `schematics-runtime-job` image is built by using the [Universal Base Image (UBI-8)](https://catalog.redhat.com/software/containers/ubi8/ubi/5c359854d70cc534b3a3784e){: external}. The following table lists the pre-installed helper software and tools in the `schematics-runtime-job` image:

## Terraform-runtime-job image used by {{site.data.keyword.bpshort}} Workspaces
{: #terraform-runtime-job}

The {{site.data.keyword.bpshort}} Workspaces uses the following Terraform versions in the `Terraform-runtime-job` image:
-	Terraform 0.13.x, 0.14.x, 0.15.x
-	Terraform 1.0.x, 1.1.x

The latest minor version of the Terraform CLI is used in the {{site.data.keyword.bpshort}}.
{: note}

| Terraform helpers | Description | 
| --- | --- |
| `Ansible 2.9.27`| You can use the [ansible-provisioner](https://github.com/radekg/terraform-provisioner-ansible){: external} for Terraform to include Ansible automation alongside your Terraform template. </br>It is recommended to use the {{site.data.keyword.bpshort}} Actions, to run your Ansible automation. |
| `{{site.data.keyword.cloud_notm}} CLI` | You can use the {{site.data.keyword.cloud_notm}} CLI from within the Terraform automation. </br>It is recommended to use the Terraform resources from {{site.data.keyword.cloud_notm}}, instead of writing scripts by using [{{site.data.keyword.cloud_notm}} CLI release](https://github.com/IBM-Cloud/ibm-cloud-cli-release/releases/tag/v2.9.0){: external}. |
| `JQ v1.6` | You can use the [JSON processor](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_jq) in your Terraform automation. |
| `kubectl` | You can use the Kubernetes command-line interface to work with your [Kubernetes](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_kubectl) clusters. |
| `OpenShift client` | You can use {{site.data.keyword.redhat_openshift_notm}} command-line interface to work with your [{{site.data.keyword.redhat_openshift_notm}} clusters](/docs/openshift?topic=openshift-access_cluster).</br>It is recommended to use the Terraform resources for [{{site.data.keyword.cloud_notm}}, instead of writing scripts using {{site.data.keyword.cloud_notm}} CLI. |
| `Python 3.6` | You can use [Python 3](/docs/cli?topic=cli-enable-existing-python) and above with the `netaddr` libraries in your Terraform automation.|
{: caption="Helpers in terraform-runtime-job" image caption-side="top"}

## Terraform-runtime-agent-job image used by {{site.data.keyword.bpshort}} Agents
{: #terraform-runtime-agent-job}

The {{site.data.keyword.bpshort}} Agents uses `Terraform v1.0.x` and `Terraform v1.1.x` versions in the `Terraform-runtime-agent-job` image:

## Ansible-runtime-job image used by {{site.data.keyword.bpshort}} Actions
{: #Ansible-runtime-job}

The {{site.data.keyword.bpshort}} Actions uses the `Ansible v2.9.27` in the `Ansible-runtime-job` image:

The latest minor version of the Ansible CLI is used in the {{site.data.keyword.bpshort}}.
{: note}

| Terraform helpers | Description | 
| --- | --- |
| `{{site.data.keyword.cloud_notm}} CLI` | You can use the {{site.data.keyword.cloud_notm}} CLI from the Ansbile automation.|
| `JQ v1.6` | You can use the [JSON processor](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_jq) in your Ansible automation. |
| `kubectl` | You can use the Kubernetes command-line interface to work with your [Kubernetes](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_kubectl) clusters. |
| `OpenShift client` | You can use {{site.data.keyword.redhat_openshift_notm}} command-line interface to work with your [{{site.data.keyword.redhat_openshift_notm}} clusters](/docs/openshift?topic=openshift-access_cluster). |
| `Python 3.6` | You can use [Python 3](/docs/cli?topic=cli-enable-existing-python) and the following libraries, in your Ansible automation. </br> * netaddr </br>* kubernetes </br>* openshift </br>* pywinrm </br>* boto3 </br>* boto </br>* botocore </br>* PyVmomi |
{: caption="Helpers in ansible-runtime-job" image caption-side="top"}

To avoid the installation of these tools, you can also use the [Cloud Shell](https://cloud.ibm.com/shell) from the {{site.data.keyword.cloud_notm}} console.
{: tip}
