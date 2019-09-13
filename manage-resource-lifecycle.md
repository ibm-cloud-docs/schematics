---

copyright:
  years: 2017, 2019
lastupdated: "2019-09-13"

keywords: Schematics, automation, Terraform

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

# Managing resource lifecycles
{: #manage-lifecycle}

Deploy, modify, and remove {{site.data.keyword.cloud_notm}} resources by changing the infrastructure code in your Terraform template and by using {{site.data.keyword.cloud_notm}} Schematics to apply the template in your {{site.data.keyword.cloud_notm}} account. 
{: shortdesc}

**When do I use {{site.data.keyword.cloud_notm}} Schematics vs. the individual resource dashboards?**</br>
With {{site.data.keyword.cloud_notm}} Schematics, you can run your infrastructure code in {{site.data.keyword.cloud_notm}} to manage the lifecycle of {{site.data.keyword.cloud_notm}} resources. After you provision a resource, you use the dashboard of the individual resource to work and interact with your resource. For example, if you provision a virtual server instance in a Virtual Private Cloud (VPC) with {{site.data.keyword.cloud_notm}} Schematics, you use the VPC console, API, or CLI to stop, reboot, and power on your virtual server instance. However to remove the virtual server instance, you use {{site.data.keyword.cloud_notm}} Schematics. 

**What happens if I manually added, or removed a resource from the service dashboard directly?** </br>
When you provision resources with {{site.data.keyword.cloud_notm}} Schematics, the state of your resources is stored in a local {{site.data.keyword.cloud_notm}} Schematics state file. This state file is the single source of truth for {{site.data.keyword.cloud_notm}} Schematics to determine what resources are provisioned in your {{site.data.keyword.cloud_notm}} account. If you manually add a resource without using {{site.data.keyword.cloud_notm}} Schematics, this resource is not stored in the {{site.data.keyword.cloud_notm}} Schematics state file, and as a consequence cannot be managed with {{site.data.keyword.cloud_notm}} Schematics. 

When you manually remove a resource that you provisioned with {{site.data.keyword.cloud_notm}} Schematics, the state file is not updated automatically and becomes out of sync. When you create your next Terraform execution plan or apply a new template version, Schematics verifies that the {{site.data.keyword.cloud_notm}} resources in the state file exist in your {{site.data.keyword.cloud_notm}} account with the state that is captured in your state file. If the resource is not found, the state file is updated and the Terraform execution plan is changed accordingly. 

To keep your {{site.data.keyword.cloud_notm}} Schematics state file and the {{site.data.keyword.cloud_notm}} resources in your account in sync, use {{site.data.keyword.cloud_notm}} Schematics only to provision, or remove your resources. 
{: important}

## Deploying your resources
{: #deploy-resources}

Run your infrastructure code to provision, or modify your {{site.data.keyword.cloud_notm}} resources with Schematics.  
{:shortdesc}

**Before you begin**: 
- [Create a workspace from your GitHub repository](/docs/schematics?topic=schematics-workspace-setup#create-workspace) that hosts your Terraform template. 
- Make sure that you have the required [permissions](/docs/schematics?topic=schematics-access) to deploy the resources in {{site.data.keyword.cloud_notm}}. 

**To deploy your resources**: 

1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces), select the workspace that points to the Terraform template that you want to apply. 
2. Click **Pull latest** to get the latest version of your Terraform template from the linked GitHub source repository.
3. Optional: From the navigation, select **Variables** to review the variables that you set for your workspace. The values of your variables are used every time that you reference the variable in your Terraform template. Make sure that your variables use the correct values.  
4. Click **Generate plan** to create a Terraform execution plan. This action equals the `terraform plan` command. You can review the status of your plan in the **Recent activity** section of your workspace details page.
5. From the **Recent activity** section, click **View log** to review the log files of your execution plan. The execution plan includes a summary of {{site.data.keyword.cloud_notm}} resources that must be created, modified, or deleted to achieve the state that you described in your Terraform template. If you have syntax errors in your Terraform configuration files, you can review the error message in the log file. 
6. Review available service plans and pricing information for each of the {{site.data.keyword.cloud_notm}} resources that Schematics is about to create or change. Some services come with a limit per {{site.data.keyword.cloud_notm}} account. If you are about to reach the service limit for your account, the resource is not provisioned until you increase the service quota, or remove existing services first. 
7. When you are ready, apply your Terraform template by clicking **Apply plan** from the workspace details page. This action equals the `terraform apply` command. {{site.data.keyword.cloud_notm}} Schematics starts provisioning, modifying, or deleting your {{site.data.keyword.cloud_notm}} resources based on what actions were identified in the execution plan. Depending on the type and number of resources that you want to provision or modify, this process might take a few minutes, or even up to hours to complete. During this time, you cannot make changes to your workspace. After all updates are applied, the state of your {{site.data.keyword.cloud_notm}} resources is stored in a Terraform state file that {{site.data.keyword.cloud_notm}} Schematics uses to determine what resources exist in your {{site.data.keyword.cloud_notm}} account. 
8. Review the log file to ensure that no errors occurred during the provisioning, modification, or deletion process. 
9. From the navigation, select **Resources** to find a summary of {{site.data.keyword.cloud_notm}} resources that are available in your {{site.data.keyword.cloud_notm}} account.

## Updating your resources
{: #update-resources}

To update {{site.data.keyword.cloud_notm}} resources, you must first change the infrastructure code in your Terraform template before you can run your code in {{site.data.keyword.cloud_notm}} Schematics.   
{: shortdesc}

**What changes can I make to my resources?** </br>
You can choose to add, modify, or remove infrastructure code in your Terraform template in GitHub, or update variable values from the workspace dashboard in {{site.data.keyword.cloud_notm}} Schematics.  

Depending on how you change the configuration of existing resources, {{site.data.keyword.cloud_notm}} Schematics might not be able to update your resource. Instead, {{site.data.keyword.cloud_notm}} Schematics might decide that in order to apply the change, the resource must be removed, and a new resource must be created. If your resource must be removed, make sure that you do not interrupt a working environment, or delete data. 
{: note}

**When I change my configuration file in GitHub, is my change automatically available in the next execution plan?** </br>
If you change the code of your Terraform template in GitHub, these changes are not available automatically when you create an execution plan in {{site.data.keyword.cloud_notm}} Schematics. To pull the latest changes from your GitHub repository, make sure that you click the **Pull latest** button from the workspace details page before you create your execution plan.

To update your resources: 

1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that points to the Terraform template that you updated. 
2. Click **Pull latest** to get the latest version of your Terraform template from the linked GitHub source repository. 
3. If you added, or removed variables in your Terraform configuration files, or if you want to change the variable values that you set when you created the workspace, open the **Variables** details of your workspace, and enter or change the variable values. 
4. Go back to the **Details** of your workspace, and click **Generate plan** to create a Terraform execution plan. 
5. From the **Recent activity** section, click **View log** to review the log files of your execution plan. This log file provides a summary of all the resources that {{site.data.keyword.cloud_notm}} Schematics is about to modify. {{site.data.keyword.cloud_notm}} Schematics might not be able to modify some of your resources, and suggest removing and re-creating the resource.
6. Click **Apply plan** to apply the new Terraform template version. Depending on the changes that you made, it might take a few minutes or up to a few hours for the template to be applied.   
7. Review the log files to ensure that no errors occurred during the modification process. 
8. From the navigation, select **Resources** and verify that your resources show the updated configuration. 


## Reviewing resource and deployment details
{: #review-logs}

View the details of the {{site.data.keyword.cloud_notm}} Schematics deployments and the {{site.data.keyword.cloud_notm}} resources that you currently manage with {{site.data.keyword.cloud_notm}} Schematics.
{:shortdesc}

1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that you want to inspect.
2. From the workspace details page, review the logs of previous Terraform execution plans and the plans that you applied in your environment in the **Recent activity** section. 
3. From the navigation, select **Resources** to review the state of the {{site.data.keyword.cloud_notm}} resources that you created with this workspace. 
4. To review who made a change to your Terraform template, go to the source repository in GitHub that is linked to your workspace, and use the built-in capabilities such as the commit history and pull requests to review changes. 
  
## Removing your resources
{: #destroy-resources}

To remove an {{site.data.keyword.cloud_notm}} resource that you provisioned with {{site.data.keyword.cloud_notm}} Schematics, you can either remove your resources with Schematics, delete your workspace, or remove the infrastructure code in the Terraform template of your GitHub source repository. 
{:shortdesc}

**How should I remove resources with {{site.data.keyword.cloud_notm}} Schematics?** </br>
You can use the {{site.data.keyword.cloud_notm}} Schematics console or CLI to remove all of the resources that you provisioned with Schematics. To stay in sync with your Terraform template, make sure to also remove the associated infrastructure code from your Terraform template so that your resources are not re-added when you apply a new version of your Terraform template. 

**What happens if I choose to delete my resource directly from the resource dashboard?** </br>
When you manually remove a resource that you provisioned with {{site.data.keyword.cloud_notm}} Schematics, the state file is not updated automatically and becomes out of sync. When you create your next Terraform execution plan or apply a new template version, Schematics verifies that the {{site.data.keyword.cloud_notm}} resources in the state file exist in your {{site.data.keyword.cloud_notm}} account with the state that is captured. If the resource is not found, the state file is updated and the Terraform execution plan is changed accordingly. 

Although the state file is updated before new changes to your {{site.data.keyword.cloud_notm}} resources are applied, do not manually remove resources from the resource dashboard to avoid unexpected results. Instead, use the {{site.data.keyword.cloud_notm}} Schematics console or CLI to remove your resources, or remove the associated infrastructure code from your Terraform template. 
{: important}

**Are my resources removed when I remove the workspace** </br>
Removing the workspace from {{site.data.keyword.cloud_notm}} Schematics does not remove any of your {{site.data.keyword.cloud_notm}} resources. If you remove the workspace before you removed your resources, you must manually remove all of your {{site.data.keyword.cloud_notm}} resources from the individual resource dashboard. 

Removing an {{site.data.keyword.cloud_notm}} resource cannot be undone. Make sure that you backed up your data before you remove a resource. If you choose to remove the infrastructure code, or comment out the resource in your Terraform configuration file, make sure to thoroughly review the log file of your execution plan to verify that all your resources are included in the removal.    
{: important}

**To remove your resources by deleting the infrastructure code from you Terraform template**: 

1. Open the Terraform configuration file in your source repository in GitHub that includes the infrastructure code of your resource. 
2. Either remove the infrastructure code from the file, or comment out the resources that you want to remove by adding `#` to the beginning of each line. 

   Example for commenting out a resource: 
   ```
   ...
   #resource ibm_is_instance "vsi1" {
   #  name    = "${local.BASENAME}-vsi2"
   #  vpc     = "${ibm_is_vpc.vpc.id}"
   #  zone    = "${local.ZONE}"
   #  keys    = ["${data.ibm_is_ssh_key.ssh_key_id.id}"]
   #  image   = "${data.ibm_is_image.ubuntu.id}"
   #  profile = "cc1-2x4"

   #  primary_network_interface = {
   #    subnet          = "${ibm_is_subnet.subnet1.id}"
   #    security_groups = ["${ibm_is_security_group.sg1.id}"]
   #  }
   #}
   ```
   {: codeblock}

3. Commit the change to your Terraform configuration file. 
4. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that points to the Terraform template that you just changed. 
5. Click **Pull latest** to get the latest version of your Terraform template from the linked GitHub source repository. 
6. Click **Generate plan** to create a Terraform execution plan. 
7. From the **Recent activity** section, click **View log** to review the log files of your execution plan. The log files provide a summary of all the resources that {{site.data.keyword.cloud_notm}} Schematics is about to remove. 
8. Click **Apply plan** to remove the {{site.data.keyword.cloud_notm}} resources from your account. 
9. Review the log files to ensure that no errors occurred during the deletion process. 
10. From the navigation, select **Resources** and verify that your resources are removed. 
11. Optional: After you removed all your resources, remove your workspace. 
    1. Open the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external} and find the workspace that you want to remove. 
    2. From the actions menu, click **Delete workspace**. 
    3. Confirm the deletion of your workspace by clicking **Delete**. 

