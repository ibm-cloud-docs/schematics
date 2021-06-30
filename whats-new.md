---

copyright:
  years: 2019, 2021
lastupdated: "2021-06-30"

keywords: schematics activity tracker events, schematics events, schematics audit, schematics audit events, schematics audit logs

subcollection: schematics

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:beta: .beta}
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

## June 2021
{: #june-2021}

|Date|Description|
|-----|---------|
|30 June 2021 |<ul><li> **Support taint and untaint feature enhancement in {{site.data.keyword.bplong_notm}}:** You can execute `ibmcloud schematics state list` command to view the tainted status of your resources. For more information, see [about taint and untaint command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-taint) and refer to [Timeout errors with {{site.data.keyword.bplong_notm}} blog](https://www.ibm.com/cloud/blog/timeout-errors-with-ibm-cloud-schematics){: external}.</li><li>**Documentation support to deploy resources in specific region or across multiple region:** For more information, see [Deploying IBM Cloud resources in a specific region or across multiple regions {{site.data.keyword.cloud_notm}} resources](/docs/schematics?topic=schematics-multi-region-deployment).</li><li>**Documentation support to create workspace by using {{site.data.keyword.bplong_notm}} resources**: For more information, see [Setting up Terraform for {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-terraform-setup).</li></ul>|
{: caption="What's new in June" caption-side="top"}




## May 2021
{: #may-2021}

|Date|Description|
|-----|---------|
|26 May 2021 |<ul><li> **Version constraints support in {{site.data.keyword.bplong_notm}}:** Specifying version constraints for Terraform and Ansible. For more information, see [specifying version constraints in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-version-constraints).</li><li>**Troubleshooting guide support:** For more information, about the debugging {{site.data.keyword.bpshort}} apply errors, see [Why do timeout failures result in tainted {{site.data.keyword.cloud_notm}} resources?](/docs/schematics?topic=schematics-tainted-resources), [Why am I getting 5xx HTTP errors?]/docs/schematics?topic=schematics-server-errors), [Why can't {{site.data.keyword.bpshort}} find the resource group?](/docs/schematics?topic=schematics-rg-not-found), and [How can I find the root cause of why {{site.data.keyword.bpshort}} apply is failing?](/docs/schematics?topic=schematics-nullresource-errors)</li><li>**{{site.data.keyword.bpshort}} supports sample solutions:** Sample solutions by using Terraform templates and modules to set up the infrastructure. For more information, see [Sample solutions for {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-sol-overview).</li></ul>|
{: caption="What's new in May" caption-side="top"}



## April 2021
{: #april-2021}

|Date|Description|
|-----|---------|
|14 April 2021 |**Ansible support in {{site.data.keyword.bplong_notm}} is now generally available:** Use {{site.data.keyword.bpshort}} Actions to run your Ansible playbooks in {{site.data.keyword.cloud_notm}}, and automate the configuration, operation, and management of cloud resources, or deploy multi-tier app workloads. To get started, see [{{site.data.keyword.bpshort}} landing page](/docs/schematics?topic=schematics-getting-started), try out one of the [IBM-provided Ansible playbooks](/docs/schematics?topic=schematics-sample_actiontemplates) or learn more about how Schematics integrates Ansible in [About {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-about-schematics#how-to-actions).|
{: caption="What's new in April" caption-side="top"}


## March 2021
{: #march-2021}


|Date|Description|
|-----|---------|
|29 March 2021 |<ul><li> **Terraform v0.14 support**: {{site.data.keyword.bplong_notm}} now supports Terraform v0.14 now. You can now select to run your infrastructure code with Terraform version `0.11` or `0.12` or `0.13` or `0.14`. Make sure that you use the syntax that is compatible with the Terraform version that you want to use. **Note** Terraform v0.14 introduced new sensitive attribute in the variable metadata configuration. {{site.data.keyword.bpshort}} do not detect this sensitive attribute. Users should continue to use the sensitive checkbox in the {{site.data.keyword.bplong_notm}} console, and use secure flag in the API payload. {{site.data.keyword.bpshort}} already supports masking the sensitive updated values. **Note** Terraform v0.14 introduced a lock file for versions with the name `.terraform.lock.hcl`. This file is created during `terraform init`. If you use `.terraform.lock.hcl` file in the Terraform commands, the versions stored in `.terraform.lock.hcl` file are used. {{site.data.keyword.bpshort}} doesn't support this feature. {{site.data.keyword.bpshort}} will not store this file for subsequent actions. </li></ul> |
{: caption="What's new in March" caption-side="top"}

## February 2021
{: #February-2021}

|Date|Description|
|-----|---------|
|25 February 2021 |<ul><li> **{{site.data.keyword.bpshort}} supports the ability to persist the user-defined file**: {{site.data.keyword.bplong_notm}} supports the ability to persist the user-defined file for running the subsequent Terraform commands. For more information, see [persist user-defined files in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-faqs#persist-file).</li><li> **Allowed file extensions in {{site.data.keyword.bpshort}}**: Allowed and blocked file extensions support during cloning. For more information, see [allowed and blocked file extensions](/docs/schematics?topic=schematics-faqs#clone-file-extension).</li><li> **{{site.data.keyword.bpshort}} CLI plug-in and commands support in CLI documentation**: {{site.data.keyword.bplong_notm}} [command-line plug-in to install](/docs/cli?topic=schematics-cli-plugin-schematics-cli-reference), and [view command-line commands](/docs/cli?topic=schematics-cli-plugin-schematics-cli-reference) in the CLI documentation.</li></ul> |
|12 February 2021 |<ul><li> **Ansible open beta release**: {{site.data.keyword.bplong_notm}} supports and releases Ansible open beta version for the IBMers and customers. For more information, see [about Ansible](/docs/schematics?topic=schematics-getting-started-ansible), watch [video about Ansible](https://www.youtube.com/watch?v=fHO1X93e4WA), and see [{{site.data.keyword.cloud_notm}} Terraform provider updates and Ansible actions in {{site.data.keyword.bpshort}}](https://www.ibm.com/cloud/blog/announcements/ibm-cloud-terraform-provider-updates-and-closed-beta-of-ansible-actions-in-schematics) blog.</li> </ul> |
{: caption="What's new in February" caption-side="top"}


## January 2021
{: #January-2021}

|Date|Description|
|-----|---------|
|29 January 2021 |<ul><li>**Virtual Private Endpoint Gateways support**: {{site.data.keyword.bplong_notm}} supports Virtual Private Endpoint Gateways for secure connection. For more information, see [Virtual private endpoint gateways for {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-private-endpoints#endpoint-setup)</li></ul>|
|20 January 2021 |<ul><li> **Terraform commands API support**: {{site.data.keyword.bplong_notm}} supports Terraform commands API. For more information, see [Commands API](https://cloud.ibm.com/apidocs/schematics#update-terraform-commands)</li><li>**Terraform commands command-line support**: {{site.data.keyword.bplong_notm}} supports Terraform command-line commands. For more information, see [Terraform command-line commands](/docs/schematics?topic=schematics-schematics-cli-reference#tf-cmds).</li><li>**command-line Commands**: {{site.data.keyword.bplong_notm}} supports command-line workspace and state commands such as [import](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-import), [output](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-output), [show](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-show), [state move](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-wks_statemv), [state remove](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-wks_staterm), [taint](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-taint), and [untaint](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-untaint).</li></ul>|
|7 January 2021 |<ul><li>**Multiple SDK support**: {{site.data.keyword.bplong_notm}} supports `Java`, `Node`, `Python`, and `Go` SDK for the APIs. For more information, see [Schematics API documentation](https://cloud.ibm.com/apidocs/schematics?code=go#introduction)</li></ul>|
{: caption="What's new in January" caption-side="top"}

## December 2020
{: #december-2020}

|Date|Description|
|-----|---------|
|9 December 2020 |<ul><li> **Ansible beta release**: {{site.data.keyword.bplong_notm}} supports and releases Ansible beta version for the IBMers. For more information, see [about Ansible](/docs/schematics?topic=schematics-getting-started-ansible) and watch [video about Ansible](https://www.youtube.com/watch?v=fHO1X93e4WA). **Beta:**  The open beta release of Ansible support is now available in {{site.data.keyword.bplong_notm}} to IBM users. Contact your IBM Cloud Schematics Technical Offering Manager [Sai Vennam](mailto:svennam@us.ibm.com), if you are interested in getting early access to this beta offering.</li></ul> |
{: caption="What's new in December" caption-side="top"}




## November 2020
{: #november-2020}

|Date|Description|
|-----|---------|
|25 November 2020 |<ul><li> **Terraform v0.13 support**: {{site.data.keyword.bplong_notm}} now supports Terraform v0.13. You can now choose to run your infrastructure code with Terraform version `0.11` or `0.12` or `0.13`.  With Terraform version 0.13, the syntax for configuration files changed. Make sure that you use the syntax that is compatible with the Terraform version that you want to use. </li></ul> |
{: caption="What's new in November" caption-side="top"}


## October 2020
{: #october-2020}

|Date|Description|
|-----|---------|
|16 October 2020 |<ul><li>**Monitoring**: {{site.data.keyword.bplong_notm}} now supports monitoring Schematics services by using {{site.data.keyword.cloud_notm}} Monitoring. For more information, about the monitoring Schematics workspaces, see [Monitoring Schematics instances](/docs/schematics?topic=schematics-monitoring-instances).</li><li>**Files and resources for your workspace actions**: {{site.data.keyword.bplong_notm}} now performs the vulnerability check of the files and resources that are added for the first time to your repository.</li><li>**Creating a deploy to {{site.data.keyword.bplong_notm}} link**: {{site.data.keyword.bplong_notm}} now supports an efficient way to share your Git repository so that other people can experiment to create workspace by using Schematics without affecting your original code. For more information, about deploy to {{site.data.keyword.cloud_notm}}, see [create deploy to Schematics](/docs/schematics?topic=schematics-workspace-setup#create-deploy-to-schematics).</li></ul> |
{: caption="What's new in October" caption-side="top"}


## September 2020
{: #september-2020}

|Date|Description|
|-----|-----------|
|11 September 2020|<ul><li>**`Bitbucket` supports private repository**: {{site.data.keyword.bplong_notm}} supports private bit bucket repository as a template repository source. All you need to use the URL in this format `https://bitbucket.org/<your_user_name>/<repo_name>/src/<branch_name>/<folder_name>` and for the URL with branch you need to use in this format `https://bitbucket.org/<your_user_name>/<repo_name>/src/<branch_name>`. If the workspace name is different from your user name then you need to provide the workspace name in this format. `https://<username>@bitbucket.org/<workspace_name>/tf_cloudless_sleepy/src/master`.</li><li>**Support to override the default variable**: {{site.data.keyword.bplong_notm}} now supports to override the Terraform default variable store value. For more information, about configuring to override the variable, see [Workspace update](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update).</li></ul> |
{: caption="What's new in September" caption-side="top"}

## August 2020
{: #august-2020}

|Date|Description|
|-----|-----------|
|14 August 2020|<ul><li>**Support for multiple Terraform provider**: {{site.data.keyword.bplong_notm}} now supports multiple Terraform provider versions. You need to add Terraform provider block with the right provider version. By default the provider executes latest version `1.21.0`, and previous four versions such as `1.20.1`, `1.20.0`, `1.19.0`, `1.18.0` are supported. For more information, about the provider configuration, see [Multiple Terraform Provider](/docs/schematics?topic=schematics-faqs#provider-versions).</li><li>**Support for complex data types**: {{site.data.keyword.bplong_notm}} now supports complex data types of Terraform v0.12. For more information, about declaring complex data types, see [Configuring variables](/docs/schematics?topic=schematics-create-tf-config#configure-variables).</li><li>**Time out set for local-exec and remote-exec users**: If you run local-exec or remote-exec users, make sure the execution completes within 30 minutes. Otherwise execution times out automatically. </li><li>**`Bitbucket` is used as a template repository source**: {{site.data.keyword.bplong_notm}} supports public bit bucket repository as a template repository source. Private bit bucket repository needs a workaround. Download the files from the repository, then, you need to `tar` the files and upload in the {{site.data.keyword.bplong_notm}} workspace.</li></ul> |

## July 2020
{: #july-2020}

|Date|Description|
|-----|-----------|
|9 July 2020|<ul><li>**Stop apply support**: {{site.data.keyword.bplong_notm}} now supports stopping a {{site.data.keyword.bpshort}} apply action that currently runs against your workspace from the console and the API. For more information, see [Managing resource life cycles](/docs/schematics?topic=schematics-manage-lifecycle) or use the [`DELETE /v1/workspaces/{id}/actions/{action_id}`](/apidocs/schematics#stop-a-schematics-apply-action) API. </li><li>**New {{site.data.keyword.bpshort}} locations**: You can now create {{site.data.keyword.bpshort}} workspaces in the Frankfurt or London location by using the location selector from the {{site.data.keyword.bpshort}} console or targeting the matching {{site.data.keyword.cloud_notm}} region from the CLI. For more information, see [Locations and service endpoints](/docs/schematics?topic=schematics-locations) and [Where is my data stored?](/docs/schematics?topic=schematics-secure-data#pi-location)</li></ul> |

## June 2020
{: #june-2020}

|Date|Description|
|-----|-----------|
|26 June 2020|**Ansible provisioner support**: You can now use the Ansible provisioner with {{site.data.keyword.bplong_notm}} to deploy software on {{site.data.keyword.cloud_notm}} resources or perform actions against your resources, such as shutting down a virtual server instance. For more information about how to use the Ansible provisioner, see the following blogs: <ul><li>[Discover best practice VPC configuration for application deployment](https://developer.ibm.com/articles/secure-vpc-access-with-a-bastion-host-and-terraform/)</li><li>[Learn about repeatable and reliable end-to-end app provisioning and configuration](https://developer.ibm.com/articles/application-deployment-with-redhat-ansible-and-ibm-cloud-schematics/). </li></ul>   |
|25 June 2020|**Version 1.8.0 of the {{site.data.keyword.cloud_notm}} Provider plug-in available**: The {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform version 1.8.0 is now enabled in {{site.data.keyword.bplong_notm}}. For more information about the version, see the [release notes](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v1.8.0){: external}. For an overview of supported {{site.data.keyword.cloud_notm}} resources and data sources, see the [{{site.data.keyword.cloud_notm}} Provider plug-in reference](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-index-of-terraform-on-ibm-cloud-resources-and-data-sources).| 
|22 June 2020|**Upload Terraform templates as TAR files**: You can now provide your Terraform template by uploading a tap archive file from your local machine. This feature is supported from the command-line or API. For more information, see the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command or [`PUT /v1/workspaces/{id}/templates/{template_id}/template_repo_upload`](/apidocs/schematics#upload-a-tar-file-to-your-workspace) API.|

## May 2020
{: #may-2020}

|Date|Description|
|-----|-----------|
|8 May 2020|**New EU API endpoint**: You can now choose to create your {{site.data.keyword.bpshort}} workspaces in the US or Europe. Depending on the location that you choose, your {{site.data.keyword.bpshort}} actions run in either the US (`us-south` or `us-east`) or in Europe (`eu-de` or `eu-gb`). The location that you choose for your workspace is independent from the location where you want to provision your resources. For more information, see [Locations and service endpoints](/docs/schematics?topic=schematics-locations). |

## April 2020
{: #april-2020}

|Date|Description|
|-----|-----------|
|17 April 2020|<ul><li><strong>Terraform v0.12 support</strong>: You can now choose to run your infrastructure code with Terraform version 0.11 or 0.12. With Terraform version 0.12, the syntax for configuration files changed. Make sure that you use the syntax that is compatible with the Terraform version that you want to use. To migrate your Terraform configuration files from version 0.11 to version 0.12, see [Migrating your Terraform configuration files from version 0.11 to version 0.12](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-migration-versioncontrol).  </li><li><strong>New workspace creation flow</strong>: The workspace creation flow is now [a two-step process](/docs/schematics?topic=schematics-workspace-setup#create-workspace). First, you create the workspace without connecting it to a GitHub or GitLab repository. Then, you add the details of your GitHub or GitLab repository, retrieve input variables, and let {{site.data.keyword.bpshort}} scan your Terraform configuration files for syntax errors. With the change of the workspace creation flow, [new workspace states](/docs/schematics?topic=schematics-workspace-setup#wks-state) are introduced as well.</li></ul> |
