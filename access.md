---

copyright:
  years: 2017, 2020
lastupdated: "2020-01-24"

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




{{site.data.keyword.bplong_notm}}is designed to be used by teams that are set up by an {{site.data.keyword.cloud_notm}} account owner or an authorized account administrator. To simplify the process of assigning access to your users, consider creating an IAM access group that includes the permissions to {{site.data.keyword.bplong_notm}} and the resources that you want your users to work with. 
{: tip}


The following image shows how {{site.data.keyword.cloud_notm}} account owners can set up access to {{site.data.keyword.bplong_notm}} and other {{site.data.keyword.cloud_notm}} resources. 

<img src="images/schematics-user-flow.png" alt="Assigning access to {{site.data.keyword.bplong_notm}}" width="900" style="width: 900px; border-style: none"/>



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
| Create workspace | | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
| Update workspace | | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
| Delete workspace | | | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
| Freeze and unfreeze workspace | | | ![Checkmark icon](images/checkmark.svg) | ![Checkmark icon](images/checkmark.svg) | 
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

3. [Create a resource group](/docs/resources?topic=resources-rgs#create_rgs) for each of your teams so that you can organize access to their {{site.data.keyword.cloud_notm}} services in your account and bundle them under one common view and billing process. 

3. [Assign access to your IAM access group](/docs/iam?topic=iam-groups#access_ag). Consider the following guidelines when you assign access to an IAM access group: 
   - Make sure to scope the access of your group to the resource group that you created for this team. 
   - If your team must have access to multiple resource groups, for example you want them to have **Administrator** and **Manager** permissions on all resources in resource group A, but only **Viewer** access for the resources in resource group B, you can create multiple access policies to achieve that.
   - For a team to use {{site.data.keyword.bpshort}}, you must assign the appropriate [service access role for {{site.data.keyword.bpshort}}](#access-roles), and the permissions that are required for the {{site.data.keyword.cloud_notm}} resources that this team provisions and manages with {{site.data.keyword.bpshort}}. You can review the [documentation](/docs/home/alldocs) for each of the {{site.data.keyword.cloud_notm}} services to find the appropriate IAM access policy. 
   - You can scope access of the group to specific workspaces within the resource group. If you do not specify a workspace, the same permissions are granted for all existing and future workspaces in the resource group.  

Next, you can [create Terraform configuration files](/docs/schematics?topic=schematics-create-tf-config), [create a workspace](/docs/schematics?topic=schematics-workspace-setup), and start [provisioning {{site.data.keyword.cloud_notm}} resources](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources) in your account.
