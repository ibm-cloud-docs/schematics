---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-11"

keywords: schematics, automation, terraform

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
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# Managing user access
{: #access}

Use {{site.data.keyword.cloud}} Identity and Access Management (IAM) to grant permissions to {{site.data.keyword.bplong_notm}} workspaces and actions. 
{: shortdesc}

## Using IAM access groups and resource groups to organize access to {{site.data.keyword.bplong_notm}} resources
{: #team-org}

Learn how you can organize and simplify access to your workspaces and actions by leveraging IAM access groups and {{site.data.keyword.cloud_notm}} resource groups. 
{: shortdesc}

As the {{site.data.keyword.cloud_notm}} account owner, you want to ensure that you control user access to the {{site.data.keyword.bplong_notm}} workspaces and the actions in your account. 

**What is a resource group and how does it help me organize my team?**<br>
Assigning access to a particular {{site.data.keyword.cloud_notm}} service is a good way of allowing a user to work with a specific service in your account. However, when you build production workloads in the cloud, you most likely have multiple {{site.data.keyword.cloud_notm}} services and resources that are used by different teams. With resource groups, you can organize multiple services in your account and bundle them under one common view and billing process. To allow your team to work with these resources, you can assign IAM access policies to a resource group that allows them to view and manage the resources within a resource group. 

For example, you have a team A that is responsible to manage an {{site.data.keyword.containerlong_notm}} cluster, and another team B that develops serverless apps with {{site.data.keyword.openwhisk}}. Both teams use {{site.data.keyword.bplong_notm}} workspaces to manage their {{site.data.keyword.cloud_notm}} resources. To ensure workspace and resource isolation, you create a resource group for each team. Then, you assign the required permissions to each resource group. For example, the **Manager** service access role to all workspaces in resource group A, but only **Reader** access to the workspaces in resource group B. 

**What is the benefit by using IAM access group?** <br>
To minimize the number of IAM access policies you need to assign to an individual user, you can create an [IAM access group](/docs/account?topic=account-groups) for each team, and assign them all necessary permissions to work with the resources in a resource group. 

The following image shows how you can leverage IAM access groups and resource groups to organize permissions in your {{site.data.keyword.cloud_notm}} account. 

<img src="images/schematics-user-flow-rg.png" alt="Using resource groups and IAM access groups to organize access to {{site.data.keyword.bplong_notm}}" width="900" style="width: 900px; border-style: none"/>

1. The account owner or an authorized administrator defines a team and creates an IAM access group for each team. 
2. The IAM access group is assigned access to resources within a specific resource group. For example, access group A receives editor permissions for all resources in resource group A, but only viewer permissions for the resources in resource group B. 
3. The account owner or an authorized administrator adds users to the IAM access group. All users automatically inherit the permissions of the IAM access group. 

## Overview of {{site.data.keyword.bpshort}} service access roles and required permissions
{: #access-roles}
{: help}
{: support}

Grant access to {{site.data.keyword.bplong_notm}} by assigning {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) service access roles to your users. 
{: shortdsec}

**Who must grant access to {{site.data.keyword.bplong_notm}}?** <br>
As the account owner or an authorized account administrator you can assign IAM service access roles to your users. The IAM service access roles determine the actions that you can perform on an {{site.data.keyword.bplong_notm}} resource, such as a workspace or an action. To avoid assigning access policies to individual users, consider creating [IAM access groups](/docs/account?topic=account-groups). 

**Is access to {{site.data.keyword.bplong_notm}} sufficient to work with {{site.data.keyword.cloud_notm}} resources?** <br>
No. If you are assigned an {{site.data.keyword.bplong_notm}} service access role, you can view, create, update, or delete workspaces or actions in {{site.data.keyword.bplong_notm}}. To provision {{site.data.keyword.cloud_notm}} resources, run cloud operations, or install software on target hosts, you must be assigned the IAM platform or service access role for the individual {{site.data.keyword.cloud_notm}} resource that you want to work with. Refer to the [documentation](/docs/home/alldocs) for your resource to determine the access policies that you need to work with your resource. 

### Workspace permissions
{: #workspace-permissions}

Review the following table to see what permissions you need to work with {{site.data.keyword.bpshort}} workspaces.

| Action | Reader | Writer | Manager | Account owner |
|-----|-----|-----|-----|--------|
| View workspace | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| View workspace activities | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| View workspace logs | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| Create workspace | | | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| Update workspace | | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| Delete workspace | | | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| Freeze and unfreeze workspace | | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| View the `readme` of a template| ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg)|![Check mark icon](images/checkmark.svg)|![Check mark icon](images/checkmark.svg)| 
| Create Terraform execution plan | | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| Apply a Terraform template | | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| Destroy workspace resources | | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
{: row-headers}
{: class="comparison-table"}
{: caption="User permissions for {{site.data.keyword.bpshort}} workspaces" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the access role in column one, and the permission descriptions in column two."}

### Action permissions
{: #action-permissions}

Review the following table to see what permissions you need to work with {{site.data.keyword.bpshort}} actions.

| Action | Reader | Writer | Manager | Account owner |
|-----|-----|-----|-----|--------|
| View action | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| View action jobs | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| View job logs | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| Create action | | | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| Update action | | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| Delete action | | | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| Run check action job | | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg) | 
| Run an action| ![Check mark icon](images/checkmark.svg) | ![Check mark icon](images/checkmark.svg)|![Check mark icon](images/checkmark.svg)|![Check mark icon](images/checkmark.svg)| 
{: row-headers}
{: class="comparison-table"}
{: caption="User permissions for {{site.data.keyword.bpshort}} actions" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the access role in column one, and the permission descriptions in column two."}


## Setting up access for your users
{: #access-setup}

As the {{site.data.keyword.cloud_notm}} account owner or authorized account administrator. Create an IAM access group for your users and assign service access policies to {{site.data.keyword.bplong_notm}} and the resources that you want your users to work with.  
{: shortdesc}

1. [Invite users to your {{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-iamuserinv).

2. Define your teams and [create an IAM access group](/docs/account?topic=account-groups#create_ag) for each team. 

3. [Create a resource group](/docs/account?topic=account-rgs#create_rgs) for each teams. So that you can organize access to their {{site.data.keyword.cloud_notm}} services and workspaces in your account, and bundle them under one common view and billing process. If you want to keep your {{site.data.keyword.bplong_notm}} workspaces and actions separate from the {{site.data.keyword.cloud_notm}} resources, you must create multiple resource groups. 

4. [Assign access to your IAM access group](/docs/account?topic=account-groups#access_ag). Consider the following guidelines when you assign access to an IAM access group: 
   - Make sure to scope access of your group to the resource group that you created for this team. 
   - If your team have access to multiple resource groups. For example, you want them to have **Administrator** and **Manager** permissions on all resources in resource group A, but only **Viewer** access for the resources in resource group B, you can create multiple access policies to achieve.
   - The resource group of the {{site.data.keyword.bpshort}} workspace can be different from the resource group where your {{site.data.keyword.cloud_notm}} resources are provisioned.
   - For a team to use {{site.data.keyword.bpshort}}. You must assign the appropriate [service access role for {{site.data.keyword.bpshort}}](#access-roles), and the permissions that are required for the {{site.data.keyword.cloud_notm}} resources that this team provisions and manages with {{site.data.keyword.bpshort}}. You can review the [documentation](/docs/home/alldocs) for each of the {{site.data.keyword.cloud_notm}} services to find the appropriate IAM access policy.  
   
Next, you can [create Terraform configuration files](/docs/schematics?topic=schematics-create-tf-config), [create a workspace](/docs/schematics?topic=schematics-workspace-setup), and start [provisioning {{site.data.keyword.cloud_notm}} resources](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources) in your account.



