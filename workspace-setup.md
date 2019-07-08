---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-08"

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

# Setting up your workspace
{: #workspace-setup}

An {{site.data.keyword.cloud_notm}} Schematics workspace is a collection of all the files that Schematics needs to successfully provision your {{site.data.keyword.cloud_notm}} resources by using Terraform. 
{: shortdesc} 

## Designing your workspace structure
{: #plan-workspace}

Plan out the organizational structure of your workspaces so that they match the mircoservice and permission structure of your organization. 
{: shortdesc}

Similar to splitting up a huge monolith app into smaller microservices, 

### How many workspaces do I need?
When you set up your workspaces, you want to make sure that your workspaces match the organizational structure of your microservices and the permissions that you want to assign to each component.
{: shortdesc}

For example, if you have a product app that consists of a search, payment, and review microservice component, consider creating a separate workspace for each microservice component and each environment such as development, staging, and prod. Separate workspaces for each microservice component and environment allows you to separately develop, deploy, and manage the Terraform configuration files and associated {{site.data.keyword.cloud_notm}} resources without affecting the resources of other microservices. 

Do not use one workspace to manage an entire staging or production environment. When you deploy all of your {{site.data.keyword.cloud_notm}} resources into a single workspace, it can become difficult for various teams to coordinate updates and manage access for these resources.
{: important}

Review the following image to see the number of workspaces in {{site.data.keyword.cloud_notm}} Schematics for an app that consists of three microservices. 

<img src="images/workspace-structure.png" alt="Workspace structure for {{site.data.keyword.cloud_notm}} Schematics" width="800" style="width: 800px; border-style: none"/>

For more informa

### How do I structure my GitHub repository to map my workspaces?

- Workspace is a collection of everything TF needs to run: TF config file, variable values, state data to keep track of operations between runs
- Workspaces should match your organizational permissions structure > one workspace for each environment of a given infrastructure (TF configurations * environments = number of workspaces)
- Don’t use a single workspace to manage your entire prod or staging environment 
- Make smaller workspaces so that you can delegate those and allow parallel development
- Name the workspace component-environment
*  provision infrastructure with Terraform, without conflicts and with clear understanding of their access permissions.
* Expert users within an organization can produce standardized infrastructure templates, and beginner users can consume those to follow infrastructure best practices for the organization.
- Design the workspace structure https://www.terraform.io/docs/enterprise/workspaces/repo-structure.html)
- Create the workspace
- Plan and create teams
- Assign permissions

### Reusing common configs

### How do I control access to my workspaces? 
To control access to a w

Before you create environments, it can be helpful to plan out the organizational structure of your environments and learn how to use Schematics as part of your continuous delivery pipeline.



Within each microservice, create different environments for development, testing, and production landscapes by reusing common configurations
If you want to control who can change a certain environment, you can control write access by deploying environments into different accounts. Otherwise, you can create all three environments in the same account.

Figure 1. User access with environments in a single account

Figure 1. User access with environments in a single account

Figure 2. User access with environments in multiple accounts

Figure 2. User access with environments in multiple accounts

To replicate the same architecture in each environment, reuse common configurations or templates. Because you treat your infrastructure as code with Terraform, you can spin up the same resources in each environment as part of your regular continuous integration/continuous delivery pipeline. For example, you can spin up a temporary QA environment that looks like production and tear it down when you're done with testing.

Figure 3. Reusing common configurations in multiple environments

Figure 3. Reusing common configurations in multiple environments

Collaborate in version control
Because you treat your infrastructure as code with Terraform, a version control system is imperative as part of the deployment development process. Version control allows you to revert to previous configurations, audit changes to configurations, and share code with multiple teams. Version control also allows you to use a master branch that serves as the single source of truth for your infrastructure. Team members can then plan changes in branches before they merge those changes into the master branch.



Writing configurations
If you bring your own Terraform configuration to Schematics, the following recommendations can help you better write configurations that are well-structured, reusable, and comprehensive. For more information about configuration anatomy, see Creating a configuration.

Use input variables to pass credentials instead of embedding credentials in configurations
Because configurations are reusable, do not embed your personal credentials into configurations. Instead, use input variables to pass credentials into configurations. When you use the GUI to pass these variable values, you can mask these sensitive values from other users in the organization by clicking the eye icon.

Provide a default value to make a variable optional
You can use the default parameter to set a default value for a variable. However, providing a default value automatically makes the variable optional. If no default value is provided, then the variable is required.

For example, the following API key variable declaration has no default value, requiring a user-input value.

"softlayer-api-key" {
  description = "Your IBM Cloud Infrastructure (SoftLayer) API key."
}
However, the following variable declaration for a load balancer service group port has a default value of 80.

variable "lb-service-group-port" {
default = 80
description = "The port for the local load balancer service group."
}
Use a dedicated file to store output declarations, and if your configuration is highly modularized, a dedicated file to store variable declarations
It is common practice to use a dedicated file, often called outputs.tf, to store your output variable declarations. It is common practice to also use a dedicated file to store variable declarations, especially if your configuration is simple and your components are highly modularized. For example, the NGINX Auto Scale Group template External link icon is fairly modularized, and declares variables in output.tf and variables.tf files. But for more complex configurations, store your variable declarations with the resource file that calls it so that it's easier for anyone who reads your configuration to map the variables to the resources.

Use tags and notes to provide information about your resources
If the resource you are defining supports tags, use them to label your resource. Labels can help you organize your resources according to dimensions like which environment the resource is in. Currently, tags are managed locally and are not stored on the IBM Cloud service endpoint.

If the resource you are defining supports notes, use them to add comments to the resource. Notes can help other contributors understand the purpose of the resource, how it interacts with other resources in the environment, or other related considerations.

Updating and managing
The following recommendations can help you use Schematics to safely manage and update your resources. For more information about how to update resources, see Managing resources in your environment.

Use only Schematics to change a deployed environment
When you deploy an environment with Schematics, always use the Schematics GUI, CLI, or API to change the environment. Schematics manages the statefiles that are used to compare previous configurations to new configurations when you change your resources. If you do not use Schematics, changes you make can lead to mismatches within the Terraform files. These mismatches cause errors in your deployments. For more information about state, see the Terraform state documentation External link icon.

Run plan to check for errors and preview changes to your environment before you run apply
When you run plan, Schematics extracts the variables in your environment details and the latest version of your Terraform configuration from source control. The plan output displays how the configuration compares to what is deployed according to your Terraform state file. The output allows you to see how your changes might affect resources already running before you commit those changes.

Running plan becomes especially important when you work with configurations that teams contribute to collaboratively under version control. The plan helps you see whether the configuration file you are using might have changed since your last plan.

Use API calls to audit changes to environments
Version control systems provide you with a way to audit changes to configurations. However, if you want to audit changes that are made to an environment, you can use the Schematics REST API External link icon. The call GET /v1/environments/{id}/activities retrieves a list of all activities that occurred in a specified environment, and the call GET /v1/environments/{id}/activities/{activity_id} lists details for a specific activity that ran against an environment.


## Deciding on your Schematics template



## Creating your workspace 

from the UI, CLI, API

## Managing access to your workspace
