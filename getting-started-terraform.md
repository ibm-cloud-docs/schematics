---

copyright:
  years: 2017, 2022
lastupdated: "2022-02-16"

keywords: get started with schematics, infrastructure management, infrastructure as code, iac, schematics cloud environment, schematics infrastructure, schematics terraform, terraform provider
subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Getting started with infrastructure and cloud service deployment in {{site.data.keyword.bplong_notm}}
{: #get-started-terraform}

Use one of the IBM provided templates to create an [{{site.data.keyword.cos_full}}](/docs/cloud-object-storage?topic=cloud-object-storage-about-cloud-object-storage) service instance that you can use to persistently store your data in {{site.data.keyword.cloud_notm}}. 
{: shortdesc}

A {{site.data.keyword.bplong_notm}} template is a set of files that define the {{site.data.keyword.cloud_notm}} resources that you want to create, update, or delete. You create a {{site.data.keyword.bpshort}} workspace that points to your template and use the built-in capabilities of the {{site.data.keyword.cloud_notm}} provider plug-in for Terraform to provision your {{site.data.keyword.cloud_notm}} resources. For more information about the provider and how {{site.data.keyword.bpshort}} spins up your {{site.data.keyword.cloud_notm}} resources, see [Infrastructure deployment with {{site.data.keyword.bpshort}} workspaces](/docs/schematics?topic=schematics-about-schematics#how-to-workspaces). 

## Before you begin
{: #prereq}

Before you can use this template, you must complete the following tasks. 
{: shortdesc}

- Make sure that you have the permissions to [create a {{site.data.keyword.bpshort}} workspace](/docs/schematics?topic=schematics-access#access-roles). 
- Make sure that you have the permissions to [create an {{site.data.keyword.cos_full_notm}} instance](/docs/cloud-object-storage?topic=cloud-object-storage-iam). 

## Creating an {{site.data.keyword.cos_full_notm}} instance with {{site.data.keyword.bpshort}}
{: #create-cos}

Use the IBM-provided Terraform template to provision an {{site.data.keyword.cos_full_notm}} instance with a {{site.data.keyword.bpshort}} workspace. 
{: shortdesc}

1. From the [{{site.data.keyword.bpshort}} workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, click **Create workspace**.
2. In **Specify template** panel. Enter GitHub, GitLab, or `Bitbucket` **Repository URL** as 
    ```sh
    https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-resource-instance
    ```
    {: codeblock}

3. Select `terraform_v0.12` from the **Terraform version** drop down.
4. Click **Next**.
5. In the **Workspace details** panel, enter your **Workspace name**, **Tags**, **Resource group**, **Location** where you want to create the workspace, and **Description**.

    Ensure you provide the right resource group, and location details.
    {: note}

6. Click **Next** and then click **Create** to create {{site.data.keyword.bpshort}} workspace successfully.
7. In the **Variables** panel, template variables are displayed. Optionally, override the variables by referring the [readme file](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-resource-instance){: external}. 
    
    If you already have an existing {{site.data.keyword.cos_full_notm}} instance in your account, you must enter `standard` in the **plan** field. 
    {: tip}

8. Click **Generate plan** to see the plan is generated successfully.
9. Click **Apply plan**. This process might take a few minutes to complete. Click **Jobs** to see the details of the provisioning process.

### Output
{: #create-cos-output}

View the provisioned {{site.data.keyword.cos_full_notm}} instance. 
1. From the [{{site.data.keyword.cloud_notm}} resource list](https://cloud.ibm.com/resources), select the **Storage** to view the provisioned {{site.data.keyword.cos_full_notm}} instance.
2. For more information, to create {{site.data.keyword.cos_full_notm}} bucket, see [create some buckets to store your data](/docs/cloud-object-storage?topic=cloud-object-storage-getting-started-cloud-object-storage#gs-create-buckets).



Congratulations! You used the built-in Terraform capabilities of {{site.data.keyword.bpshort}} to create an {{site.data.keyword.cos_full_notm}} service instance in your {{site.data.keyword.cloud_notm}} account. 


## What's next? 
{: #whats-next}

Now that you created your first {{site.data.keyword.cloud_notm}} resource with {{site.data.keyword.bpshort}}, you can explore the following options: 

- Try out this [IBM-provided template](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cos-bucket){: external} to create a bucket in the {{site.data.keyword.cos_full_notm}} instance that you created with {{site.data.keyword.bpshort}}. 
- Learn how to [create your own Terraform template](/docs/schematics?topic=schematics-create-tf-config). 
- Explore other [IBM-provided templates](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples){: external}.
- Set up the {{site.data.keyword.bpshort}} [CLI](/docs/schematics?topic=schematics-setup-cli) or [API](/docs/schematics?topic=schematics-setup-api) to start automating {{site.data.keyword.cloud_notm}} resources. 



