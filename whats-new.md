---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-25"

keywords: schematics activity tracker events, schematics events, schematics audit, schematics audit events, schematics audit logs

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

# What's new in {{site.data.keyword.bpshort}}?
{: #new-in-schematics}

Learn about the latest changes to the {{site.data.keyword.bplong_notm}} service that are grouped by month.

## June 2020
{: #june-2020}

|Date|Description|
|-----|-----------|
|25 June 2020|The {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform version 1.8.0 is now enabled in {{site.data.keyword.bplong_notm}}. For more information about the version, see the [release notes](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v1.8.0){: external}. For an overview of supported {{site.data.keyword.cloud_notm}} resources and data sources, see the [{{site.data.keyword.cloud_notm}} Provider plug-in reference](/docs/terraform?topic=terraform-index-of-terraform-resources-and-data-sources).| 
|22 June 2020|You can now provide your Terraform template by uploading a TAR file from your local machine. This feature is supported from the CLI or API. For more information, see the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command or [`PUT /v1/workspaces/{id}/templates/{template_id}/template_repo_upload`](/apidocs/schematics#upload-a-tar-file-to-your-workspace) API.|

## May 2020
{: #may-2020}

|Date|Description|
|-----|-----------|
|8 May 2020|<ul><li>You can now choose to create your {{site.data.keyword.bpshort}} workspaces in the US or Europe. Depending on the location that you choose, your {{site.data.keyword.bpshort}} actions run in either the US (`us-south` or `us-east`) or in Europe (`eu-de` or `eu-gb`). The location that you choose for your workspace is independent from the location where you want to provision your resources. For more information, see [Locations and service endpoints](/docs/schematics?topic=schematics-locations). </li></ul>|

## April 2020
{: #april-2020}

|Date|Description|
|-----|-----------|
|17 April 2020|<ul><li><strong>Terraform v0.12 support</strong>: You can now choose to run your infrastructure code with Terraform version 0.11 or 0.12. With Terraform version 0.12, the syntax for configuration files changed. Make sure that you use the syntax that is compatible with the Terraform version that you want to use. To migrate your Terraform configuration files from version 0.11 to version 0.12, see [Migrating your Terraform configuration files from version 0.11 to version 0.12](/docs/terraform?topic=terraform-setup_cli#tf-0.12-migration).  </li><li><strong>New workspace creation flow</strong>: The workspace creation flow is now [a two-step process](/docs/schematics?topic=schematics-workspace-setup#create-workspace). First, you create the workspace without connecting it to a GitHub or GitLab repository. Then, you add the details of your GitHub or Gitlab repository, retrieve input variables, and let {{site.data.keyword.bpshort}} scan your Terraform configuration files for syntax errors. With the change of the workspace creation flow, [new workspace states](/docs/schematics?topic=schematics-workspace-setup#workspace-states) are introduced as well.</li></ul> |
