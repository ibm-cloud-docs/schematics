---

copyright:
  years: 2020, 2022
lastupdated: "2022-07-08"

keywords: schematics, hybrid, multicloud, RACI

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Shared responsibilities for using {{site.data.keyword.bplong_notm}}
{: #sc-responsibilities}

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.bplong}}. For a high-level view of the service types in {{site.data.keyword.cloud_notm}} and the breakdown of responsibilities between you as the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{: shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.bplong_notm}}. For the overall terms of use, see [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms). For responsibilities that you have for other {{site.data.keyword.cloud_notm}} services that you use with {{site.data.keyword.bpshort}}, refer to the documentation of those services, such as [{{site.data.keyword.openshiftlong_notm}} responsibilities](/docs/openshift?topic=openshift-responsibilities_iks).

## Managed products
{: #managed-responsibilities}

Products that are managed by {{site.data.keyword.IBM_notm}} require customer responsibilities only for the data or applications that customers add to the service. They are multi-tenant, accessed remotely, hosted on {{site.data.keyword.IBM_notm}} virtual resources, created in {{site.data.keyword.IBM_notm}}-owned accounts, and have control plane and data plane security that is owned by {{site.data.keyword.IBM_notm}}. Examples of this product type are {{site.data.keyword.cloud_notm}} databases or {{site.data.keyword.cloudant_short_notm}} database instances. You can find a list of these types of products in the {{site.data.keyword.cloud_notm}} catalog on the Services tab. However, any products that are listed in an infrastructure subcategory are infrastructure-as-a-service type products. 

| Resource | [Incident and Operations Management](#incident-ops-mgt) | [Change Management](#change-management)| [Identity and Access Management](#iam) | [Security and Regulation Compliance](#responsibilities-security) | [Disaster Recovery](#disaster-recovery) |
| - | - | - | - | - | - |
| Data |Customer  | Customer | Customer | Customer | Customer | 
| Application | Customer | Customer | Customer | Customer | Customer |
| Service instance | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | Shared |
| Virtual storage | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
| Virtual network | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
| Physical servers and memory | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
| Physical storage | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
| Physical network and devices | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
| Facilities and data centers | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
{: caption="Shared responsibilities for fully-managed products" caption-side="top"}

For disaster recovery, {{site.data.keyword.IBM_notm}} is responsible to ensure that other regions that are not impacted by the disaster are fully operational and will recover the impacted region by the disaster as quickly as possible.
{: note}

## Managed products for Agents
{: #managed-offerings-on-customers-resources-responsibilities}

Managed products on {{site.data.keyword.bpshort}} Agents resources are orchestrated by {{site.data.keyword.IBM_notm}}. They are single-tenant and data plane products. They are accessed locally in customer accounts, data plane hosted on virtual resources in the customer's account, control plane security owned by {{site.data.keyword.IBM_notm}}, and data plane security owned by the customer. {{site.data.keyword.cloud_notm}} products of this type include {{site.data.keyword.containerlong_notm}} on classic infrastructure.

| Resource | [Incident and Operations Management](#incident-ops-mgt) | [Change Management](#change-management) | [Identity and Access Management](#iam) | [Security and Regulation Compliance](#responsibilities-security) | [Disaster Recovery](#disaster-recovery) |
| - | - | - | - | - | - |
| Data | Customer | Customer | Customer | Customer | Customer |
| Runtime | Customer | Customer | Customer | Customer | Customer |
| Service instance | Shared | Shared | Shared | Shared | Shared |
| Operating system | Shared | Shared | Shared | Shared | Shared |
| Virtual and bare metal servers | Shared | Shared | Shared | Shared | Shared |
| Virtual storage | Shared | Shared | Shared | Shared | Shared |
| Virtual network | Shared | Shared | Shared | Shared | Shared |
| Physical servers and memory | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
| Physical storage | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
| Physical network and devices | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
| Facilities and data centers | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} | {{site.data.keyword.IBM_notm}} |
{: caption="Shared responsibilities for {{site.data.keyword.bpshort}} Agents " caption-side="top"}

For areas marked as shared responsibilities, the customer is responsible for all the configurations, and {{site.data.keyword.IBM_notm}} is responsible for all underlying management. For disaster recovery, the customer is responsible for creating resources in a secondary region and managing the application and data disaster recovery.
{: note}

## Security
{: #responsibilities-security}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
| General | * Provide the tools to lock a workspace and disable changes for a workspace. </br> * Automatically apply security and version updates to sample templates and the templates in the {{site.data.keyword.cloud_notm}} catalog. </br> * Update operational {{site.data.keyword.bplong_notm}} components, such as the {{site.data.keyword.terraform-provider_full_notm}}, the Terraform command-line version, and other supported providers and provisioning engines. </br> * Provide access to {{site.data.keyword.bpshort}} log information for resource-related actions. </br> * Track workspace activities and automatically send events to {{site.data.keyword.at_full_notm}}. </br> * Encrypt workspace data in transit and at rest.  | * Use {{site.data.keyword.iamshort}} to control access to a {{site.data.keyword.bpshort}} Workspaces and related {{site.data.keyword.cloud_notm}} resources. </br> * Secure the source repository for your Terraform template, including access control, security settings, collaboration, and version control. </br> * Secure the {{site.data.keyword.cloud_notm}} resources that you create by using the security features that are provided by the resource offering. </br> * Use the provided tools of your {{site.data.keyword.cloud_notm}} resources to apply security patches, access controls, and encryption to your resources. |
{: caption="Overview of security." caption-side="bottom"}

## Resource provisioning and change management 
{: #change-management}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
| General | * Provide a suite of tools to automate resource provisioning and resource management in {{site.data.keyword.cloud_notm}}, such as the {{site.data.keyword.bplong_notm}} console, CLI, and API. </br> * Provide and maintain sample Terraform templates. </br> * Create, update, and manage the Terraform state file (`terraform.tfstate`) to determine the required actions to achieve the required state that is described in your Terraform template.  | * Create your own Terraform templates or use one of the provided sample templates to configure the resources that you want. </br> * Design the source repository structure, and set up and manage the source repository for your Terraform template. </br> * Understand the {{site.data.keyword.cloud}} resource offerings that are used in your templates, including availability, pricing, limitations, and the tools to set up security, high availability, and logging and monitoring. </br> * Design and provision your resources in a way that achieves high availability and resiliency, such as [multizone clusters](/docs/containers?topic=containers-ha_clusters#multizone) or [multi-region VPCs](/docs/vpc?topic=solution-tutorials-vpc-multi-region#vpc-multi-region). </br> * Use the provided tools to [provision](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources), [modify](/docs/schematics?topic=schematics-manage-lifecycle#update-resources), or [delete](/docs/schematics?topic=schematics-manage-lifecycle#destroy-resources) your {{site.data.keyword.cloud_notm}} resources. </br> * Set up logging and monitoring for your resources to track resource activity and monitor the resource health, such as with [{{site.data.keyword.la_full_notm}}](/docs/log-analysis?topic=log-analysis-getting-started) and [{{site.data.keyword.mon_full_notm}}](/docs/monitoring?topic=monitoring-getting-started).|
{: caption="Overview of change management." caption-side="bottom"}

## High availability, backup, and disaster recovery
{: #disaster-recovery}

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
| General | Back up the workspace data across {{site.data.keyword.cloud_notm}} regions. Recover workspace data from the replicated region within 24 hours. Recover operational {{site.data.keyword.bplong_notm}} components. | Design and provision your resources in a way that achieves high availability and resiliency, such as [multizone clusters](/docs/containers?topic=containers-ha_clusters#multizone) or [multi-region VPCs](/docs/vpc?topic=solution-tutorials-vpc-multi-region#vpc-multi-region). Set up a backup and recovery strategy for your resources and your workload data. |
{: caption="Overview of disaster recovery." caption-side="bottom"}


## Identity and access management
{: #iam}

Identity and access management includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access. For more information, see [Managing user access](/docs/schematics?topic=schematics-access).

Create an IAM access group for your users and assign service access policies to {{site.data.keyword.bplong_notm}} and the resources. Refer to, [Setting up an access for the users](/docs/schematics?topic=schematics-access#access-setup).

## Incident and Operations Management 
{: #incident-ops-mgt}

Incident and operations management includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.

| Task | {{site.data.keyword.IBM_notm}} responsibilities | Your responsibilities |
|----------|-----------------------|--------|
| General | * Provide customer support for {{site.data.keyword.bpshort}}. </br> * Provide [customer support plans](/docs/get-support?topic=get-support-support-plans) to help resolve problems that you might encounter. | * Procure the underlying compute, network, and storage infrastructure in your own environments that {{site.data.keyword.bpshort}} uses to extend {{site.data.keyword.cloud_notm}} to these environments. |
{: caption="Responsibilities of incident and operations." caption-side="bottom"}
