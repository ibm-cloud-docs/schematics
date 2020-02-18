---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-07"

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

Use {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) to grant permissions to {{site.data.keyword.bplong_notm}} and the {{site.data.keyword.cloud_notm}} resources that you want to provision. 
{: shortdesc}
  
As of 6 February 2019, {{site.data.keyword.bplong_notm}} uses resource groups to organize permissions to workspaces. The previous account-level permissions to workspaces are deprecated and must be migrated to use resource groups. For more information about this change and how to migrate your permissions, see [Migrating to resource group permissions](#rg-permissions). 
{: note}

## Using IAM access groups and resource groups to organize access to workspaces
{: #team-org}

Learn how you can organize and simplify access to your workspaces by leveraging IAM access groups and {{site.data.keyword.cloud_notm}} resource groups. 
{: shortdesc}

As the {{site.data.keyword.cloud_notm}} account owner, you want to ensure that you control user access to the {{site.data.keyword.bplong_notm}} workspaces and the actions that your users can perform. {{site.data.keyword.bplong_notm}} is designed to be used by teams that have access to resources in a specific {{site.data.keyword.cloud_notm}} resource group. 

**What is a resource group and how does it help me organize my team?**</br>
Assigning access to a particular {{site.data.keyword.cloud_notm}} service is a good way of allowing a user to work with a specific service in your account. However, when you build production workloads in the cloud, you most likely have multiple {{site.data.keyword.cloud_notm}} services and resources that are used by different teams. With resource groups, you can organize multiple services in your account and bundle them under one common view and billing process. To allow your team to work with these resources, you can assign IAM access policies to a resource group that allows them to view and manage the resources within a resource group. 

For example, let's say you have a team A that is responsible to manage an {{site.data.keyword.containerlong_notm}} cluster, and another team B that develops serverless apps with {{site.data.keyword.openwhisk}}. Both teams use {{site.data.keyword.bplong_notm}} workspaces to manage their {{site.data.keyword.cloud_notm}} resources. To ensure workspace and resource isolation, you create a resource group for each team. Then, you assign the required permissions to each resource group, such as the **Manager** service access role to all workspaces in resource group A, but only **Reader** access to the workspaces in resource group B. 

**Do my workspace and {{site.data.keyword.cloud_notm}} resources need to be in the same resource group?**</br>
No. You can create different resource groups for your {{site.data.keyword.bplong_notm}} workspaces and {{site.data.keyword.cloud_notm}} resources. 

**What is the benefit of using IAM access group?** </br>
To minimize the number of IAM access policies that you need to assign to individual users, you can create an [IAM access group](/docs/iam?topic=iam-groups) for each team, and assign them all necessary permissions to work with the resources in a resource group. 

The following image shows how you can leverage IAM access groups and resource groups to organize permissions in your {{site.data.keyword.cloud_notm}} account. 

<img src="images/schematics-user-flow-rg.png" alt="Using resource groups and IAM access groups to organize access to {{site.data.keyword.bplong_notm}}" width="900" style="width: 900px; border-style: none"/>

1. The account owner or an authorized administrator defines a team and creates an IAM access group for each team. 
2. The IAM access group is assigned access to resources within a specific resource group. For example access group A receives editor permissions for all resources in resource group A, but only viewer permissions for the resources in resource group B. 
3. The account owner or an authorized administrator adds users to the IAM access group. All users automatically inherit the permissions of the IAM access group. 

## Overview of {{site.data.keyword.bpshort}} service access roles and required permissions
{: #access-roles}
{: help}
{: support}

Grant access to {{site.data.keyword.bplong_notm}} by assigning {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) service access roles to your users. 
{: shortdsec}

**Who must grant access to {{site.data.keyword.bplong_notm}}?** </br>
As the account owner or an authorized account administrator you can assign IAM service access roles to your users. The IAM service access roles determine the actions that you can perform on an {{site.data.keyword.bplong_notm}} workspace. To avoid assigning access policies to individual users, consider creating [IAM access groups](/docs/iam?topic=iam-groups). 

**If I have access to {{site.data.keyword.bplong_notm}}, can I automatically provision {{site.data.keyword.cloud_notm}} resources?** </br>
No. If you are assigned an {{site.data.keyword.bplong_notm}} service access role, you can view, create, update, or delete workspaces in {{site.data.keyword.bplong_notm}}. To provision the {{site.data.keyword.cloud_notm}} resources that you defined in your Terraform template, you must be assigned the IAM platform or service access role that is required to provision the individual resource. For example, to provision an {{site.data.keyword.containerlong_notm}} cluster, you must have the **Administrator** platform role and the **Manager** service access role for {{site.data.keyword.containerlong_notm}}. Refer to the [documentation](/docs/home/alldocs) for your resource to determine the access policies that you need to provision and work with your resource. 

**What else is required to enable users to provision {{site.data.keyword.cloud_notm}} resources?** </br>
To successfully provision {{site.data.keyword.cloud_notm}} resources, users must have access to a paid {{site.data.keyword.cloud_notm}} account. Charges incur when you create the resources in the {{site.data.keyword.cloud_notm}} account, which is initiated by clicking the **Apply plan** button from the {{site.data.keyword.bplong_notm}} console, or running the `ibmcloud terraform apply` command. 

Review the pricing information and account limitations for each {{site.data.keyword.cloud_notm}} resource that you want to provision before you create the resources. 
{: tip}

**What can I do in {{site.data.keyword.bpshort}} with a specific IAM service access role?** </br>
The following table shows the user permissions that are granted in {{site.data.keyword.bplong_notm}} when you assign an IAM service access role to your users.  

| Action | Reader | Writer | Manager | Account owner |
|-----|-----|-----|-----|--------|
| View workspace | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
| View workspace activities | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
| View workspace logs | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
| Create workspace | | | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
| Update workspace | | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
| Delete workspace | | | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
| Freeze and unfreeze workspace | | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
| View the `readme` of a template| ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg)|![Checkmark icon](images/checkmark.svg)|![Checkmark icon](images/checkmark.svg)| 
| Create Terraform execution plan | | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
| Apply a Terraform template | | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
| Destroy workspace resources | | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
{: row-headers}
{: class="comparison-table"}
{: caption="User permissions by service user type, account type, and access role" caption-side="top"}
{: summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the access role in column one, and the permission descriptions in column two."}


## Setting up access for your users
{: #access-setup}

As the {{site.data.keyword.cloud_notm}} account owner or authorized account administrator, create an IAM access group for your users and assign service access policies to {{site.data.keyword.bplong_notm}} and the resources that you want your users to work with.  
{: shortdesc}

1. [Invite users to your {{site.data.keyword.cloud_notm}} account](/docs/iam?topic=iam-iamuserinv).

2. Define your teams and [create an IAM access group](/docs/iam?topic=iam-groups#create_ag) for each team. 

3. [Create a resource group](/docs/resources?topic=resources-rgs#create_rgs) for each of your teams so that you can organize access to their {{site.data.keyword.cloud_notm}} services and workspaces in your account, and bundle them under one common view and billing process. If you want to keep your {{site.data.keyword.bplong_notm}} workspaces separate from the {{site.data.keyword.cloud_notm}} resources, you must create multiple resource groups. 

4. [Assign access to your IAM access group](/docs/iam?topic=iam-groups#access_ag). Consider the following guidelines when you assign access to an IAM access group: 
   - Make sure to scope access of your group to the resource group that you created for this team. 
   - If your team must have access to multiple resource groups, for example you want them to have **Administrator** and **Manager** permissions on all resources in resource group A, but only **Viewer** access for the resources in resource group B, you can create multiple access policies to achieve that.
   - The resource group of the {{site.data.keyword.bpshort}} workspace can be different from the resource group where your {{site.data.keyword.cloud_notm}} resources are provisioned.
   - For a team to use {{site.data.keyword.bpshort}}, you must assign the appropriate [service access role for {{site.data.keyword.bpshort}}](#access-roles), and the permissions that are required for the {{site.data.keyword.cloud_notm}} resources that this team provisions and manages with {{site.data.keyword.bpshort}}. You can review the [documentation](/docs/home/alldocs) for each of the {{site.data.keyword.cloud_notm}} services to find the appropriate IAM access policy.  
   
Next, you can [create Terraform configuration files](/docs/schematics?topic=schematics-create-tf-config), [create a workspace](/docs/schematics?topic=schematics-workspace-setup), and start [provisioning {{site.data.keyword.cloud_notm}} resources](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources) in your account.


## Migrating to resource group permissions
{: #rg-permissions}
  
Migrate your existing {{site.data.keyword.bpshort}} account-level permissions to use resource groups. 
{: shortdesc}

As of 6 February 2019, {{site.data.keyword.bplong_notm}} uses resource groups to organize permissions to workspaces. The previous account-level permissions to workspaces are deprecated and must be migrated to use resource groups. 
{: note}

If you used {{site.data.keyword.bplong_notm}} before 6 February 2020, you granted permissions to the service via Identity & Access Management (IAM). Because {{site.data.keyword.bpshort}} did not support scoping a service policy to a resource group, all service policies were scoped to the {{site.data.keyword.cloud_notm}} account by default. With the introduction of resource group-level policies, existing account-level policies must be removed and replaced with resource group-level policies to ensure proper access management to {{site.data.keyword.bpshort}} workspaces. 

1. Follow the steps in [Setting up access for your users](#access-setup).  

2. Remove any existing account-level permissions to {{site.data.keyword.bplong_notm}} from your users and IAM access groups. 
  

