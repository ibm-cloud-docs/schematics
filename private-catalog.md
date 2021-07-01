---

copyright:
  years: 2017, 2021
lastupdated: "2021-07-01"

keywords: schematics, automation, terraform

subcollection: schematics

content-type: tutorial
services: schematics
account-plan: lite
completion-time: 30m

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Importing Schematics templates into the IBM Cloud catalog
{: #private-catalog}
{: toc-content-type="tutorial"}
{: toc-services="schematics"}
{: toc-completion-time="30m"}

Create your own private content catalog in {{site.data.keyword.cloud}} and import your Terraform templates as products to make them available to your users. With a private catalog, you can limit the services that you want your users to see and the service settings that they can adjust. This way, you have more control over the type of service that is provisioned in your account and that naming conventions for services and service components are followed in your organization. 
{: shortdesc}

In this tutorial, you import the IBM-provided Observability Terraform template as a product to your private catalog to help users create the following {{site.data.keyword.cloud_notm}} services at once: 

- [**{{site.data.keyword.loganalysislong_notm}}**](/docs/log-analysis?topic=log-analysis-getting-started#getting-started): Use this service to add logging capabilities to other {{site.data.keyword.cloud_notm}} services, and to manage system and app logs.
- [**{{site.data.keyword.monitoringlong_notm}}**](/docs/monitoring?topic=monitoring-getting-started#getting-started): Use this service to gain operational visibility into the performance and health of your apps, services, and platforms.
- [**{{site.data.keyword.cloudaccesstraillong_notm}}**](/docs/activity-tracker?topic=activity-tracker-getting-started): Use this service to track any activity for a service so that you can comply with regulatory audit requirements.

## Prerequisites
{: prerequisits}

Before you begin, make sure that you are assigned the following permissions: 
- [Permissions to create a private catalog](/docs/account?topic=account-create-private-catalog#prereq-create) in {{site.data.keyword.cloud_notm}}
- [Permissions to create a {{site.data.keyword.bpshort}} workspace](/docs/schematics?topic=schematics-access#workspace-permissions)
- [Permissions to create an {{site.data.keyword.loganalysislong_notm}} instance](/docs/log-analysis?topic=log-analysis-iam#platform)
- [Permissions to create an {{site.data.keyword.monitoringlong_notm}} instance](/docs/monitoring?topic=monitoring-iam#iam_platform)
- [Permissions to create an {{site.data.keyword.cloudaccesstraillong_notm}} instance](/docs/activity-tracker?topic=activity-tracker-iam#platform)
- Write access to a GitHub repository on `github.com`. This repository is needed to upload the Terraform template that you want to add as a product to your private catalog.  


## Prepare your Terraform template for the private content catalog
{: #prepare-tf-templates}
{: step}

To upload a Terraform template to a private catalog, you must first compress all of your Terraform configuration files to a `TGZ` file, and upload this file to a GitHub repository to create a release. 
{: shortdesc}

1. Download the content of the `terraform-ibm-observability` sample repository to your local machine. This repository is owned and maintained by IBM, and provides a Terraform template to create an instance of {{site.data.keyword.loganalysislong_notm}}, {{site.data.keyword.monitoringlong_notm}}, and {{site.data.keyword.cloudaccesstraillong_notm}}. 

   If you want to use your own Terraform template, make sure that you put all Terraform configuration files in to a folder on your local machine. Do not store Terraform configuration files in a subfolder. 
   {: tip}
   
   ```
   git clone https://github.com/Cloud-Schematics/terraform-ibm-observability.git
   ```
   {: pre}

2. Change in to the `terraform-ibm-observability` directory.
   ```
   cd terraform-ibm-observability
   ```
   {: pre}
   
3. Optional: Review the `readme.md` file and the Terraform configuration files that are stored in the `terraform` directory. 
4. Compress your Terraform configuration files to create the `TGZ` file. The `TGZ` file is required to upload your Terraform template as a product to the private catalog. 

   To run this command, make sure that you are not in the directory that stores your Terraform template, but that you navigate to the parent directory one level above. If you use the IBM-provided observability template as part of this tutorial, make sure that you are in the `terraform-ibm-observability` directory. 
   {: note}

   ```
   tar -czvf observability.tgz -C terraform .
   ```
   {: pre}
   
   Example output:
   ```
   a .
   a ./main.tf
   a ./variables.tf
   a ./version.tf
   a ./output.tf
   a ./provider.tf
   ```
   {: screen}
   
5. Create or find an existing repository in GitHub to upload your `TGZ` file to.  
6. Open the GitHub release page for your repository by appending `/releases` to your repository URL as shown in the following example. 
   ```
   https://github.com/<gh_org>/<gh_repo>/releases
   ```
   {: codeblock}

7. Click **Draft a new release**. 
8. Enter a tag version, a title, and an optional description for your release. Use the tagging suggestions in the GitHub UI to find a supported tag version. 
9. Drag your `TGZ` file from your local machine to the **Attach binaries by dropping them here or selecting them** section. 
10. Click **Publish release**. 
11. When the release is published, right-click on your `TGZ` file and copy the link to the file. 
12. Enter the link in your browser to verify that the `TGZ` file is automatically downloaded to your local machine. 
13. Decompress the `TGZ` file and verify that you can see all Terraform configuration files without the subfolder. 

## Create a private content catalog and add your Terraform template as a product
{: #create-private-catalog}
{: step}

1. [Create a private catalog in {{site.data.keyword.cloud_notm}}](/docs/account?topic=account-create-private-catalog#create-catalog).
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
 5. Change to the **Add license agreement** tab, and add any license that the user needs to agree to. 
 6. Change to the **Edit readme** tab, and add or edit the readme for your product. 
 7. Change to the **Validate product** tab. 
    1. Enter a name for the {{site.data.keyword.bpshort}} workspace that you want to create for the product validation. 
    2. In the **Deployment values** section, verify that the default values are displayed. If you want to use different values to validate your product, change the deployment values as necessary. 
    3. Click **Validate** to start the validation. During the validation, a {{site.data.keyword.bpshort}} workspace is created and the {{site.data.keyword.cloud_notm}} services that you defined in your Terraform templates are created. To monitor the progress of the validation in your workspace, you can click **View logs**. If the validation is successful, the status of your product changes to `Not published: Validated`. 
8. From the actions menu, click **Publish to account** to make your product available to other users in your private catalog. 
9. Optional: From the [{{site.data.keyword.cloud_notm}} **Resource list**](https://cloud.ibm.com/resources){: external}, remove the {{site.data.keyword.loganalysislong_notm}}, {{site.data.keyword.monitoringlong_notm}}, and {{site.data.keyword.cloudaccesstraillong_notm}} service instances that you created when you validated the product.
  
Congratulations! In this tutorial, learned how to create a private catalog in {{site.data.keyword.cloud_notm}} and how to upload an IBM-provided Terraform template as a product to your catalog. 

## What's next?
{: #whats-nxt}

- [Make your private catalog available to your users](/docs/account?topic=account-restrict-by-user#catalog-off-ui).
- [Assign users access to your private catalog](/docs/account?topic=account-catalog-access).
- [Explore other settings that you can apply to your private catalog](/docs/account?topic=account-filter-account).

