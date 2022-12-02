---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-02"

keywords: schematics, automation, terraform

subcollection: schematics

content-type: tutorial
services: schematics
account-plan: lite
completion-time: 30m

---

{{site.data.keyword.attribute-definition-list}}

# Importing {{site.data.keyword.bpshort}} templates into the {{site.data.keyword.cloud_notm}} catalog
{: #private-catalog}
{: toc-content-type="tutorial"}
{: toc-services="schematics"}
{: toc-completion-time="30m"}

Understand how to [Create your private catalog](/docs/account?topic=account-restrict-by-user), [manage your private catalog](/docs/account?topic=account-filter-account), [assign access to the private catalog](/docs/account?topic=account-catalog-access) in {{site.data.keyword.cloud}}. And import your Terraform templates as products to make them available to your users. With a private catalog, you can limit the services that you want your users to see and the service settings that they can adjust. This way, you have more control over the type of service that is provisioned in your account and that naming conventions for services and service components are followed in your organization. 
{: shortdesc}

## Objectives
{: #private-tut-obj}

In this tutorial, you import the {{site.data.keyword.IBM_notm}} provided Observability Terraform template as a product to your private catalog to help users create the following {{site.data.keyword.cloud_notm}} services at once: 

- [**{{site.data.keyword.loganalysislong_notm}}**](/docs/log-analysis?topic=log-analysis-getting-started#getting-started) - Use this service to add logging capabilities to other {{site.data.keyword.cloud_notm}} services, and to manage system and app logs.
- [**{{site.data.keyword.monitoringlong_notm}}**](/docs/monitoring?topic=monitoring-getting-started) - Use this service to gain operational visibility into the performance and health of your apps, services, and platforms.
- [**{{site.data.keyword.cloudaccesstraillong_notm}}**](/docs/activity-tracker?topic=activity-tracker-getting-started) - Use this service to track any activity for a service so that you can comply with regulatory audit requirements.

## Time required
{: #private-timereq}

30 minutes

## Audience
{: #private-tut-audience}

This tutorial is intended for developers and system administrators who want to learn how to use Terraform templates to create and manage cloud infrastructure services by using {{site.data.keyword.bplong_notm}}.

## Prerequisites
{: #private-prerequisites}

Before you begin, make sure that you are assigned the following permissions: 
- [To create a private catalog](/docs/account?topic=account-create-private-catalog#prereq-create) in {{site.data.keyword.cloud_notm}}.
- [To create a {{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-access#workspace-permissions).
- [To create an {{site.data.keyword.loganalysislong_notm}} instance](/docs/log-analysis?topic=log-analysis-iam#platform).
- [To create an {{site.data.keyword.monitoringlong_notm}} instance](/docs/monitoring?topic=monitoring-iam#iam_platform).
- [To create an {{site.data.keyword.cloudaccesstraillong_notm}} instance](/docs/activity-tracker?topic=activity-tracker-iam#platform).
- Write access to a GitHub repository on `github.com`. This repository is needed to upload the Terraform template that you want to add as a product to your private catalog.

## Prepare your Terraform template for the private catalog
{: #prepare-tf-templates}
{: step}

To upload a Terraform template to a private catalog, you must first compress all your Terraform configuration files to a `TGZ` file, and upload this file to a GitHub repository to create a release. 
{: shortdesc}

1. Download the content of the `terraform-ibm-observability` sample repository to your local machine. This repository is owned and maintained by {{site.data.keyword.IBM_notm}}, and provides a Terraform template to create an instance of {{site.data.keyword.loganalysislong_notm}}, {{site.data.keyword.monitoringlong_notm}}, and {{site.data.keyword.cloudaccesstraillong_notm}}. 

    If you want to use your own Terraform template, make sure that you put all Terraform configuration files in to a folder on your local machine. Do not store Terraform configuration files in a `subfolder`. 
    {: tip}

    ```sh
    git clone https://github.com/Cloud-Schematics/terraform-ibm-observability.git
    ```
    {: pre}

2. Change in to the `terraform-ibm-observability` directory.
    ```sh
    cd terraform-ibm-observability
    ```
    {: pre}

3. Optional: Review the `readme.md` file and the Terraform configuration files. 
4. Compress your Terraform configuration files to create the `TGZ` file. The `TGZ` file is required to upload your Terraform template as a product to the private catalog. 

    To run this command, make sure that you are not in the directory that stores your Terraform template, but that you navigate to the parent directory one level preceding. If you use the IBM-provided observability template as part of this tutorial, make sure that you are in the `terraform-ibm-observability` directory. 
    {: note}

    If your `.tgz` file size if greater than 40 MB. Then, use `rm -rf .git .gitignore` command to reduce the size of the `.tgz` file and then create `tar czfv <reponame>.tgz .`.
    {: tip}

    ```sh
    tar czvf observability.tgz .
    ```
    {: pre}

    Example output
    ```text
    a .
    a ./output.tf
    a ./main.tf
    a ./.README.md.swp
    a ./LICENSE
    a ./observability.png
    a ./diagrams
    a ./observability.tgztar: ./observability.tgz: Can't add archive to itself

    a ./README.md
    a ./.secrets.baseline
    a ./observability.drawio
    a ./variables.tf
    a ./local.tf
    a ./version.tf
    a ./diagrams/observability.png
    a ./diagrams/observability.drawio
    ```
    {: screen}

## Creating a release
{: #create-release}
{: step}

Create a release in your source code repository to deliver and manage versions of your software. You can create new releases with release notes.

1. Find your existing repository in GitHub to upload your `TGZ` file. 
2. Open the GitHub release page for your repository by appending `/releases` to your repository URL as shown in the following example. 
    ```sh
    https://github.com/<gh_org>/<gh_repo>/releases
    ```
    {: codeblock}

3. Click **Draft a new release**. 
4. Click **Choose a tag**, type a version number, a title, and an optional description for your release. Use the tagging suggestions in the GitHub UI to find a supported tag version. 
5. If you had created a tag, use the drop-down menu to select the branch that contains the project you want to release.
6. Drag your `TGZ` file from your local machine to the **Attach binary file by dropping them here or selecting them** section. 
7. Click **Publish release** to view your published releases feed for your repository.
8. Optional: Right-click on your `TGZ` file and copy the link to the file. 
9. Enter the link in your browser to verify that the `TGZ` file is automatically downloaded to your local machine. 
10. Decompress the `TGZ` file and verify that you can see all Terraform configuration files without the `subfolder`. 

## Create a private catalog and add your Terraform template as a product
{: #create-private-catalog}
{: step}

1. Create a [private catalog in {{site.data.keyword.cloud_notm}}](/docs/account?topic=account-catalog-access).
2. Import your {{site.data.keyword.bpshort}} template as a product into your private catalog.
    1. From the **Private catalogs** page, select the private catalog that you created.
    2. Click **Add**. 
    3. Enter the URL to your `TGZ` file that you verified earlier. 
    4. Click **Add**. 
3. From the **Version list** of your product, select the product that you uploaded.
4. Go to the **Configure product** tab.
    1. In the **Configure the deployment details** section, click **Add deployment values**. The Terraform configuration files in your `TGZ` file are automatically scanned for any input variables that are defined in the template. 
    2. Select all deployment values and click **Add deployment values**. 
    3. Review the default values that are set for your deployment values. 
    4. Enter values for the `activity_tracker_service_plan`, `logdna_service_plan`, `sysdig_service_plan`, and `region` variables by clicking **Edit** from the actions menu. You can optionally change any of the other default deployment variable values. 
    5. Save your changes by clicking **Update**.
    6. Change to the **Add license agreement** tab, and add any license that the user needs to agree to. 
    7. Change to the **Edit readme** tab, and add or edit the readme for your product. 
    8. Change to the **Validate product** tab. 
       - Enter a name for the {{site.data.keyword.bpshort}} Workspaces that you want to create for the product validation. 
       - In the **Deployment values** section, verify that the default values are displayed. If you want to use different values to validate your product, change the deployment values as necessary. 
       - Click **Validate** to start the validation. During the validation, a {{site.data.keyword.bpshort}} Workspaces is created and the {{site.data.keyword.cloud_notm}} services that you defined in your Terraform templates are created. To monitor the progress of the validation in your workspace, you can click **View logs**. If the validation is successful, the status of your product changes to `Not published: Validated`. 
    9. From the **actions** menu, click **Share** to make your product available to other users in your private catalog. To provide access group and assign your catalog to users, see [Setting up the access groups](/docs/account?topic=account-groups).
    10. Optional: From the [{{site.data.keyword.cloud_notm}} **Resource list**](https://cloud.ibm.com/resources){: external}, remove the {{site.data.keyword.loganalysislong_notm}}, {{site.data.keyword.monitoringlong_notm}}, and {{site.data.keyword.cloudaccesstraillong_notm}} service instances that you created when you validated the product.

In this tutorial, you learned how to create a private catalog in {{site.data.keyword.cloud_notm}}? and How to upload an IBM-provided Terraform template as a product to your catalog?

## What's next?
{: #private_what's_next}

- [Make your private catalog available to your users](/docs/account?topic=account-restrict-by-user#prereq-restrict).
- [Assign users access to your private catalog](/docs/account?topic=account-catalog-access).
- [Explore other settings that you can apply to your private catalog](/docs/account?topic=account-filter-account).



