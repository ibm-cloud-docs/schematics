---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-19"

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
|22 June 2020|<ul><li>You can now provide your Terraform template by uploading a tape archive file (`.tar`). This feature is supported from the CLI or API. For more information, see the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command.  </li></ul>|

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
