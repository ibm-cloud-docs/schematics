---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-25"

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

# Setting up your service
{: #workspace-setup}



## Managing access
{: #manage-access}

Schematics is designed to be used by teams. Within the team is one service owner and many service users. Each have their own service instance, but their capapbilities are defined by whether they are a service owner or user, what kind of account they have, and what their user role is designated as within IAM.
{: shortdesc}

A service owner must have a paid {{site.data.keyword.cloud_notm}} account. Charges are incurred by creating resources, which can be done only in the service owner account, not in the accounts of the users. Service owners can allow access to their resources to other users. When given permission, service users can provision existing resources whether they have a paid or free {{site.data.keyword.cloud_notm}} account.

**Before you begin**

The service owner must [create a workspace](#create-workspace) in their IBM Cloud account.


To manage access from the service owner's account:
1. Click **Manage** > **Access (IAM)** > **Users**.
    
2. Click **Invite Users** to invite users to the account.

3. Enter the email address for the user.

4. In **Assign access to**, select **Resource**.

5. In **Services**, select **Schematics**.

6. Assign user access roles.
<table summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the access role in column one, and the permission descriptions in column two.">
<caption>User permissions by service user type, account type, and access role</caption>
  <thead>
  <th>Service user type</th>
  <th>{{site.data.keyword.cloud_notm}} account type</th>
  <th>Access role</th>
  <th>Permissions</th>
  </thead>
  <tbody>
    <tr>
      <td>Service owner</td>
      <td>Paid</td>
      <td>Manager</td>
      <td><ul>
          <li>Create workspace</li>
          <li>Update workspace</li>
          <li>Delete workspace</li>
          <li>View workspace</li>
          </ul></td>
    </tr>
    <tr>
      <td>Service user</td>
      <td>Paid or free</td>
      <td>Manager</td>
      <td><ul>
          <li>Update workspace</li>
          <li>View workspace</li>
          </ul></td>
    </tr>
    <tr>
      <td>Service user</td>
      <td>Paid or free</td>
      <td>Writer</td>
      <td><ul>
          <li>Update workspace</li>
          <li>View workspace</li>
          </ul></td>
    </tr>
    <tr>
      <td>Service user</td>
      <td>Paid or free</td>
      <td>Reader</td>
      <td><ul>
          <li>View workspace</li>
          </ul></td>
    </tr>
  </tbody>
  </table>

Next, depending on the permissions you assigned, the user that you added can start working with [Terraform resources](https://www.terraform.io/docs/index.html){: external}.


## Setting up workspaces

An {{site.data.keyword.cloud_notm}} Schematics workspace is a collection of all the files that Schematics needs to successfully provision your {{site.data.keyword.cloud_notm}} resources by using Terraform. Before you create your workspace, make sure that you design the organizational structure of your GitHub repository and workspaces so that you can replicate and manage your configurations across multipe environments. 
{: shortdesc} 

### Designing your workspace and GitHub repository structure
{: #structure-workspace}

Plan out the organizational structure of your workspaces and GitHub repository so that they match the mircoservice and permission structure of your organization. 
{: shortdesc}

#### How many workspaces do I need?
{: #plan-number-of-workspaces}

To find out how many workspaces you need in {{site.data.keyword.cloud_notm}} Schematics, look at the microservices that build your app and the environments that you need to develop, test, and publish your microservice. 
{: shortdesc}

As a rule of thumb, consider creating separate workspaces for each of your microservice and the environment that you use. For example, if you have a product app that consists of a search, payment, and review microservice component, consider creating a separate workspace for each microservice component and development, staging, and production environment. Separate workspaces for each microservice component and environment allows you to separately develop, deploy, and manage the Terraform configuration files and associated {{site.data.keyword.cloud_notm}} resources without affecting the resources of other microservices. 

Review the following image to see the number of workspaces in {{site.data.keyword.cloud_notm}} Schematics for an app that consists of three microservices. 

<img src="images/workspace-structure.png" alt="Workspace structure for {{site.data.keyword.cloud_notm}} Schematics" width="800" style="width: 800px; border-style: none"/>

Do not use one workspace to manage an entire staging or production environment. When you deploy all of your {{site.data.keyword.cloud_notm}} resources into a single workspace, it can become difficult for various teams to coordinate updates and manage access for these resources.
{: important}

#### How do I structure my GitHub repository to map my workspaces?
{: #plan-github-structure}

Structure your GitHub repository so that you have one repository for all your Terraform configuration files that build your microservice, and use variables, branches, or directories to differentiate between your development, staging, and production environment. 
{: shortdesc}

Review the following table to find a list of options for how to structure your GitHub repository to map the different workspace environments. 
{: shortdesc}

| Option | Description | 
| ------- | ---------------------------- | 
| One GitHub repo, use variables to distinguish between environments | Create one GitHub repository where you store the Terraform configuration files that make up your microservice component. Make your Terraform configuration files as general as possible so that you can reuse the same configuration across your workspace environments. To configure the specifics of your development, staging, and production environment, use workspace variables. This setup is useful if you have one team that manages the lifecycle of the microservice component and where your environment configurations do not differ drastically. |
| One GitHub repo, use branches to distinguish between environments | Create one GitHub repository for your microservice component, and use different GitHub branches to store the Terraform configuration files for each of your environments. With this setup, you have a clear distinction between your environments and more control over who can access and change a particular configuration. Make sure to set up a process for how changes in one configuration file are populated across branches to avoid that you have different configurations in each environment. |
| One GitHub repo, use directories to distinguish between environments | For organizations that prefer short-lived branches, and where configurations differ drastically across environments, consider creating directories that represent the different configurations of your environments. With this setup, all your directories listen for changes that are committed to the `master` branch. Make sure to set up a process for how changes in one configuration file are populated across directories to avoid that you have different configurations in each environment. |
| Use one GitHub repo per environment | Use one GitHub repository for each of your environment. With this setup, you have a 1:1 relationship between your workspace and GitHub repository. While this setup allows you to apply separate permissions for each of your environments, you must make sure that your team can manage multiple GitHub repositories and keep them in sync. | 

For more information about how to structure your GitHub repository, see [Repository Structure](https://www.terraform.io/docs/enterprise/workspaces/repo-structure.html){: external}. 

#### How can I reuse configuration files across environments and workspaces? 
{: #plan-reuse}

Try to minimize the number of Terraform configuration files that you need to manage by creating standardized resource templates and using variables to customize the template to your needs. 
{: shortdesc}

With standardized resource templates, you can ensure that development best practices are followed within your organization and that all Terraform configuration files have the same structure. Knowing the structure of a Terraform configuration file makes it easier for your developers to understand a file, declare variables, contribute to the code, and troubleshoot the errors. 

#### How do I control access to my workspaces? 
{: #plan-workspace-access}

{{site.data.keyword.cloud_notm}} Schematics is fully integrated with {{site.data.keyword.cloud_notm}} Identity and Access Management. To control access to a workspace, and who can execute your infrastructure code with {{site.data.keyword.cloud_notm}} Schematics, see [Managing access to resources](#manage-access). 

### Creating your workspace 
{: #create-workspace}

Create your workspace that points to the GitHub repository that hosts your Terraform configuration files by using the {{site.data.keyword.cloud_notm}} Schematics console. 
{: shortdesc}

**Before you begin**

To create a workspace, you must be a service owner and you must have [Terraform configuration files](/docs/schematics?topic=schematics-create-tf-config) in your repository.

To create a workspace:
1. Open the {{site.data.keyword.cloud_notm}} Schematics [catalog page](https://cloud.ibm.com/schematics/overview){: external}. 
2. Click **Create workspace**. 
3. Configure your workspace. 
   1. Enter the link to your public GitHub repository. The link must point to the `master` branch in GitHub. You cannot link to other branches during the beta. 
   2. Enter a name for your workspace. Make sure that you include the microservices component and the environment in your name. For more information about how to structure your workspaces, see [How many workspaces do I need?](#plan-number-of-workspaces).
   3. Optional: Enter tags for your workspace. You can use the tags later to find your workspaces more easily. 
   
   4. Optional: Enter a description for your workspace.
   5. Enter the values for your variables. When you enter the GitHub repository URL that hosts your Terraform configuration files, {{site.data.keyword.cloud_notm}} Schematics automatically parses through your files to find variable declarations. 
4. Click **Create** to create your workspace. When you create the workspace, all Terraform configuration files are loaded into {{site.data.keyword.cloud_notm}} Schematics, but your resources are not yet deployed to {{site.data.keyword.cloud_notm}}. 
5. [Create an execution plan for your workspace](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources). 

