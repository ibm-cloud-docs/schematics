---

copyright:
  years: 2017, 2022
lastupdated: "2022-04-12"

keywords: schematics whats new?, schematics features and enhancements, schematics releases

subcollection: schematics

content-type: release-note

---

{{site.data.keyword.attribute-definition-list}}


# Release notes
{: #schematics-relnotes}

Use the release notes to learn about the latest changes to the {{site.data.keyword.bpshort}} documentation that are grouped by month.
{: shortdesc}

For information about releases that occurred before 22 October 2021, see [What's new?](/docs/schematics?topic=schematics-new-in-schematics){: external}.
{: note}

## April 2022
{: #schematics-apr22}

Review the release notes for April 2022.
{: shortdesc}

### 12 April 2022
{: #schematics-apr3122}
{: release-note}

{{site.data.keyword.bpshort}} command-line supports private {{site.data.keyword.bpshort}} endpoint
:   The {{site.data.keyword.bpshort}} command-line [supports private {{site.data.keyword.bpshort}} endpoint](/docs/schematics?topic=schematics-private-endpoints#private-cse).

Support `.JSON` and `.tfvars` file extension for {{site.data.keyword.bpshort}} plan and apply commands
:   The {{site.data.keyword.bpshort}} command-line supports `.JSON` and `.tfvars` file extension in {{site.data.keyword.bpshort}} plan and apply commands.

Enhance resources tabular data view for resources.
:   The {{site.data.keyword.bpshort}} command-line lists the provisioned resources from your workspace in a tabular data view output with **Resource**, **Type**, **State**, **Resource group**, **URL**, and **Tags** fields. For example, use [`ibmcloud schematics state list`](/docs/schematics?topic=schematics-schematics-cli-reference#state-list) command to list the resources provisioned in your workspace.

Deprecate and warning message when using `ibmcloud terraform` command.
:   The {{site.data.keyword.bpshort}} `ibmcloud terraform` command usage displays warning and deprecation message as **Alias 'terraform' will be deprecated. Please use 'schematics' or 'sch' in your commands.**

Release {{site.data.keyword.bpshort}} command-line plugin 
:   The {{site.data.keyword.bpshort}} [command-line plugin v1.8.0](/docs/schematics?topic=schematics-cli_version-releases) released on 9th April 2022.

## March 2022
{: #schematics-mar22}

Review the release notes for March 2022.
{: shortdesc}





### 31 March 2022
{: #schematics-mar3122}
{: release-note}

Support deleting {{site.data.keyword.bpshort}} data objects 
:   The {{site.data.keyword.bpshort}} supports [deleting {{site.data.keyword.bpshort}} data from UI, CLI, and API](/docs/schematics?topic=schematics-delete-schematics-data-intro&interface=ui) for workspace, action, and inventories objects.

Fixes related to {{site.data.keyword.bpshort}} Actions and workspace
:   - Now you can create Actions with an [empty resource group](/apidocs/schematics/schematics#create-action). The empty resource group automatically points to the `Default` resource group.
:   - [List workspace API](/apidocs/schematics/schematics#list-workspaces) support `summary` profile type.
:   - [Get inventory definition](/apidocs/schematics/schematics#get-inventory) supports `detailed` profile type.

Get job files API supports `plan_json` file type
:   The {{site.data.keyword.bpshort}} supports `plan_json` file type in [Get job output API](/apidocs/schematics/schematics#get-job-files).
:   PATCH inventory definition in the inventories API is removed from the documentation.


### 15 March 2022
{: #schematics-mar1522}
{: release-note}

Support `__netrc__` environment values in private Git repository
:   The {{site.data.keyword.bpshort}} supports the latest `__netrc__` environment values to support download the Terraform module templates for private Git repository in [command-line](/docs/schematics?topic=schematics-download-modules-pvt-git) and [APIs](/apidocs/schematics/schematics#create-workspace).


### 4 March 2022
{: #schematics-mar422}
{: release-note}

Support `Terraform v1.1` in {{site.data.keyword.bpshort}} 
:   The {{site.data.keyword.bpshort}} supports the latest `Terraform version 1.1` in [UI](/docs/schematics?topic=schematics-workspace-setup&interface=ui#create-workspace_ui), [command-line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) and [APIs](/apidocs/schematics/schematics#create-workspace).

Release {{site.data.keyword.bpshort}} command-line plugin 
:   The {{site.data.keyword.bpshort}} [command-line plugin v1.7.3](/docs/schematics?topic=schematics-cli_version-releases) released on 4th March 2022.

## February 2022
{: #schematics-feb22}

Review the release notes for February 2022.
{: shortdesc}

### 16 February 2022
{: #schematics-feb1622}
{: release-note}

Release {{site.data.keyword.bpshort}} command-line plugin 
:   The {{site.data.keyword.bpshort}} [command-line plugin v1.7.2](/docs/schematics?topic=schematics-cli_version-releases) released on 16th February 2022.

Supports installer for Linux&trade; arm64 and Mac OS arm64 binaries 
:   The {{site.data.keyword.bpshort}} supports command-line installer for [Linux&trade; arm64 and Mac OS arm64 binaries](/docs/schematics?topic=schematics-setup-cli) Operating System.

### 11 February 2022
{: #schematics-feb1122}
{: release-note}

Release {{site.data.keyword.bpshort}} command-line plugin 
:   The {{site.data.keyword.bpshort}} [command-line plugin v1.7.1](/docs/schematics?topic=schematics-cli_version-releases) released on 11th February 2022.


## January 2022
{: #schematics-jan22}

Review the release notes for January 2022.
{: shortdesc}

### 31 January 2022
{: #schematics-jan3122}
{: release-note}

Release {{site.data.keyword.bpshort}} command-line plugin 
:   The {{site.data.keyword.bpshort}} [command-line plugin v1.7.0](/docs/schematics?topic=schematics-cli_version-releases) released on 18th January 2022.

Supports installer for PowerLinux&trade; and System/390 Linux&trade; 
:   The {{site.data.keyword.bpshort}} supports command-line installer for [PowerLinux&trade; 64-bit and System/390 Linux&trade; 64-bit](/docs/schematics?topic=schematics-setup-cli) Operating System.

## December 2021
{: #schematics-dec21}

Review the release notes for December 2021.
{: shortdesc}

### 30 December 2021
{: #schematics-dec3021}
{: release-note}

Release {{site.data.keyword.bpshort}} command-line plugin 
:   The {{site.data.keyword.bpshort}} [command-line plugin v1.6.2](/docs/schematics?topic=schematics-cli_version-releases) released on 2nd December 2021.

## November 2021
{: #schematics-nov21}

Review the release notes for November 2021.
{: shortdesc}

### 30 November 2021
{: #schematics-nov3021}
{: release-note}

Centrally manage access tags for {{site.data.keyword.bpshort}} workspaces in your account
:   To create and associate access tags for {{site.data.keyword.bpshort}} workspaces in your account, see [Manage access tag in your account](/docs/schematics?topic=schematics-access#access-tag).

Support `WinRM` in user interface
:   {{site.data.keyword.bpshort}} supports [Windows Remote Management (`WinRM`)](/docs/schematics?topic=schematics-action-setup&interface=ui#create-action-setup) for {{site.data.keyword.bpshort}} actions.

Global catalog settings for {{site.data.keyword.bpshort}} workspaces location
: You can now, manage the catalog settings for {{site.data.keyword.bpshort}} resources based on the location. For more information, see [Manage location settings in catalog](/docs/schematics?topic=schematics-access-ibm-cloud-catalog).

About `compact` download
:   You can download only the relevant files from the Git repository for your workspaces, for more information, see [Compact download for {{site.data.keyword.bpshort}} workspace](/docs/schematics?topic=schematics-compact-download&interface=ui).

About {{site.data.keyword.bpshort}} Job files
:   You can now download the state-file at every job level along with the latest state-file of a workspace by using the existing [Get Job API](/apidocs/schematics/schematics#get-job-files). For more information, see [Download {{site.data.keyword.bpshort}} Job files](/docs/schematics?topic=schematics-job-download).

ResourceQuery attribute deprecated 
:   ResourceQuery attribute is replaced as [resource_queries](/apidocs/schematics/schematics#list-resource-query) in the API.

## October 2021
{: #schematics-oct21}

Review the release notes for October 2021.
{: shortdesc}

### 22 October 2021
{: #schematics-oct2221}
{: release-note}

Onboarding Terraform templates to private catalog
:   For onboarding multiple Terraform templates into {{site.data.keyword.cloud_notm}} private catalog, see [Onboard bulk Terraform templates to private catalog](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-template#provider-onboard).

Sample templates to deploy into {{site.data.keyword.cloud_notm}}
:   Explore [Terraform sample Terraform templates](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-template#sample-templates) to provision different {{site.data.keyword.cloud_notm}} services using {{site.data.keyword.bpshort}} workspaces.

Support `WinRM` in command-line
:   The {{site.data.keyword.bpshort}} supports [Windows Remote Management (`WinRM`)](https://www.ibm.com/docs/en/license-metric-tool?topic=v-configuring-winrm-hyper-hosts) for {{site.data.keyword.bpshort}} actions. Added the `--inventory-connection-type`, `--bastion-credential-json`, and `--credential-json` option value to the [**create**](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-create-action), and [**update**](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-update-action) commands.

Documentation lists the Command-line version change log history
:   The {{site.data.keyword.bpshort}} documentation supports the list of [command-line features, enhancements, and fixes note](/docs/schematics?topic=schematics-cli_version-releases).

