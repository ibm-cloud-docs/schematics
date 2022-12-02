---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-02"

keywords: schematics, automation, terraform

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Managing user access
{: #access}

Use [{{site.data.keyword.iamlong}}](/docs/account?topic=account-iamoverview) to grant permissions to {{site.data.keyword.bpshort}} Workspaces and actions. 
{: shortdesc}

As the {{site.data.keyword.cloud_notm}} account owner, you want to ensure that you control user access to workspaces and the actions in your account. {{site.data.keyword.bplong_notm}} integrate with {{site.data.keyword.iamlong}} (IAM) to securely authenticate users for platform services and control access to resources. IAM uses the concept of resource groups, access groups, roles, and access policies to manage the access to {{site.data.keyword.cloud}} resources. For more information about how IAM works and how you can use resource groups, access groups, and access policies to organize {{site.data.keyword.bpshort}} access for a team, see [What is {{site.data.keyword.iamlong}}?](/docs/account?topic=account-iamoverview)



## Overview of {{site.data.keyword.bpshort}} service access roles and required permissions
{: #access-roles}
{: help}
{: support}

Grant access to {{site.data.keyword.bplong_notm}} by assigning {{site.data.keyword.iamlong}} (IAM) service access roles to your users. 
{: shortdesc}

**Who must grant access to {{site.data.keyword.bplong_notm}}?**

As the account owner or an authorized account administrator, you can assign IAM service access roles to your users. The IAM service access roles determine the actions that you can perform on an {{site.data.keyword.bplong_notm}} resource, such as a workspace or an action. To avoid assigning access policies to individual users, consider creating [IAM access groups](/docs/account?topic=account-groups).

**Is access to {{site.data.keyword.bplong_notm}} sufficient to manage {{site.data.keyword.cloud_notm}} resources?**

No. If you are assigned an {{site.data.keyword.bplong_notm}} service access role, you can view, create, update, or delete workspaces and actions in {{site.data.keyword.bplong_notm}}. However, to manage other {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}, you must be assigned the IAM platform or service access role for the individual {{site.data.keyword.cloud_notm}} resource that you want to work with. see the [documentation](/docs/home/alldocs) for your resource to determine the access policies that you need to work with your resource.

### Workspace permissions
{: #workspace-permissions}

Review the following table to see what permissions you need to work with {{site.data.keyword.bpshort}} Workspaces.

| Activities | Reader | Writer | Manager | Account owner |
|-----|-----|-----|-----|--------|
| `View workspace` | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `View workspace activities` | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `View workspace logs` | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `Create workspace` | | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `Update workspace` | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `Delete workspace` | | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `Freeze and unfreeze workspace` | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `View the readme of a template` | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg)|![Check mark icon.](images/checkmark.svg)|![Check mark icon.](images/checkmark.svg)|
| `Create Terraform execution plan` | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `Apply a Terraform template` | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `Destroy workspace resources` | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
{: row-headers}
{: class="comparison-table"}
{: caption="User permissions for {{site.data.keyword.bpshort}} Workspaces" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the access role in column one, and the permission descriptions in column two."}

### Action permissions
{: #action-permissions}

Review the following table to see what permissions you need to work with {{site.data.keyword.bpshort}} Actions.

| Activities | Reader | Writer | Manager | Account owner |
|-----|-----|-----|-----|--------|
| `View action` | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | 
| `View action jobs` | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | 
| `View job logs` | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | 
| `Create action` | | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | 
| `Update action` | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | 
| `Delete action` | | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | 
| `Run check action job` | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | 
| `Run an action` | | ![Check mark icon.](images/checkmark.svg)|![Check mark icon.](images/checkmark.svg)|![Check mark icon.](images/checkmark.svg)| 
{: row-headers}
{: class="comparison-table"}
{: caption="User permissions for {{site.data.keyword.bpshort}} Actions" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the access role in column one, and the permission descriptions in column two."}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

### Blueprint permissions
{: #blueprint-permissions}

Review the following table to see what permissions you need to work with {{site.data.keyword.bpshort}} Blueprint.

In addition to the listed blueprints activities and permission, you must check whether you have related [workspace permissions](#workspace-permissions) for `blueprint config create`, `blueprint run apply`, `blueprint config delete`, and `blueprint run destroy` activities to execute successfully.
{: important} 

| Activities | Reader | Writer | Manager | Account owner |
|-----|-----|-----|-----|--------|
| `View blueprint` | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `View blueprint logs` | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `blueprint config create` | | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `blueprint config update` | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `blueprint config delete`| | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `blueprint run apply` | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
| `blueprint run destroy` | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) |
{: row-headers}
{: class="comparison-table"}
{: caption="User permissions for {{site.data.keyword.bpshort}} Blueprint" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the access role in column one, and the permission descriptions in column two."}

### KMS permissions
{: #kms-permissions}

Review the following table to see what permissions you need to work with {{site.data.keyword.bpshort}} key management system.

| Activities | Reader | Writer | Manager | Account owner |
|-----|-----|-----|-----|--------|
| `View KMS instances` | ![Check mark icon.](images/checkmark.svg)| ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg)|
| `Read KMS settings` | ![Check mark icon.](images/checkmark.svg)| ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg)|
| `Update the KMS settings` | | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg) | ![Check mark icon.](images/checkmark.svg)|
{: row-headers}
{: class="comparison-table"}
{: caption="User permissions for {{site.data.keyword.bpshort}} KMS" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the access role in column one, and the permission descriptions in column two."}

## Setting up access for your users
{: #access-setup}

As the {{site.data.keyword.cloud_notm}} account owner or authorized account administrator. Create an IAM access group for your users and assign service access policies to {{site.data.keyword.bplong_notm}} and the resources that you want your users to work with. 
{: shortdesc}

1. [Invite users to your {{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-iamuserinv).

2. Define your teams and [create an IAM access group](/docs/account?topic=account-groups#create_ag) for each team. 

3. [Create a resource group](/docs/account?topic=account-rgs#create_rgs) for each teams. So that you can organize access to their {{site.data.keyword.cloud_notm}} services and workspaces in your account, and bundle them under one common view and billing process. If you want to keep your {{site.data.keyword.bpshort}} Workspaces and actions separate from the {{site.data.keyword.cloud_notm}} resources, you must create multiple resource groups. 

4. [Assign access to your IAM access group](/docs/account?topic=account-groups#access_ag). Consider the following guidelines when you assign access to an IAM access group: 
    - Make sure to scope access of your group to the resource group that you created for this team. 
    - If you want your team to have access to multiple resource groups, such as the **Administrator** and **Manager** permissions on all resources in resource group A, but **Viewer** access for the resources in resource group B, you must create multiple access policies. 
    - The resource group of the {{site.data.keyword.bpshort}} Workspaces or action can be different from the resource group of the {{site.data.keyword.cloud_notm}} resources that you want to work with.
    - For a team to use {{site.data.keyword.bpshort}}, you must assign the appropriate [service access role for {{site.data.keyword.bpshort}}](#access-roles), and the permissions that are required for the {{site.data.keyword.cloud_notm}} resources that this team manages with {{site.data.keyword.bpshort}}. You can review the [documentation](/docs/home/alldocs) for each of the {{site.data.keyword.cloud_notm}} services to find the appropriate IAM access policy. 


## Manage access tag in your account 
{: #access-tag}

You can now centrally manage access tags to the {{site.data.keyword.bpshort}} Workspaces in your account at scale. Tags contains the metadata values in the form of key and value to help you organize your cloud data. Tags are essential, as it helps to efficiently optimize your workspace within your account. Following steps helps to create and associate access tags for {{site.data.keyword.bpshort}} Workspaces in your account.

- To create an access tag, see [Create an access management tag](/docs/account?topic=account-access-tags-tutorial#tagging-resources-create). 
- To associate access tags, see [Attach your access management tag to a {{site.data.keyword.bpshort}} Workspaces](/docs/account?topic=account-access-tags-tutorial#tagging-resources-add)

For more information about managing access tags, see [Controlling access to resources by using tags](/docs/account?topic=account-access-tags-tutorial).







