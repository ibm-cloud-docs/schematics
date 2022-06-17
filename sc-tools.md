---

copyright: 
  years: 2017, 2021
lastupdated: "2022-06-16"

keywords: tools and utilities, utilities, tools, runtime tools, schematics tools, schematics utilities

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Schematics runtime development tools
{: #sch-utilities}

Schematics runtime is built by using Universal Base Image (UBI-8) and the runtime utilities/softwares that come with the UBI-8 are available for Terraform provisioners and Ansible actions.

{{site.data.keyword.bpshort}} runtime is built by using [Universal Base Image (UBI-8)](/docs/RegistryImages?topic=RegistryImages-ibmliberty#ibmliberty_get_started) and the runtime utilities or softwares that come with the UBI-8 are available in Terraform provisioners and Ansible actions. For more information, to use these tools in multiple Operating System, refer to, [Solution tutorials](/docs/solution-tutorials?topic=solution-tutorials-tutorials).

The following table describes the utilities that are used in {{site.data.keyword.bpshort}} functions and  automation scripts: 

|Utilities | Description | 
|---------|----------|
| `Ansible 2.9.23`| {{site.data.keyword.bpshort}} uses [Ansible v2.9.23](/docs/cloud-pak-multicloud-management?topic=cloud-pak-multicloud-management-ansible-getting-started) and higher. |
| `Helm` |A chart to define, install, and upgrade the most complex [Kubernetes application](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_helm).|
| `{{site.data.keyword.cloud_notm}} CLI v.1.2.0`| The [command-line](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_cli) to interact with {{site.data.keyword.cloud_notm}} API.|
| `JQ 1.6`| A lightweight and flexible command-line [JSON processor](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_jq).|
| `kubectl`| A command-line interface for running commands against the [Kubernetes](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_kubectl) clusters.|
| `OpenShift client` |A [service built docker containers](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_oc) that are orchestrated and managed by Kubernetes on a foundation of Red Hat Enterprise Linux.| 
| `Python 3` | {{site.data.keyword.bpshort}} uses [Python 3](/docs/cli?topic=cli-enable-existing-python) and higher version to analyze and organize the data, and to automate DevOps. | 
| `Python 3 - pip` |Standard package manager for Python. Allows you to manage modules that are not part of the [Python standard library](https://docs.python.org/3/library/){: external}. For example, `netaddr`, `kubernetes`, `OpenShift`, `ibm-cloud-sdk-core`, `ibm-vpc`.| 
| `Terraform v0.12 and higher`|   Automates your resource provisioning. [Terraform v12, and higher](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started) to use the modules and resource in {{site.data.keyword.bpshort}} workspace. |
| `Terraform CLI v1.0.11 and v1.1.5`| {{site.data.keyword.bpshort}} uses [Terraform CLI v1.0.11](https://releases.hashicorp.com/terraform/1.0.11) and [Terraform CLI v1.1.5](https://releases.hashicorp.com/terraform/1.1.5) pre-installed binaries for {{site.data.keyword.bpshort}} Agent feature.|
{: caption="Utilities to create script for automation." caption-side="top"}

To avoid the installation of these tools, you can also use the [Cloud Shell](https://cloud.ibm.com/shell) from the {{site.data.keyword.cloud_notm}} console.
{: tip}
