---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-10"

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

# Managing the lifecycle of your resources
{: #manage-lifecycle}

## Deploying your resources
{: #deploy-resources}

Run your infrastructure code to provision, or modify your {{site.data.keyword.cloud_notm}} resources by using the {{site.data.keyword.cloud_notm}} Schematics console.  
{:shortdesc}

Before you begin, make sure that you [created a workspace from your GitHub repository](/docs/schematics?topic=schematics-workspace-setup#create-workspace) that hosts your Terraform configuration files. 

1. From the workspace details page, click **Run new plan** to create a Terraform execution plan. This plan equals the output of the `terraform plan` command. You can review the status of your plan in the **Recent activtity** section of your workspace details page.
2. Review the log files of your execution plan. This plan includes a summary of {{site.data.keyword.cloud_notm}} resources that must be created, modified, or deleted to achieve the state that you described in your Terraform configuration files. If you have syntax errors in your configuration files, you can review the error message in the log file. 
3. Optional: Open the **Variables** tab from the workspace details page to review the variables that you set for your workspace. The values of your variables are used everytime you reference the variable in your Terraform configuration file. 
4. Review available service plans and pricing information for each of the {{site.data.keyword.cloud_notm}} resources that you are about to create. Some services come with a limit per {{site.data.keyword.cloud_notm}} account. If you are about to reach the service limit for your account, the resource is not provisioned until you increase the service quota, or remove existing services first. 
5. When you are ready, apply your Terraform configuration by clicking **Apply plan** from the **Details** tab of the workspace details page. {{site.data.keyword.cloud_notm}} Schematics starts provisioning, modifying, or deleting your {{site.data.keyword.cloud_notm}} resources based on what actions were identified in the execution plan. Depending on the type and number of resources that you want to provision or modify, this process might take a few minutes, or even up to hours to complete. During this time, you cannot make changes to your workspace. After all updates are applied, the state of your {{site.data.keyword.cloud_notm}} resources is stored in a Terraform state file that {{site.data.keyword.cloud_notm}} Schematics uses to determine what resources exist in your {{site.data.keyword.cloud_notm}} account. 
6. Review the log file to ensure that no errors occured during the provisioning, modification, or deletion process. 
7. From the workspace details page, select the **Resources** tab to find a summary of {{site.data.keyword.cloud_notm}} resources that are available in your {{site.data.keyword.cloud_notm}} account.



To be incorporated: 
With {{site.data.keyword.bplong}}, you can deploy your latest code changes directly from your source code. When you deploy your environment, you are deploying the resources the are defined by templates or by your own Terraform configuration. You can then manage the provisioning, orchestrations, and lifecycle of the environment from within {{site.data.keyword.bpshort}}.
{:shortdesc}

You can create an environment from a template or bring your own Terraform configuration.



## Updating your resources

Deploying changes to your environment is a lightweight process. To change which resources are allocated, you code changes to your Terraform configuration in declarative syntax, meaning you state only the outcome you want. With {{site.data.keyword.bpshort}}, you can preview your changes before deployment.


After you publish code changes to your Terraform configuration in your source control repository, or modify variables in the GUI, complete the following steps to deploy updates to your environment:

1. In the **{{site.data.keyword.bpshort}}** dashboard, select **Environments**.

2. Click the row of the specific environment that you want to update.

3. Click **Plan** and inspect your plan log for errors.

4. Click **Apply** to deploy updates.



## Reviewing resource and deployment details
{: #review-logs}

You can view the change history of Terraform configurations in your source control repository, like you would with any other codebase. To monitor resource deployments to your environments, or to view logs of previous deployments, you can view audit logs with the {{site.data.keyword.bpshort}} GUI, CLI, and API.
{:shortdesc}

You can look at the environment status indicators for a quick glance at which environments are actively running resources.

To inspect resource or deployment details:

1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces), select the workspace that you want to inspect.
2. From the workspace details page, review the logs of previous Terraform execution plans and the plans that you applied in your environment in the **Recent Activity** section. 
3. To review the state of the {{site.data.keyword.cloud_notm}} resources that you created with this workspace, select the **Resources** tab of the workspace details page. 
  
## Removing your resources
{: #destroy-resources}

If you do not need your {{site.data.keyword.cloud_notm}} resources anymore, you can remove the resources by removing the workspace that manages these resources. 
{:shortdesc}

**What happens if I deleted resources manually?** </br>

**Can I remove my resources without removing the workspace?** </br>

Removing a workspace, removes all of the {{site.data.keyword.cloud_notm}} resources that are specified in the Terraform configuration files that your workspace points to. This action cannot be undone. Make sure that you backed up data before you remove workspaces. To avoid unexpected results, do not remove an entire production workspace. Instead, remove resources one at a time by removing the configuration from your Terraform files and verify the execution plan before you proceed with the removal.  
{: important}

To remove your resources: 

1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces), find the workspace that you want to remove. 
2. From the actions menu, click **Delete**. 
3. Confirm the deletion of your workspace by clicking **Delete**. After you remove your workspace, {{site.data.keyword.cloud_notm}} Schematics automatically starts removing all of the {{site.data.keyword.cloud_notm}} resources that are specified in the Terraform configuration files that your workspace points to. 

