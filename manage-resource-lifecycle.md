---

copyright:
  years: 2017, 2020
lastupdated: "2020-03-26"

keywords: manage resources with schematics, schematics resource lifecycle, deploy resources with schematics, update resources with schematics, create terraform execution plan, apply terraform template

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

Deploy, modify, and remove {{site.data.keyword.cloud_notm}} resources by changing the infrastructure code in your Terraform template and by using {{site.data.keyword.bplong_notm}} to apply the template in your {{site.data.keyword.cloud_notm}} account. 
{: shortdesc}

**When do I use {{site.data.keyword.bplong_notm}} vs. the individual resource dashboards?**</br>
With {{site.data.keyword.bplong_notm}}, you can run your infrastructure code in {{site.data.keyword.cloud_notm}} to manage the lifecycle of {{site.data.keyword.cloud_notm}} resources. After you provision a resource, you use the dashboard of the individual resource to work and interact with your resource. For example, if you provision a virtual server instance in a Virtual Private Cloud (VPC) with {{site.data.keyword.bplong_notm}}, you use the VPC console, API, or CLI to stop, reboot, and power on your virtual server instance. However to remove the virtual server instance, you use {{site.data.keyword.bplong_notm}}. 

**What happens if I manually added, or removed a resource from the service dashboard directly?** </br>
When you provision resources with {{site.data.keyword.bplong_notm}}, the state of your resources is stored in a local {{site.data.keyword.bplong_notm}} state file. This state file is the single source of truth for {{site.data.keyword.bplong_notm}} to determine what resources are provisioned in your {{site.data.keyword.cloud_notm}} account. If you manually add a resource without using {{site.data.keyword.bplong_notm}}, this resource is not stored in the {{site.data.keyword.bplong_notm}} state file, and as a consequence cannot be managed with {{site.data.keyword.bplong_notm}}. 

When you manually remove a resource that you provisioned with {{site.data.keyword.bplong_notm}}, the state file is not updated automatically and becomes out of sync. When you create your next Terraform execution plan or apply a new template version, {{site.data.keyword.bpshort}} verifies that the {{site.data.keyword.cloud_notm}} resources in the state file exist in your {{site.data.keyword.cloud_notm}} account with the state that is captured in your state file. If the resource is not found, the state file is updated and the Terraform execution plan is changed accordingly. 

To keep your {{site.data.keyword.bplong_notm}} state file and the {{site.data.keyword.cloud_notm}} resources in your account in sync, use {{site.data.keyword.bplong_notm}} only to provision, or remove your resources. 
{: important}

## Deploying your resources
{: #deploy-resources}

Run your infrastructure code to provision, or modify your {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}.  
{:shortdesc}

**Before you begin**: 
- [Create a workspace from your GitHub repository](/docs/schematics?topic=schematics-workspace-setup#create-workspace) that hosts your Terraform template. 
- Make sure that you have the required [permissions](/docs/schematics?topic=schematics-access) to deploy the resources in {{site.data.keyword.cloud_notm}}. 

**To deploy your resources**: 

1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces), select the workspace that points to the Terraform template that you want to apply. 
2. Select the **Settings** tab.
3. In the **Summary** section, click **Pull latest** to get the latest version of your Terraform template from the linked GitHub source repository.
4. Optional: Review the variables that you set for your workspace. The values of your variables are used every time that you reference the variable in your Terraform template. Make sure that your variables use the correct values.  
5. Click **Generate plan** to create a Terraform execution plan. This action equals the `terraform plan` command. After you click this button, the workspace **Activity** page opens and {{site.data.keyword.bpshort}} starts to compare the state of the resources that you already provisioned in your {{site.data.keyword.cloud_notm}} account with the resources that you want to provision with your Terraform template. 
6. Click **View log** to review the log files of your execution plan. The execution plan includes a summary of {{site.data.keyword.cloud_notm}} resources that must be created, modified, or deleted to achieve the state that you described in your Terraform template. If you have syntax errors in your Terraform configuration files, you can review the error message in the log file. 
7. Review available service plans and pricing information for each of the {{site.data.keyword.cloud_notm}} resources that {{site.data.keyword.bpshort}} is about to create or change. Some services come with a limit per {{site.data.keyword.cloud_notm}} account. If you are about to reach the service limit for your account, the resource is not provisioned until you increase the service quota, or remove existing services first. 
8. When you are ready, apply your Terraform template by clicking **Apply plan**. This action equals the `terraform apply` command. After you click the button, {{site.data.keyword.bplong_notm}} starts provisioning, modifying, or deleting your {{site.data.keyword.cloud_notm}} resources based on what actions were identified in the execution plan. Depending on the type and number of resources that you want to provision or modify, this process might take a few minutes, or even up to hours to complete. During this time, you cannot make changes to your workspace. After all updates are applied, the state of your {{site.data.keyword.cloud_notm}} resources is stored in a Terraform state file that {{site.data.keyword.bplong_notm}} uses to determine what resources exist in your {{site.data.keyword.cloud_notm}} account. 
9. Review the log file to ensure that no errors occurred during the provisioning, modification, or deletion process. 
10. From the navigation, select **Resources** to find a summary of {{site.data.keyword.cloud_notm}} resources that are available in your {{site.data.keyword.cloud_notm}} account.

## Updating your resources
{: #update-resources}

To update {{site.data.keyword.cloud_notm}} resources, you must first change the infrastructure code in your Terraform template before you can run your code in {{site.data.keyword.bplong_notm}}.   
{: shortdesc}

**What changes can I make to my resources?** </br>
You can choose to add, modify, or remove infrastructure code in your Terraform template in GitHub, or update variable values from the workspace dashboard in {{site.data.keyword.bplong_notm}}.  

Depending on how you change the configuration of existing resources, {{site.data.keyword.bplong_notm}} might not be able to update your resource. Instead, {{site.data.keyword.bplong_notm}} might decide that in order to apply the change, the resource must be removed, and a new resource must be created. If your resource must be removed, make sure that you do not interrupt a working environment, or delete data. 
{: note}

**When I change my configuration file in GitHub, is my change automatically available in the next execution plan?** </br>
If you change the code of your Terraform template in GitHub, these changes are not available automatically when you create an execution plan in {{site.data.keyword.bplong_notm}}. To pull the latest changes from your GitHub repository, make sure that you click the **Pull latest** button from the workspace settings page before you create your execution plan.

To update your resources: 

1. From the [workspace dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/schematics/workspaces), select the workspace that points to the Terraform template that you updated. 
2. Select the **Settings** tab.
3. In the **Summary** section, click **Pull latest** to get the latest version of your Terraform template from the linked GitHub source repository. 
4. If you added, or removed variables in your Terraform configuration files, or if you want to change the variable values that you set when you created the workspace, enter or change the variable values. 
5. Click **Generate plan** to create a Terraform execution plan. Note that during this time, you cannot make changes to your workspace. 
6. Click **View log** to review the log files of your execution plan. This log file provides a summary of all the resources that {{site.data.keyword.bplong_notm}} is about to modify. {{site.data.keyword.bplong_notm}} might not be able to modify some of your resources, and suggest removing and re-creating the resource.
7. Click **Apply plan** to apply the new Terraform template version. Depending on the changes that you made, it might take a few minutes or up to a few hours for the template to be applied. Note that during this time, you cannot make changes to your workspace.
8. Review the log files to ensure that no errors occurred during the modification process. 
9. From the navigation, select **Resources** and verify that your resources show the updated configuration. 


## Reviewing resource and deployment details
{: #review-logs}

View the details of the {{site.data.keyword.bplong_notm}} deployments and the {{site.data.keyword.cloud_notm}} resources that you currently manage with {{site.data.keyword.bplong_notm}}.
{:shortdesc}

1. From the [workspace dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/schematics/workspaces), select the workspace that you want to inspect.
2. From the navigation, select **Activity** to find a summary of activities in your workspace.
3. Review the logs of previous Terraform execution plans and the plans that you applied. 
4. From the navigation, select **Resources** to review the state of the {{site.data.keyword.cloud_notm}} resources that you created with this workspace. 
5. To review who made a change to your Terraform template, go to the source repository in GitHub that is linked to your workspace, and use the built-in capabilities such as the commit history and pull requests to review changes.
6. To review events that {{site.data.keyword.bpshort}} sent to {{site.data.keyword.cloudaccesstrailshort}}, see [{{site.data.keyword.at_full_notm}} events](/docs/schematics?topic=schematics-at_events). 
  
## Removing your resources
{: #destroy-resources}

To remove an {{site.data.keyword.cloud_notm}} resource that you provisioned with {{site.data.keyword.bplong_notm}}, you can either remove your resources with {{site.data.keyword.bpshort}}, delete your workspace, or remove the infrastructure code in the Terraform template of your GitHub source repository. 
{:shortdesc}

**How should I remove resources with {{site.data.keyword.bplong_notm}}?** </br>
You can use the {{site.data.keyword.bplong_notm}} console or CLI to remove all of the resources that you provisioned with {{site.data.keyword.bpshort}}. To stay in sync with your Terraform template, make sure to also remove the associated infrastructure code from your Terraform template so that your resources are not re-added when you apply a new version of your Terraform template. 

**What happens if I choose to delete my resource directly from the resource dashboard?** </br>
When you manually remove a resource that you provisioned with {{site.data.keyword.bplong_notm}}, the state file is not updated automatically and becomes out of sync. When you create your next Terraform execution plan or apply a new template version, {{site.data.keyword.bpshort}} verifies that the {{site.data.keyword.cloud_notm}} resources in the state file exist in your {{site.data.keyword.cloud_notm}} account with the state that is captured. If the resource is not found, the state file is updated and the Terraform execution plan is changed accordingly. 

Although the state file is updated before new changes to your {{site.data.keyword.cloud_notm}} resources are applied, do not manually remove resources from the resource dashboard to avoid unexpected results. Instead, use the {{site.data.keyword.bplong_notm}} console or CLI to remove your resources, or remove the associated infrastructure code from your Terraform template. 
{: important}

**Are my resources removed when I remove the workspace** </br>
Removing the workspace from {{site.data.keyword.bplong_notm}} does not remove any of your {{site.data.keyword.cloud_notm}} resources. If you remove the workspace before you removed your resources, you must manually remove all of your {{site.data.keyword.cloud_notm}} resources from the individual resource dashboard. 

Removing an {{site.data.keyword.cloud_notm}} resource cannot be undone. Make sure that you backed up your data before you remove a resource. If you choose to remove the infrastructure code, or comment out the resource in your Terraform configuration file, make sure to thoroughly review the log file of your execution plan to verify that all your resources are included in the removal.    
{: important}

**To remove your resources by deleting the infrastructure code from your Terraform template**: 

1. Open the Terraform configuration file in your source repository in GitHub that includes the infrastructure code of your resource. 
2. Either remove the infrastructure code from the file, or comment out the resources that you want to remove by adding `#` to the beginning of each line. 

   Example for commenting out a resource: 
   ```
   ...
   #resource ibm_is_instance "vsi1" {
   #  name    = "${local.BASENAME}-vsi2"
   #  vpc     = ibm_is_vpc.vpc.id
   #  zone    = "${local.ZONE}"
   #  keys    = [data.ibm_is_ssh_key.ssh_key_id.id]
   #  image   = data.ibm_is_image.ubuntu.id
   #  profile = "cc1-2x4"

   #  primary_network_interface {
   #    subnet          = ibm_is_subnet.subnet1.id
   #    security_groups = [ibm_is_security_group.sg1.id]
   #  }
   #}
   ```
   {: codeblock}

3. Commit the change to your Terraform configuration file. 
4. From the [workspace dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/schematics/workspaces), select the workspace that points to the Terraform template that you just changed. 
5. From the navigation, select **Settings**. 
6. In the **Summary** section, click **Pull latest** to get the latest version of your Terraform template from the linked GitHub source repository. 
7. Click **Generate plan** to create a Terraform execution plan. The workspace **Activity** page opens. Note that during this time, you cannot make changes to your workspace.
8. Click **View log** to review the log files of your execution plan. The log files provide a summary of all the resources that {{site.data.keyword.bplong_notm}} is about to remove. 
9. Click **Apply plan** to remove the {{site.data.keyword.cloud_notm}} resources from your account. 
10. Review the log files to ensure that no errors occurred during the deletion process. 
11. From the navigation, select **Resources** and verify that your resources are removed. 
12. Optional: After you removed all your resources, remove your workspace. 
    1. Open the [workspace dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/schematics/workspaces) and find the workspace that you want to remove. 
    2. From the actions menu, click **Delete**. 
    3. Select **Delete workspace**. 
    4. Confirm the deletion of your workspace by clicking **Delete**. 


**To remove your resources from the {{site.data.keyword.bpshort}} console**: 

1. From the [workspace dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/schematics/workspaces), find the workspace that includes the resources that you want to delete. 
2. From the action menu, click **Delete**. 
3. Select **Delete workspace** and **Delete all associated resources** to delete the resources and your workspace. 
4. Confirm the deletion of your workspace by clicking **Delete**. 
5. From the navigation, select **Activity** to review the logs for your resource deletion. Ensure that no errors occurred during the deletion process. 
6. From the navigation, select **Resources** and verify that your resources are removed. 

After the deletion of your resources is complete, the {{site.data.keyword.bplong_notm}} workspace is removed. 
