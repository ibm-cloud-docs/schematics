---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-02"

keywords: schematics responsibilities, schematics high availability, schematics backup, schematics disaster recovery, schematics security, schematics ibm vs user
subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# User responsibilities by using {{site.data.keyword.bplong_notm}}
{: #responsibilities}
{: help}
{: support}

Learn about the responsibilities that you have when you use {{site.data.keyword.bplong_notm}}. 
{: shortdesc}

| Category | Responsibilities |
| -- | -- |
| Resource provisioning and management | **IBM responsibilities:**  \n * Provide a suite of tools to automate resource provisioning and resource management in {{site.data.keyword.cloud_notm}}, such as the {{site.data.keyword.bplong_notm}} console, CLI, and API. \n * Provide and maintain sample Terraform templates. \n * Create, update, and manage the Terraform state file (`terraform.tfstate`) to determine the required actions to achieve the required state that is described in your Terraform template. \n **User responsibility:**  \n * Create your own Terraform templates or use one of the provided sample templates to configure the resources that you want. \n * Design the source repository structure, and set up and manage the source repository for your Terraform template. \n * Understand the {{site.data.keyword.cloud}} resource offerings that are used in your templates, including availability, pricing, limitations, and the tools to set up security, high availability, and logging and monitoring. \n * Design and provision your resources in a way that achieves high availability and resiliency, such as [multizone clusters](/docs/containers?topic=containers-ha_clusters#mz-clusters) or [multi-region VPCs](/docs/vpc?topic=solution-tutorials-vpc-multi-region#vpc-multi-region). \n * Use the provided tools to [provision](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources), [modify](/docs/schematics?topic=schematics-manage-lifecycle#update-resources), or [delete](/docs/schematics?topic=schematics-manage-lifecycle#destroy-resources) your {{site.data.keyword.cloud_notm}} resources. \n * Set up logging and monitoring for your resources to track resource activity and monitor the resource health, such as with [{{site.data.keyword.la_full_notm}}](/docs/log-analysis?topic=log-analysis-getting-started) and [{{site.data.keyword.mon_full_notm}}](/docs/monitoring?topic=monitoring-getting-started). |
| Security | **IBM responsibilities:**  \n * Provide the tools to lock a workspace and disable changes for a workspace. \n * Automatically apply security and version updates to sample templates and the templates in the {{site.data.keyword.cloud_notm}} catalog. \n * Update operational {{site.data.keyword.bplong_notm}} components, such as the {{site.data.keyword.terraform-provider_full_notm}}, the Terraform command-line version, and other supported providers and provisioning engines. \n * Provide access to {{site.data.keyword.bpshort}} log information for resource-related actions. \n * Track workspace activities and automatically send events to {{site.data.keyword.at_full_notm}}. \n * Encrypt workspace data in transit and at rest. \n **User responsibility:**  \n * Use {{site.data.keyword.iamshort}} to control access to a {{site.data.keyword.bpshort}} Workspaces and related {{site.data.keyword.cloud_notm}} resources. \n * Secure the source repository for your Terraform template, including access control, security settings, collaboration, and version control. \n * Secure the {{site.data.keyword.cloud_notm}} resources that you create by using the security features that are provided by the resource offering. \n * Use the provided tools of your {{site.data.keyword.cloud_notm}} resources to apply security patches, access controls, and encryption to your resources. |
| High availability, backup, and disaster recovery | **IBM responsibilities:**  \n * Back up workspace data across {{site.data.keyword.cloud_notm}} regions. \n * Recover workspace data from the replicated region within 24 hours. \n * Recover operational {{site.data.keyword.bplong_notm}} components. \n **User responsibility:**  \n * Design and provision your resources in a way that achieves high availability and resiliency, such as [multizone clusters](/docs/containers?topic=containers-ha_clusters#mz-clusters) or [multi-region VPCs](/docs/vpc?topic=solution-tutorials-vpc-multi-region#vpc-multi-region). \n * Set up a backup and recovery strategy for your resources and your workload data. |
{: caption="High availability for {{site.data.keyword.cloud_notm}} resources" caption-side="bottom"}
