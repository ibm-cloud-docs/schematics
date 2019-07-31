---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-31"

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

# Setting up workspaces
{: #workspace-setup}

An {{site.data.keyword.cloud_notm}} Schematics workspace is a collection of all the files that Schematics needs to successfully provision your {{site.data.keyword.cloud_notm}} resources by using Terraform. Before you create your workspace, make sure that you design the organizational structure of your GitHub repository and workspaces so that you can replicate and manage your configurations across multiple environments. 
{: shortdesc} 

## Designing your workspace and GitHub repository structure
{: #structure-workspace}

Plan out the organizational structure of your workspaces and GitHub repository so that they match the microservice and permission structure of your organization. 
{: shortdesc}

### How many workspaces do I need?
{: #plan-number-of-workspaces}

To find out how many workspaces you need in {{site.data.keyword.cloud_notm}} Schematics, look at the microservices that build your app and the environments that you need to develop, test, and publish your microservice. 
{: shortdesc}

As a rule of thumb, consider creating separate workspaces for each of your microservices and the environment that you use. For example, if you have a product app that consists of a search, payment, and review microservice component, consider creating a separate workspace for each microservice component and development, staging, and production environment. Separate workspaces for each microservice component and environment allows you to separately develop, deploy, and manage the Terraform configuration files and associated {{site.data.keyword.cloud_notm}} resources without affecting the resources of other microservices. 

Review the following image to see the number of workspaces in {{site.data.keyword.cloud_notm}} Schematics for an app that consists of three microservices. 

<img src="images/workspace-structure.png" alt="Workspace structure for {{site.data.keyword.cloud_notm}} Schematics" width="800" style="width: 800px; border-style: none"/>

Do not use one workspace to manage an entire staging or production environment. When you deploy all of your {{site.data.keyword.cloud_notm}} resources into a single workspace, it can become difficult for various teams to coordinate updates and manage access for these resources.
{: important}

### How do I structure my GitHub repository to map my workspaces?
{: #plan-github-structure}

Structure your GitHub repository so that you have one repository for all your Terraform configuration files that build your microservice, and use variables, or directories to differentiate between your development, staging, and production environment. 
{: shortdesc}

Review the following table to find a list of options for how to structure your GitHub repository to map the different workspace environments. 
{: shortdesc}

| Option | Description | 
| ------- | ---------------------------- | 
| One GitHub repo, use variables to distinguish between environments | Create one GitHub repository where you store the Terraform configuration files that make up your microservice component. Make your Terraform configuration files as general as possible so that you can reuse the same configuration across your environments. To configure the specifics of your development, staging, and production environment, use input variables in your configuration files. Input variables are automatically loaded into {{site.data.keyword.cloud_notm}} Schematics when you create your workspace. Customize your workspace by entering values for your variables that are specific to the environment. This setup is useful if you have one team that manages the lifecycle of the microservice component and where the configuration of your environments does not differ drastically. |
| One GitHub repo, use directories to distinguish between environments | For organizations that prefer short-lived branches, and where configurations differ drastically across environments, consider creating directories that represent the different configurations of your environments. With this setup, all your directories listen for changes that are committed to the `master` branch. Make sure to set up a process for how changes in one configuration file are populated across directories to avoid that you have different configurations in each environment. |
| Use one GitHub repo per environment | Use one GitHub repository for each of your environments. With this setup, you have a 1:1 relationship between your workspace and GitHub repository. While this setup allows you to apply separate permissions for each of your GitHub repositories, you must make sure that your team can manage multiple GitHub repositories and keep them in sync. | 



For more information about how to structure your GitHub repository, see [Repository Structure](https://www.terraform.io/docs/enterprise/workspaces/repo-structure.html){: external}. 

### How can I reuse configuration files across environments and workspaces? 
{: #plan-reuse}

Try to minimize the number of Terraform configuration files that you need to manage by creating standardized resource templates and using variables to customize the template to your needs. 
{: shortdesc}

With standardized resource templates, you can ensure that development best practices are followed within your organization and that all Terraform configuration files have the same structure. Knowing the structure of a Terraform configuration file makes it easier for your developers to understand a file, declare variables, contribute to the code, and troubleshoot the errors. 

### How do I control access to my workspaces? 
{: #plan-workspace-access}

{{site.data.keyword.cloud_notm}} Schematics is fully integrated with {{site.data.keyword.cloud_notm}} Identity and Access Management. To control access to a workspace, and who can execute your infrastructure code with {{site.data.keyword.cloud_notm}} Schematics, see [Managing user access](/docs/schematics?topic=schematics-access). 

## Creating workspaces
{: #create-workspace}

Create your workspace that points to the GitHub repository that hosts your Terraform configuration files by using the {{site.data.keyword.cloud_notm}} Schematics console. 
{: shortdesc}

**Before you begin**
- [Create a Terraform configuration](/docs/schematics?topic=schematics-create-tf-config), and store the configuration in a GitHub or GitLab repository. 
- Make sure that you have the [required permissions](/docs/schematics?topic=schematics-access) to create a workspace. 

**To create a workspace**:
1. From the {{site.data.keyword.cloud_notm}} menu, select [**Schematics**](https://cloud.ibm.com/schematics/overview){: external}. 
2. Click **Create a workspace**. 
3. Configure your workspace. 
   1. Enter a descriptive name for your workspace. When you create a workspace for your own Terraform template, consider including the microservice component that you set up with your Terraform template and the {{site.data.keyword.cloud_notm}} environment where you want to deploy your resources in your name. For more information about how to structure your workspaces, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace).
   2. Optional: Enter tags for your workspace. You can use the tags later to find workspaces that are related to each other. 
   3. Optional: Enter a description for your workspace.
   4. Enter the link to your public GitHub repository. The link must point to the `master` branch in GitHub. You cannot link to other branches during the beta. 
   
   5. Click **Retrieve input variables**. {{site.data.keyword.cloud_notm}} Schematics automatically parses through your files to find variable declarations. 
   6. In the **Input variables** section, enter the values for your input variables. 
4. Click **Create** to create your workspace. When you create the workspace, all Terraform configuration files are loaded into {{site.data.keyword.cloud_notm}} Schematics, but your resources are not yet deployed to {{site.data.keyword.cloud_notm}}. 
5. [Create an execution plan for your workspace](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources). 

## Locking and unlocking workspaces 
{: #lock-workspace}

As the {{site.data.keyword.cloud_notm}} account owner or an {{site.data.keyword.cloud_notm}} Schematics user who is assigned the **Manager** IAM service access role for Schematics, you can lock a workspace so that you cannot create a Terraform execution plan or run your infrastructure code to provision or configure your resources. 
{: shortdesc}

Before you begin, make sure that you are assigned the [**Manager** IAM service access role](/docs/schematics?topic=schematics-access) for {{site.data.keyword.cloud_notm}} Schematics. 

**To lock a workspace**: 
1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that you want to lock. 
2. In the **State** section on the workspace details page, set the toggle to **Frozen**. The user ID and a timestamp are automatically logged. After you locked a workspace, no user can generate a Terraform execution plan or apply the plan in {{site.data.keyword.cloud_notm}}. 

**To unlock a workspace**: 
1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that you want to unlock. 
2. In the **State** section on the workspace details page, set the toggle to **Unfrozen**. The user ID and a timestamp are automatically logged. After you unlocked a workspace, you can generate new Terraform execution plans or run your infrastructure code by applying the plan in {{site.data.keyword.cloud_notm}}.


## Overview of workspace states
{: #workspace-states}

The state of a workspace indicates if you have successfully created a Terraform execution plan and applied the plan to provision your resources in your {{site.data.keyword.cloud_notm}} account. 
{: shortdesc}

| State | Description | 
| ------- | ---------------------------- | 
| Active | After you successfully ran your infrastructure code with {{site.data.keyword.cloud_notm}} Schematics by applying your Terraform execution plan, the state of your workspace changes to **Active**. 
| Failed | If errors occur during the creation of the Terraform execution plan or when you instruct {{site.data.keyword.cloud_notm}} Schematics to run your infrastructure code, your workspace state is set to **Failed**. |
| Inactive | Every workspace that you create is set up with an **Inactive** state by default. This state indicates that no {{site.data.keyword.cloud_notm}} resources are provisioned with {{site.data.keyword.cloud_notm}} Schematics yet. If you have an **Active** workspace and decide to remove all your resources, your workspace is set to **Inactive** after all your resources are removed.  |
| In progress | When you instruct {{site.data.keyword.cloud_notm}} Schematics to run your infrastructure code by applying your Terraform execution plan, the state of our workspace changes to **In progress**. |
