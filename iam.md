---

copyright:
  years: 2017, 2024
lastupdated: "2024-08-29"

keywords: schematics, automation, terraform

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Managing user access
{: #access}

Use [{{site.data.keyword.iamlong}}](/docs/account?topic=account-iamoverview) to grant permissions to {{site.data.keyword.bpshort}} workspaces and actions.
{: shortdesc}

As the {{site.data.keyword.cloud}} account owner, you need to ensure that you control user access to {{site.data.keyword.bpshort}} workspaces and the actions in your account. {{site.data.keyword.bplong_notm}} integrate with {{site.data.keyword.iamlong}} (IAM) to securely authenticate users for platform services and control access to the resources. IAM uses the concept of resource groups, access groups, roles, and access policies to manage the access to {{site.data.keyword.cloud}} resources. For more information about how IAM works and how you can use resource groups, access groups, and access policies to organize {{site.data.keyword.bpshort}} access for a team, see [What is {{site.data.keyword.iamlong}}?](/docs/account?topic=account-iamoverview)

## Overview of {{site.data.keyword.bpshort}} service access roles and required permissions
{: #access-roles}
{: help}
{: support}

Grant access to {{site.data.keyword.bplong_notm}} by assigning {{site.data.keyword.iamlong}} (IAM) service access roles to your users. 
{: shortdesc}

Who must grant access to {{site.data.keyword.bplong_notm}}?

As the account owner or an authorized account administrator, you can assign IAM service access roles to your users. The IAM service access roles determine the actions that you can perform on an {{site.data.keyword.bplong_notm}} resource, such as a workspace or an action. To avoid assigning access policies to individual users, consider creating [IAM access groups](/docs/account?topic=account-groups).

Is access to {{site.data.keyword.bplong_notm}} sufficient to manage {{site.data.keyword.cloud_notm}} resources?

No. If you are assigned an {{site.data.keyword.bplong_notm}} service access role, you can view, create, update, or delete workspaces and actions in {{site.data.keyword.bplong_notm}}. However, to manage other {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}, you must be assigned the IAM platform or service access role for the individual {{site.data.keyword.cloud_notm}} resource that you want to work with. see the [documentation](/docs/home/alldocs) for your resource to determine the access policies that you need to work with your resource.

## {{site.data.keyword.bpshort}} Platform roles and service roles
{: #iam-platform-svc-roles}

The user roles exist at both the platform (account) and service level. If you are unsure about what a platform or a service role allows a user to do remember that platform roles interact mainly with {{site.data.keyword.cloud_notm}} services like the [resource controller](/docs/account?topic=account-overview) or {{site.data.keyword.iamshort}}. Roles inside of a service, on the other hand, interact mainly with the relevant API, which in this case is the {{site.data.keyword.bpshort}} API.

### Platform roles
{: #iam-platform-roles}

Platform roles be assigned over an entire account, over particular service instances, or within objects inside of a service instance.

* **Administrator**: Has the full spectrum of rights over a particular action and its child actions. It includes to invite new users and assign roles over the object (only administrators can assign roles). Note that administrators do not have service roles by default. They can, however, assign roles to themselves.
* **Editor**: Can view, create, and delete instances at the account level, except invite new users, manage the account, and assign access policies. Has limited use for actions within a service instance, beyond the ability to view them.
* **Operator**: Can view instances at the account level, but cannot edit them. Has limited use for actions within a service instance, beyond the ability to view them.
* **Viewer**: Can view instances at the account level, but cannot edit them. Has limited use for actions within a service instance, beyond the ability to view them.

### Service roles
{: #iam-service-roles}

While an account-level role gives a user particular permissions over service instances by default, roles can also be assigned over a particular service instance. Service roles can be applied to the three first class objects within a service instance: the **instance** as a whole, particular **workspace**, and **action**. However, these permissions can be assigned more granularly where necessary. For example giving a user the _Manager_ role over only a particular workspace or resource and some lesser level of permission over the instance as a whole.

Service roles can be assigned per-instance or for all instances in an account.
{: note}

* **Reader**: You can perform read-only actions within a service such as viewing service-specific resources. For example, read the action definition, KMS settings, workspace details, agent configuration settings, and so on.
* **Writer**: You can perform create, edit, and read service specific resources operation. For example, create and update workspace, action, agent, and so on.
* **Manager**: In addition to writer access, you have complete privilege as defined by the service. A _Manager_, for example, has all the permissions that a _Reader_ has and more.

## Roles and permissions about {{site.data.keyword.bpshort}} offerings 
{: #iam-roles-permission-schematics}

The list provides the details about the roles and permission needed for the [{{site.data.keyword.bpshort}} offerings](/docs/schematics?topic=schematics-learn-about-schematics#sc-offerings).

### Workspace permissions
{: #workspace-permissions}

Review the following table to see what permissions you need to work with {{site.data.keyword.bpshort}} workspaces.

| Activities | Reader | Writer | Manager | Account owner |
|-----|-----|-----|-----|--------|
| `View workspace` | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `View workspace activities` | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `View workspace logs` | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `Create workspace` | | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `Update workspace` | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `Delete workspace` | | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `Freeze and unfreeze workspace` | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `View the readme of a template` | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg)|![Check mark](images/checkmark.svg)|![Check mark](images/checkmark.svg)|
| `Create Terraform execution plan` | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `Apply a Terraform template` | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `Destroy workspace resources` | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
{: row-headers}
{: class="comparison-table"}
{: caption="User permissions for {{site.data.keyword.bpshort}} workspaces" caption-side="top"}

### Action permissions
{: #action-permissions}

Review the following table to see what permissions you need to work with {{site.data.keyword.bpshort}} actions.

| Activities | Reader | Writer | Manager | Account owner |
|-----|-----|-----|-----|--------|
| `View action` | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | 
| `View action jobs` | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | 
| `View job logs` | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | 
| `Create action` | | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | 
| `Update action` | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | 
| `Delete action` | | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | 
| `Run check action job` | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | 
| `Run an action` | | ![Check mark](images/checkmark.svg)|![Check mark](images/checkmark.svg)|![Check mark](images/checkmark.svg)| 
{: row-headers}
{: class="comparison-table"}
{: caption="User permissions for {{site.data.keyword.bpshort}} actions" caption-side="top"}

### Agent permissions
{: #agent-permissions}

The following are the different permissions that you an user need to create and deploy  the {{site.data.keyword.bpshort}} agent.

* Permission to deploy an agent
* Permission for agent to connect with {{site.data.keyword.bpshort}}
* Permission to users to manage agents

#### Permission to deploy an agent
{: #agent-deploy-permission}

Agent recommends to use a service ID and API key to provision the prerequisite the {{site.data.keyword.cloud_notm}} resources such as {{site.data.keyword.containerlong_notm}} or {{site.data.keyword.redhat_openshift_notm}}, {{site.data.keyword.cos_full_notm}}, and {{site.data.keyword.cos_full_notm}} bucket.

Following are the maximum permission and roles that services should have to deploy an agent.

| Resources | Service role | Platform role |
| --- | --- | --- |
| `{{site.data.keyword.containerlong_notm}}` | Manager | Viewer |
| `Resource Group` |  | Administrator |
| `{{site.data.keyword.redhat_openshift_notm}}` or `{{site.data.keyword.containershort_notm}}`| Object Writer | Administrator |
| `{{site.data.keyword.cos_full_notm}}` | Object Writer ++ | Administrator ++ |
| `{{site.data.keyword.cos_full_notm}} bucket` | Object Writer + Writer | Administrator |
| `{{site.data.keyword.bpshort}}` | Manager | Operator |
{: caption="Permissions to deploy an agent" caption-side="top"}

#### Permission for agent to connect with {{site.data.keyword.bpshort}}
{: #agent-schematics-connect}

Consider the following access are provided for an agent to connect with {{site.data.keyword.bpshort}}.

* You need administrator permission to access the resources such as {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.redhat_openshift_notm}}, {{site.data.keyword.cos_full_notm}}, and so on.
* You need Manager service role access, Operator role permission, and [assign access to the trusted profile](/docs/account?topic=account-create-trusted-profile&interface=ui#tp-access) to connect.

#### Permission for users to manage agents
{: #agent-manage-permission}

Review the following table to see what identity and permissions you need to use the {{site.data.keyword.bpshort}} Agent.

In addition to the listed agent activities and permission, you must ensure you have permissions to run `agent create`, `agent plan`, `agent apply`, `agent delete`, and `agent destroy` activities to execute successfully.
{: important}

| Activities | Reader | Writer | Manager | Account owner |
|-----|-----|-----|-----|--------|
| `View agents` | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `View agent logs` | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `Agent apply` | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `Agent create` | | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `Agent delete`| | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `Agent destroy` | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `Agent plan` | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
| `Agent update` | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) |
{: caption="User permissions for {{site.data.keyword.bpshort}} Agent" caption-side="top"}



### KMS permissions
{: #kms-permissions}

Review the following table to see what permissions you need to work with {{site.data.keyword.bpshort}} key management system.

| Activities | Reader | Writer | Manager | Account owner |
|-----|-----|-----|-----|--------|
| `View KMS instances` | ![Check mark](images/checkmark.svg)| ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg)|
| `Read KMS settings` | ![Check mark](images/checkmark.svg)| ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg)|
| `Update the KMS settings` | | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg) | ![Check mark](images/checkmark.svg)|
{: row-headers}
{: class="comparison-table"}
{: caption="User permissions for {{site.data.keyword.bpshort}} KMS" caption-side="top"}

## Setting up access for your users
{: #access-setup}

As the {{site.data.keyword.cloud_notm}} account owner or authorized account administrator. Create an IAM access group for your users and assign service access policies to {{site.data.keyword.bplong_notm}} and the resources that you want your users to work with. 
{: shortdesc}

1. [Invite users to your {{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-iamuserinv).

2. Define your teams and [create an IAM access group](/docs/account?topic=account-groups&interface=ui#create_ag) for each team.

3. [Create a resource group](/docs/account?topic=account-rgs&interface=ui#create_rgs) for each teams. So that you can organize access to their {{site.data.keyword.cloud_notm}} services and workspaces in your account, and bundle them under one common view and billing process. If you want to keep your {{site.data.keyword.bpshort}} workspaces and actions separate from the {{site.data.keyword.cloud_notm}} resources, you must create multiple resource groups.

4. [Assign access to your IAM access group](/docs/account?topic=account-groups&interface=ui#access_ag). Consider the following guidelines when you assign access to an IAM access group:

    * Make sure to scope access of your group to the resource group that you created for this team.
    * If you want your team to have access to multiple resource groups, such as the **Administrator** and **Manager** permissions on all resources in resource group A, but **Viewer** access for the resources in resource group B, you must create multiple access policies.
    * The resource group of the {{site.data.keyword.bpshort}} workspaces or action can be different from the resource group of the {{site.data.keyword.cloud_notm}} resources that you want to work with.
    * For a team to use {{site.data.keyword.bpshort}}, you must assign the appropriate [service access role for {{site.data.keyword.bpshort}}](#access-roles), and the permissions that are required for the {{site.data.keyword.cloud_notm}} resources that this team manages with {{site.data.keyword.bpshort}}. You can review the [documentation](/docs/home/alldocs) for each of the {{site.data.keyword.cloud_notm}} services to find the appropriate IAM access policy.

## Manage access tag in your account 
{: #access-tag}

You can now centrally manage access tags to the {{site.data.keyword.bpshort}} workspaces in your account at scale. Tags contains the metadata values in the form of key and value to help you organize your cloud data. Tags are essential, as it helps to efficiently optimize your workspace within your account. Following steps helps to create and associate access tags for {{site.data.keyword.bpshort}} workspaces in your account.

* To create an access tag, see [Create an access management tag](/docs/account?topic=account-access-tags-tutorial#tagging-resources-create).
* To associate access tags, see [Attach your access management tag to a {{site.data.keyword.bpshort}} workspaces](/docs/account?topic=account-access-tags-tutorial#tagging-resources-add)

For more information about managing access tags, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).
