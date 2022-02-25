---

copyright:
  years: 2017, 2022
lastupdated: "2022-02-25"

keywords: automate continuous deployment using Schematics, automate continuous deployment of resource using Schematics and DevOps toolchain, continuous deployment of resources

subcollection: schematics

content-type: tutorial
services: schematics, continuous-deployment
account-plan: lite
completion-time: 60m

---

{{site.data.keyword.attribute-definition-list}}

# Setting up continuous deployment with {{site.data.keyword.bpshort}} and DevOps toolchain
{: #workspace-continuous-deployment}
{: toc-content-type="tutorial"}
{: toc-services="schematics, continuous-deployment"}
{: toc-completion-time="60m"}

## Description
{: #workspace-desc}

In this tutorial, you can learn to use your credentials and an API key to use a Terraform template of {{site.data.keyword.cos_full}} in the {{site.data.keyword.bpshort}} workspace. Then, you also learn to automate the continuous deployment by using DevOps delivery pipeline. As part of the tutorial, you will use `ibm_cos_bucket` Terraform template example.

The `ibm_cos_bucket` example creates an instance of {{site.data.keyword.cos_full_notm}}, {{site.data.keyword.cloud}} Activity Tracker, and {{site.data.keyword.monitoringfull}}. 
{: shortdesc}

Costs are incurred based on your resource usage. For more information, about the pricing, refer to [Pricing](/docs/billing-usage?topic=billing-usage-charges). About the support and help, refer to [{{site.data.keyword.bpshort}} help](/docs/schematics?topic=schematics-schematics-help).
{: important}

## Objectives
{: #workspace-obj}

In this tutorial, you can:
- Explore an IBM provided Terraform template to create an {{site.data.keyword.cloud_notm}} Object Storage instance that binds with the {{site.data.keyword.IBM_notm}} resource instance, and {{site.data.keyword.IBM_notm}} resource group.
- Learn how to create an {{site.data.keyword.bplong_notm}} workspace.
- Learn to automate continuous deployment of a resource by using {{site.data.keyword.bplong_notm}} and DevOps toolchain.
- Review the {{site.data.keyword.cloud_notm}} resources that you create.

## Time required
{: #workspace-timereq}

1 hour

## Audience
{: #workspace-tut-audience}

This tutorial is intended for developer and system administrators who want to learn how to use Terraform templates to create and automate the continuous deployment of resource by using {{site.data.keyword.bplong_notm}} and DevOps toolchain.

## Prerequisites
{: #workspace-prereq}

**About {{site.data.keyword.bplong_notm}}:**

[{{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-getting-started) is an {{site.data.keyword.cloud_notm}} automation tool. It provides simplified provisioning, orchestrating Infrastructure as  Code (IaC), templates, and managing {{site.data.keyword.cloud_notm}} resources in your {{site.data.keyword.cloud_notm}} environment by using various resources tools such as Terraform, Helm and so on.
IaC helps you codify your cloud environment to automate the provisioning, speeds deployment and managing your resources. The infrastructure is treated the same way as your app code, so that you can automate the DevOps core practices such as version control, testing, continuous integration and deployment.
{: shortdesc}

**About DevOps toolchain:**

A DevOps toolchain is a set of tools that automates the tasks of developing and deploying your app. A toolchain is a set of tool integrations that support development, deployment, and operations tasks. The collective power of a toolchain is greater than the sum of its individual tool integrations.

For the information, about the importance of using an {{site.data.keyword.cloud_notm}} toolchain and continuous delivery of your app, refer to [DevOps toolchain](/docs/apps?topic=apps-devops-toolchains).
{{site.data.keyword.bpshort}} provides option to enable the continuous delivery of your infrastructure configurations as well with {{site.data.keyword.cloud_notm}} toolchain.

Complete the following prerequisites for the tutorial:

- If you do not have {{site.data.keyword.cloud_notm}} account, create an {{site.data.keyword.cloud_notm}} account and pay as you use. For more information, about managing {{site.data.keyword.cloud_notm}} account, refer to [Managing IBM Cloud account](https://cloud.ibm.com/registration).
- Install the {{site.data.keyword.cloud_notm}} command-line and the {{site.data.keyword.bpshort}} command-line plug-in. For more information, about command-line setup, see [{{site.data.keyword.bpshort}} command-line setup](/docs/schematics?topic=schematics-setup-cli).
- Ensure you are assigned the required permissions in {{site.data.keyword.iamlong}} to create and work with {{site.data.keyword.bplong_notm}} workspace. Refer to [{{site.data.keyword.bpshort}} access](/docs/schematics?topic=schematics-access#access-roles) and to create an {{site.data.keyword.cos_full_notm}} service instance. 
- Follow the instructions to ensure you are assigned the required permissions in {{site.data.keyword.iamshort}} to create resources. For more information, about create {{site.data.keyword.cos_full_notm}}, refer to [{{site.data.keyword.cos_full_notm}}](/docs/cloud-object-storage?topic=cloud-object-storage-provision).

## Accessing the {{site.data.keyword.cloud_notm}} and GitHub
{: #access-automate-template}
{: step}

Complete these steps to access the {{site.data.keyword.cloud_notm}} and the Terraform templates from the GitHub:
{: shortdesc}

1. If you do not have one, create an [{{site.data.keyword.cloud_notm}} Pay-As-You-Go or Subscription {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external} account. 
2. Log in to your [GitHub](https://github.com/){: external} account. 
3. Open the Terraform template to [create an {{site.data.keyword.cos_full_notm}}](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cos-bucket){: external}.
4. From the right corner of the GitHub page, click `Fork` icon to create your own fork of the shared repository.
    
    You need to copy the URL of the Terraform template of the GitHub or the GitLab Repository URL to create your {{site.data.keyword.bpshort}} workspace.
    {: note}

## Creating your {{site.data.keyword.bplong_notm}} workspace
{: #create-wkspace}
{: step}

Complete these steps to create the {{site.data.keyword.bplong_notm}} and the Terraform template URL.
{: shortdesc}

1. From the [{{site.data.keyword.bpshort}} workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, click **Create workspace**.
2. In **Specify template** section. Enter **GitHub, GitLab, or `Bitbucket` Repository URL** as 
    
    ```text
    https://github.com/IBM-Cloud/terraform-provider-ibm/blob/master/examples/ibm-cos-bucket
    ```
    {: pre}

3. For the private repository provide your **GitHub personal access token**. See the steps to fetch the [GitHub personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token).
4. Select `terraform_v01.0` from the **Terraform version** drop down.
5. Click **Next**.
6. In the **Workspace details** section, enter your **Workspace name**, **Tags**, **Resource group**, **Location**, and **Description**.

   Ensure you provide the right resource group, and the location details where you want to create the workspace.
    {: note}

7. Click **Next** and then click **Create** to create {{site.data.keyword.bpshort}} workspace successfully.

## Configuring the variables
{: #configure-variables}
{: step}

Click `...` to configure the variables as described in the table to authenticate your credentials and save the changes.
{: shortdesc}

|Name|Value|
|-----|-----|
|`iaas_classic_username`|Enter the username to access {{site.data.keyword.cloud_notm}} classic infrastructure. |
|`iaas_classic_api_key`|Enter the API key to access {{site.data.keyword.cloud_notm}} classic infrastructure. For more information, to create an API key, refer to [Classic infrastructure API keys](/docs/account?topic=account-classic_keys#create-classic-infrastructure-key).|
|`ibmcloud_api_key`|Enter your {{site.data.keyword.cloud_notm}}API Key, for more information on API key, refer to [{{site.data.keyword.cloud_notm}} API key](https://cloud.ibm.com/iam#/apikeys).|
|`resource_group_name`| Keep as default.|
{: caption="Variables" caption-side="bottom"}

## Automating the continuous deployment process
{: #continuous-deployment}
{: step}

The `Enable continuous delivery` option has the capability of automating the different Terraform actions to the {{site.data.keyword.bpshort}} workspace. Complete these steps to observe the automation of the end to end {{site.data.keyword.bplong_notm}} workspace deployment.

    The GitHub Server type parameter expects the authorization, you need to provide GitHub credentials and confirm the authorization.
    {: note}

1. On the variable page, click `Enable continuous delivery` hyperlink option to view {{site.data.keyword.bpshort}} Infrastructure as Code Tekton Toolchain page.
2. Click `Delivery Pipeline Required` tab.
3. Enter your `{{site.data.keyword.cloud_notm}} API key`. 
4. Click **Authorize** > **Authorize IBM-Cloud** and enter your GitHub password to get authorized.
5. Click `Create` to view Toolchains page.
6. Click `Deliver Pipeline` pane to view {site.data.keyword.bpshort}}-deploy Dashboard page. 
7. Click **Run Pipeline**

    Observe the UPDATE is in STAGE RUNNING state, without the button click.
    {: note}

## Analyzing the pipeline execution process
{: #analyze-deployment}
{: step}

Observe the pipeline dashboard and view the status of your workspace execution.
{: shortdesc}

1. During the update stage process, from the example of `ibm-cos-bucket` repository observe the `main.tf` file configuration with the `cos_instance` name and `bucket_name`. These details are updated in the {{site.data.keyword.bpshort}} workspace after the APPLY stage is passed.
2. Once the `UPDATE` stage is completed, PLAN stage is in running state.
3. Click **View logs** and history to view the status of the job from the `PLAN` pane.
4. Observe that the PLAN stage is passed, and APPLY stage is in running state.
5. From the {{site.data.keyword.bpshort}} workspace check the resource name and bucket name are created successfully.
6. Observe that the `APPLY` stage is passed and TEST stage is in Running state.
7. Observe that the `TEST` stage is passed successfully.
8. Now, you can edit your template in the configured repository and observe that there is an automatic pull of your workspace by the continuous delivery toolchain.

## Analyzing the {{site.data.keyword.bpshort}} workspace
{: #analyze-workspace-process}
{: step}

Alternatively, through the {{site.data.keyword.cloud_notm}} dashboard, you can view the status of the workspace.
{: shortdesc}

1. From the [{{site.data.keyword.bpshort}} workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}.
2. Select `Navigation Menu > Schematics > Workspaces > Resources` to observe the apply state of the resources in your workspace.
3. You can view the output from your working directory, or from the {{site.data.keyword.cloud_notm}} dashboard plan logs to view the workspace status.

Congratulations! You successfully created the {{site.data.keyword.bplong_notm}} workspace and automated the end to end deployment by using the DevOps toolchain. 

## What's next?
{: #automate-what's next}

You can now learn how to set up a continuous delivery pipeline for an IBM cluster. For more information, refer to [Setting up a continuous delivery pipeline for an IBM cluster](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cluster).


