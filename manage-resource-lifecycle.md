---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-26"

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

Deploy, modify, and remove {{site.data.keyword.cloud_notm}} resources by making changes to your Terraform configuration files and using {{site.data.keyword.cloud_notm}} Schematics to apply the configuration in your {{site.data.keyword.cloud_notm}} account. 
{: shortdesc}

**When do I use {{site.data.keyword.cloud_notm}} Schematics vs. the individual resource dashboards?**</br>
With {{site.data.keyword.cloud_notm}} Schematics, you can run your infrastructure code in {{site.data.keyword.cloud_notm}} to provision and manage a set of {{site.data.keyword.cloud_notm}} resources. After you provision a resource, you use the dashboard of the individual resource to work and interact with your resource. For example, if you provision a virtual server instance in a Virtual Private Cloud (VPC) with {{site.data.keyword.cloud_notm}} Schematics, you use the VPC console, API, or CLI to stop, reboot, and power on your virtual server instance. To remove the virtual server instance, you use {{site.data.keyword.cloud_notm}} Schematics. 

**What happens if I manually added, or removed a resource from the service dashboard directly?** </br>
When you provision resources with {{site.data.keyword.cloud_notm}} Schematics, the state of your resources is stored in a local {{site.data.keyword.cloud_notm}} Schematics state file. This state file is the single source of truth for {{site.data.keyword.cloud_notm}} Schematics to determine what resources are provisioned in your {{site.data.keyword.cloud_notm}} account. If you manually add a resource without using {{site.data.keyword.cloud_notm}} Schematics, this resource is not stored in the {{site.data.keyword.cloud_notm}} Schematics state file, and as a consequence cannot be managed with {{site.data.keyword.cloud_notm}} Schematics. 

When you manually remove a resource that you provisioned with {{site.data.keyword.cloud_notm}} Schematics, the state file is not updated automatically and becomes out of sync. Even if you create a new execution plan, {{site.data.keyword.cloud_notm}} Schematics checks your Terraform configuration file against the state that is stored in the state file. Because your state file still includes the resource that you manually removed, the resource cannot be re-added with {{site.data.keyword.cloud_notm}} Schematics. 

To keep your {{site.data.keyword.cloud_notm}} Schematics state file and the {{site.data.keyword.cloud_notm}} resources in your account in sync, use {{site.data.keyword.cloud_notm}} Schematics only to provision, or remove your resources. 
{: important}

## Deploying your resources
{: #deploy-resources}

Run your infrastructure code to provision, or modify your {{site.data.keyword.cloud_notm}} resources by using the {{site.data.keyword.cloud_notm}} Schematics console.  
{:shortdesc}

Before you begin, [create a workspace from your GitHub repository](/docs/schematics?topic=schematics-workspace-setup#create-workspace) that hosts your Terraform configuration files. 

1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that points to the Terraform configuration files that you want to apply. 
2. Click **Retrieve latest configuration** to get the latest version of your Terraform configuration files from the linked GitHub source repository. 
3. Click **Run new plan** to create a Terraform execution plan. This action equals the `terraform plan` command. You can review the status of your plan in the **Recent activtity** section of your workspace details page.
4. From the **Recent activity** section, review the log files of your execution plan. This plan includes a summary of {{site.data.keyword.cloud_notm}} resources that must be created, modified, or deleted to achieve the state that you described in your Terraform configuration files. If you have syntax errors in your configuration files, you can review the error message in the log file. 
5. Optional: Open the **Variables** tab from the workspace details page to review the variables that you set for your workspace. The values of your variables are used every time you reference the variable in your Terraform configuration file. 
6. Review available service plans and pricing information for each of the {{site.data.keyword.cloud_notm}} resources that you are about to create. Some services come with a limit per {{site.data.keyword.cloud_notm}} account. If you are about to reach the service limit for your account, the resource is not provisioned until you increase the service quota, or remove existing services first. 
7. Make sure that you have the required permissions in {{site.data.keyword.cloud_notm}} to provision, modify, or remove the {{site.data.keyword.cloud_notm}} resource that is described in your Terraform configuration file. Review the {{site.data.keyword.cloud_notm}} documentation for each resource to find information about required permissions. 
8. When you are ready, apply your Terraform configuration by clicking **Apply plan** from the **Details** tab of the workspace details page. This action equals the `terraform apply` command. {{site.data.keyword.cloud_notm}} Schematics starts provisioning, modifying, or deleting your {{site.data.keyword.cloud_notm}} resources based on what actions were identified in the execution plan. Depending on the type and number of resources that you want to provision or modify, this process might take a few minutes, or even up to hours to complete. During this time, you cannot make changes to your workspace. After all updates are applied, the state of your {{site.data.keyword.cloud_notm}} resources is stored in a Terraform state file that {{site.data.keyword.cloud_notm}} Schematics uses to determine what resources exist in your {{site.data.keyword.cloud_notm}} account. 
9. Review the log file to ensure that no errors occurred during the provisioning, modification, or deletion process. 
10. From the workspace details page, select the **Resources** tab to find a summary of {{site.data.keyword.cloud_notm}} resources that are available in your {{site.data.keyword.cloud_notm}} account.

## Updating your resources
{: #update-resources}

Deploying changes to your environment is a lightweight process. To change which resources are allocated, you code changes to your Terraform configuration in declarative syntax, meaning you state only the outcome you want. With {{site.data.keyword.bpshort}}, you can preview your changes before deployment.

**What changes can I make to my resources?** </br>
You can choose to add, modify, or remove infrastructure code in your Terraform configuration files in GitHub, or update variable values from the workspace dashboard in {{site.data.keyword.cloud_notm}} Schematics.  

Depending on how you change the configuration of existing resources, {{site.data.keyword.cloud_notm}} Schematics might not be able to update your resource. Instead, {{site.data.keyword.cloud_notm}} Schematics might decide that in order to apply the change, the resource must be destroyed, and a new resource must be created. If your resource must be destroyed, make sure that you do not interrupt a working environment, or destroy data. 
{: note}

**When I change my configuration file in GitHub, is my change automatically availabe in the next execution plan?** </br>
If you make changes to the configuration files in GitHub, these changes are not available automatically when you create an execution plan in {{site.data.keyword.cloud_notm}} Schematics. To pull the latest changes from your GitHub repository, make sure that you click **Retrieve latest configuration** from the workspace details page before your create your execution plan.

To update your resources: 

1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that points to the Terraform configuration file that you just changed. 
2. Click **Retrieve latest configuration** to get the latest version of your Terraform configuration files from the linked GitHub source repository. 
3. If you added, or removed variables in your Terraform configuration files, or if you want to change the variable values that you set when you created the workspace, open the **Variables** tab from the workspace details page, and enter or change the variable values. 
4. From the **Details** tab of the workspace details page, click **Run new plan** to create a Terraform execution plan. 
5. From the **Recent activity** section, review the log files of your execution plan. This log file provide a summary of all the resources that {{site.data.keyword.cloud_notm}} Schematics is about to modify. {{site.data.keyword.cloud_notm}} Schematics might not be able to modify some of your resources, and suggest to remove and re-create the resource.
6. Click **Apply plan** to apply the new Terraform configuration. Depending on the changes that you made, it might take a few minutes or up to a few hours for the configuration to be applied.   
7. Review the log files to ensure that no errors occurred during the modification process. 
8. From the workspace details page, select the **Resources** tab and verify that your resources show the updated configuration. 


## Reviewing resource and deployment details
{: #review-logs}

View the details of the {{site.data.keyword.cloud_notm}} Schematics deployments and the {{site.data.keyword.cloud_notm}} resources that you currently manage with {{site.data.keyword.cloud_notm}} Schematics by using the Schematics console or API.
{:shortdesc}

1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that you want to inspect.
2. From the workspace details page, review the logs of previous Terraform execution plans and the plans that you applied in your environment in the **Recent Activity** section. 
3. To review the state of the {{site.data.keyword.cloud_notm}} resources that you created with this workspace, select the **Resources** tab of the workspace details page. 
4. To review who made a change to your Terraform configuration files, go to the source repository in GitHub that is linked to your workspace, and use the built-in capabilities such as the commit history and pull requests to review changes. 
  
## Removing your resources
{: #destroy-resources}

To remove an {{site.data.keyword.cloud_notm}} that you provisioned with {{site.data.keyword.cloud_notm}} Schematics, you can either remove the code, or comment out the resource in the Terraform configuration file. 
{:shortdesc}

**How should I remove resources with {{site.data.keyword.cloud_notm}} Schematics?** </br>
During the {{site.data.keyword.cloud_notm}} Schematics beta, you cannot use the console or API to instruct {{site.data.keyword.cloud_notm}} Schematics to remove all of the resources that you provisioned with {{site.data.keyword.cloud_notm}} Schematics. However, because {{site.data.keyword.cloud_notm}} Schematics executes the actions that are required to achieve the state that you describe in your Terraform configuration file, you can either remove the infrastructure code from your file, or comment out the resources that you want to remove. 

**What happens if I choose to delete my resource with the resource dashboard?** </br>
When you manually remove a resource that you provisioned with {{site.data.keyword.cloud_notm}} Schematics, the state file is not updated automatically and becomes out of sync. Even if you create a new execution plan, {{site.data.keyword.cloud_notm}} Schematics checks your Terraform configuration file against the state that is stored in the state file. Because your state file still includes the resource that you manually removed, the resource cannot be re-added with {{site.data.keyword.cloud_notm}} Schematics and remain orphaned. 

**Are my resources removed when I remove the workspace** </br>
No. Removing the workspace from {{site.data.keyword.cloud_notm}} Schematics does not remove any of your {{site.data.keyword.cloud_notm}} resources. If you remove the workspace before you removed your resources, you must manually remove all of your {{site.data.keyword.cloud_notm}} resources from the individual resource dashboard. 

Removing an {{site.data.keyword.cloud_notm}} resource cannot be undone. Make sure that you backed up your data before you remove a resource. If you choose to remove the infrastructure code, or comment out the resource in your Terraform configuration file, make sure to thoroughly review the log file of your execution plan to verify that all your resources are included in the removal.    
{: important}

To remove your resources: 

1. Open the Terraform configuration file in your source repository in GitHub. 
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
4. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that points to the Terraform configuration file that you just changed. 
5. Click **Retrieve latest configuration** to get the latest version of your Terraform configuration files from the linked GitHub source repository. 
6. Click **Run new plan** to create a Terraform execution plan. 
7. From the **Recent activity** section, review the log files of your execution plan. This log files provide a summary of all the resources that {{site.data.keyword.cloud_notm}} Schematics is about to remove. 
8. Click **Apply plan** to remove the {{site.data.keyword.cloud_notm}} resources from your account. 
9. Review the log files to ensure that no errors occurred during the deletion process. 
10. From the workspace details page, select the **Resources** tab and verify that your resources are removed. 
11. Optional: After you removed all your resources, remove your workspace. 
    1. Open the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external} and find the workspace that you want to remove. 
    2. From the actions menu, click **Delete**. 
    3. Confirm the deletion of your workspace by clicking **Delete**. 

