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
{: #plan-number-of-workspaces}

When you set up your workspaces, you want to make sure that your workspaces match the organizational structure of your microservices and the permissions that you want to assign to each component.
{: shortdesc}

For example, if you have a product app that consists of a search, payment, and review microservice component, consider creating a separate workspace for each microservice component and each environment such as development, staging, and prod. Separate workspaces for each microservice component and environment allows you to separately develop, deploy, and manage the Terraform configuration files and associated {{site.data.keyword.cloud_notm}} resources without affecting the resources of other microservices. 

Review the following image to see the number of workspaces in {{site.data.keyword.cloud_notm}} Schematics for an app that consists of three microservices. 

<img src="images/workspace-structure.png" alt="Workspace structure for {{site.data.keyword.cloud_notm}} Schematics" width="800" style="width: 800px; border-style: none"/>

Do not use one workspace to manage an entire staging or production environment. When you deploy all of your {{site.data.keyword.cloud_notm}} resources into a single workspace, it can become difficult for various teams to coordinate updates and manage access for these resources.
{: important}

### How do I structure my GitHub repository to map my workspaces?
{: #plan-github-structure}

Review the following table to find a list of options for how to structure your GitHub repository to map the different workspace environments. 
{: shortdesc}

| Option | Description | 
| ------- | ---------------------------- | 
| One GitHub repo, use variables to distinguish between environments | Create one GitHub repository where you store the Terraform configuration files that make up your microservice component. Make your Terraform configuration files as general as possible so that you can reuse the same configuration across your workspace environments. To configure the specifics of your development, staging, and production environment, use workspace variables. This setup is useful if you have one team that manages the lifecycle of the microservice component and where your environment configurations do not differ drastically. |
| One GitHub repo, use branches to distinguish between environments | Create one GitHub repository for your microservice component, and use different GitHub branches to store the Terraform configuration files for each of your environments. With this setup, you have a clear distinction between your environments and more control over who can access and change a particular configuration. Make sure to set up a process for how changes in one configuration file are populated across branches to avoid that you have different configurations in each environment. |
| One GitHub repo, use directories to distinguish between environments | For organizations that prefer short-lived branches, and where configurations differ drastically across environments, consider creating directories that represent the different configurations of your environments. With this setup, all your directories listen for changes that are committed to the `master` branch. Make sure to set up a process for how changes in one configuration file are populated across directories to avoid that you have different configurations in each environment. |
| Use one GitHub repo per environment | Use one GitHub repository for each of your environment. With this setup, you have a 1:1 relationship between your workspace and GitHub repository. While this setup allows you to apply separate permissions for each of your environments, you must make sure that your team can manage multiple GitHub repositories and keep them in sync. | 

For more information about how to structure your GitHub repository, see [Repository Structure](https://www.terraform.io/docs/enterprise/workspaces/repo-structure.html). 

### Reusing common configs
To replicate the same architecture in each environment, reuse common configurations or templates. Because you treat your infrastructure as code with Terraform, you can spin up the same resources in each environment as part of your regular continuous integration/continuous delivery pipeline. For example, you can spin up a temporary QA environment that looks like production and tear it down when you're done with testing.


### How many teams should manage a workspace?

### How do I control access to my workspaces? 
{: #plan-workspace-access}

{{site.data.keyword.cloud_notm}} Schematics is fully integrated with {{site.data.keyword.cloud_notm}} Identity and Access Management. To control access to a workspace, and who can execute your infrastructure code with {{site.data.keyword.cloud_notm}} Schematics, see [Managing access to your workspace](#manage-workspace-access). 










- Workspace is a collection of everything TF needs to run: TF config file, variable values, state data to keep track of operations between runs


*  provision infrastructure with Terraform, without conflicts and with clear understanding of their access permissions.
* Expert users within an organization can produce standardized infrastructure templates, and beginner users can consume those to follow infrastructure best practices for the organization.
- Plan and create teams

Before you create environments, it can be helpful to plan out the organizational structure of your environments and learn how to use Schematics as part of your continuous delivery pipeline.










Collaborate in version control
Because you treat your infrastructure as code with Terraform, a version control system is imperative as part of the deployment development process. Version control allows you to revert to previous configurations, audit changes to configurations, and share code with multiple teams. Version control also allows you to use a master branch that serves as the single source of truth for your infrastructure. Team members can then plan changes in branches before they merge those changes into the master branch.




Use tags and notes to provide information about your resources
If the resource you are defining supports tags, use them to label your resource. Labels can help you organize your resources according to dimensions like which environment the resource is in. Currently, tags are managed locally and are not stored on the IBM Cloud service endpoint.

If the resource you are defining supports notes, use them to add comments to the resource. Notes can help other contributors understand the purpose of the resource, how it interacts with other resources in the environment, or other related considerations.



## Deciding on your Schematics template



## Creating your workspace 

from the UI, CLI, API

## Managing access to your workspace
{: #manage-workspace-access}
