---

copyright:
  years: 2017, 2023
lastupdated: "2023-01-04"

keywords: get started with schematics, infrastructure management, infrastructure as code, iac, schematics cloud environment, schematics infrastructure, schematics terraform, terraform provider
subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Software deployment in {{site.data.keyword.bplong_notm}}
{: #get-started-software}

Try out one of the {{site.data.keyword.IBM}} provided software templates to quickly spin up a classic Virtual Server Instance (VSI), and automatically configure the instance to connect to an {{site.data.keyword.databases-for-postgresql_full}} instance.
{: shortdesc}

With {{site.data.keyword.bplong_notm}}, you can choose from a wide variety of [software and infrastructure templates](https://cloud.ibm.com/catalog#software){: external} that you can use to set up {{site.data.keyword.cloud_notm}} services, and to install {{site.data.keyword.IBM_notm}} and Third party software. The templates are applied by using the built-in `Terraform`, `Ansible`, `Helm`, `CloudPak`, and `Operator` capabilities in {{site.data.keyword.bpshort}}.

As part of this getting started tutorial, you create a {{site.data.keyword.bpshort}} Workspaces that points to the [VSI database](https://cloud.ibm.com/catalog#about){: external} template. Then, you run this template and watch {{site.data.keyword.bpshort}} provision your VSI and your {{site.data.keyword.databases-for-postgresql_full_notm}} instance. {{site.data.keyword.databases-for-postgresql_full_notm}} is a fully managed database offering in {{site.data.keyword.cloud_notm}} that supports storing of non-relational and relational data types. For more information about this offering, see [What is PostgreSQL?](https://www.ibm.com/cloud/databases-for-postgresql){: external}. 

This getting started tutorial incurs costs. You must have an [{{site.data.keyword.cloud_notm}} Pay-As-You-Go or Subscription](https://cloud.ibm.com/registration){: external} account to proceed. Make sure that you review pricing information for [classic VSIs](https://cloud.ibm.com/gen1/infrastructure/provision/vs){: external} and [PostgreSQL](https://cloud.ibm.com/databases/databases-for-postgresql/create){: external}. 
{: important}

## Before you begin
{: #vsi-postgres-prereq}

Before you can use this template, you must complete the following tasks.

- Make sure that you have the permissions to [create classic virtual servers](/docs/virtual-servers?topic=virtual-servers-managing-device-access). 
- [Create a classic API key](/docs/account?topic=account-classic_keys#create-classic-infrastructure-key) and retrieve your classic infrastructure username. This username and API key are used to verify that you have sufficient permissions to create classic infrastructure. 
- Make sure that you have the permissions to create an [{{site.data.keyword.databases-for-postgresql_full_notm}} instance](/docs/databases-for-postgresql?topic=cloud-databases-iam). 


## Setting up and configuring a classic VSI to run PostgreSQL with {{site.data.keyword.bpshort}}
{: #vsi-postgres}

Use one of the IBM provided software templates to set up and configure a classic VSI so that you can store data in an instance of {{site.data.keyword.databases-for-postgresql_full_notm}}. 
{: shortdesc}

1. Open the [**VSI database** software template](https://cloud.ibm.com/catalog/content/VSI-database){: external} from the {{site.data.keyword.cloud_notm}} catalog. Click **Log in**, in case you haven't log in to your {{site.data.keyword.cloud_notm}} account.

   Observe {{site.data.keyword.cloud_notm}} is specified in **Select your deployment target** and `Terraform Version 1.0.7` or `Terraform Version 2.0.0` is displayed in **Select a delivery method**.
   {: note}

2. In the **Configure your workspace** section, enter **Name** for your {{site.data.keyword.bpshort}} Workspace, select your **Resource group**, and the **Location** where you want to create the workspace.
3. Check **Override default Terraform version** to configure the template to support your Terraform version. 
4. In the **Set the deployment values** section, enter the following information. 
    - Click **Yes** toggle button to enter value for **admin-password** as `user123`, and **db-user-password** as `user123` that you want to use to log in to your PostgreSQL instance. 
    
      The `admin-password` and `db-user-password` must be between 10 and 32 characters long and do not support any special characters. 
      {: note}

    - Enter the **iaas_classic_username** as `<your classic_username>` that you retrieved earlier. For more information about how to retrieve this information, see [Creating a classic infrastructure API key](/docs/account?topic=account-classic_keys#create-classic-infrastructure-key). 
    - Select the resource group where you want to provision your virtual server and `PostgresSQL` instance. 

5. Accept the license agreement, and click **Install**. You are redirected to the {{site.data.keyword.bpshort}} Workspaces **Activity** page where you can monitor the progress of your VSI and PostgreSQL setup. Note that it takes a few minutes for the setup to complete. 
6. Verify your virtual server and PostgreSQL setup. 
    - From the workspace **Resources** page, find the virtual server and PostgreSQL instance that were created for you. 
    - Click the link to see the details of your instances. 
7. Optional: Remove your {{site.data.keyword.bpshort}} Workspaces and all related {{site.data.keyword.cloud_notm}} resources. 
    - Select the **Actions** drop down list, click **Delete**. 
    - Select the **Delete workspace** and **Delete all associated resources** option.
    - Enter the name of your workspace, and click **Delete**. 

You used the capabilities of {{site.data.keyword.bpshort}} to provision {{site.data.keyword.cloud_notm}} infrastructure and database services, and automatically configured your services to allow network communication. 

## What's next? 
{: #whats-next}

- Explore the capabilities of [{{site.data.keyword.databases-for-postgresql_full_notm}}](/docs/databases-for-postgresql?topic=databases-for-postgresql-getting-started).
- Browse other [software and infrastructure templates](https://cloud.ibm.com/catalog?search=label%3Aterraform#software){: external} that you can apply with {{site.data.keyword.bpshort}}.
- Learn more about the [built-in capabilities in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-learn-about-schematics).
- Import your {{site.data.keyword.bpshort}} templates and create your [private {{site.data.keyword.cloud_notm}} catalog](/docs/account?topic=account-create-private-catalog).
- Set up the {{site.data.keyword.bpshort}} [CLI](/docs/schematics?topic=schematics-setup-cli) or [API](/docs/schematics?topic=schematics-setup-api) to start automating the provisioning and management of {{site.data.keyword.cloud_notm}} resources. 
