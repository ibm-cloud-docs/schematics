---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-18"

keywords: schematics utilities, commands and utilities, utilities, jobs

subcollection: schematics

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"} 
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}


# Scripting utilities to automate
{: #scripting-tool}

{{site.data.keyword.bpshort}} contains workspaces, actions, Terraform, key management system, activity tracker, and events functions to ease the infrastructure setup in the {{site.data.keyword.cloud_notm}}. You can use following executables tool for automation scripts in {{site.data.keyword.bpshort}}.
{: shortdesc}

## Utilities
{: #utilities}

Schematics functions are build on UBI-8 and the runtimes that come with the UBI-8 are available in Schematics workspace and actions. [Universal Base Image (UBI)-8](/docs/RegistryImages?topic=RegistryImages-ibmliberty#ibmliberty_get_started) is a base of Red Hat Enterprise Linux 8, a docker container for Ansible playbook and {{site.data.keyword.bpshort}} workspace. The following table describes the utilities that are used in {{site.data.keyword.bpshort}} functions and an automation scripts:

|Utilities | Description | 
|---------|----------|
| `Python 3` | Schematics uses [Python 3](/docs/cli?topic=cli-enable-existing-python) and above to analyze and organize the data, and to automate DevOps. | 
| `Python 3 - pip` |Standard package manager for Python. It allows you to install and manage modules that are not part of the [Python standard library](/docs/ai-openscale-icp?topic=ai-openscale-icp-crt-ov#in-pyc). For example, `netaddr`, `kubernetes`, `OpenShift`, `ibm-cloud-sdk-core`, `ibm-vpc`.| 
| `OpenShift client` |A [service built docker containers](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_oc) orchestrated and managed by Kubernetes on a foundation of Red Hat Enterprise Linux.| 
| `Ansible 2.9.7`| Schematics uses [Ansible v2.9.7](/docs/cloud-pak-multicloud-management?topic=cloud-pak-multicloud-management-ansible-getting-started) and above. |
| `Terraform v11 and v12`|   Automates your resource provisioning. [Terraform v11, v12, and v13](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started) to use the modules and resource in Schematics workspace. Actions support Terraform v11 and v12. |
| `kubectl`| A command line interface for running commands against the [Kubernetes](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_kubectl) clusters.|
| `IBM Cloud CLI v.1.2.0`| The [command line](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_cli). to interact with IBM Cloud API.|
| `JQ 1.6`| A lightweight and flexible command-line [JSON processor](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_jq).|
| `Helm` |A chart to define, install, and upgrade the most complex [Kubernetes application](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_helm)|
{: caption="Utilities to create script for automation" caption-side="top"}


To avoid the installation of these tools, you can also use the [Cloud Shell](https://cloud.ibm.com/shell) from the IBM Cloud console.
{: tip}

For more information, about to use these tools in multiple Operating System, refer to [Solution tutorials](/docs/solution-tutorials?topic=solution-tutorials-tutorials).
{: important}
