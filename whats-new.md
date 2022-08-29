---

copyright:
  years: 2019, 2022
lastupdated: "2022-08-29"

keywords: schematics activity tracker events, schematics events, schematics audit, schematics audit events, schematics audit logs

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# What's new in {{site.data.keyword.bpshort}}?
{: #new-in-schematics}

Learn about the changes to the {{site.data.keyword.bplong_notm}} service that are grouped by Month.

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version).
{: deprecated}

## September 2021
{: #sept30-2021}

|Date|Description|
|-----|---------|
|30 September 2021 |<ul><li>**Inventory target feature support in {{site.data.keyword.bpshort}} Actions API:** The {{site.data.keyword.bpshort}} supports [Windows Remote Management (`WinRM`)](https://www.ibm.com/docs/en/license-metric-tool?topic=v-configuring-winrm-hyper-hosts) port as `inventory_connection_type` parameter in the [create](/apidocs/schematics/schematics#create-action) and [update](/apidocs/schematics/schematics#update-action) action `APIs`.</li><li>**Bastion host enhancement in {{site.data.keyword.bpshort}} Actions API:** The {{site.data.keyword.bpshort}} enhances the bastion host configuration as an optional parameter in the [create](/apidocs/schematics/schematics#create-action) and [update](/apidocs/schematics/schematics#update-action) action `APIs` if the `inventory connection type` is set to `winrm`.</li><li>**{{site.data.keyword.bpshort}} Actions API enhancement to support bastion host connection with non-root user:** The {{site.data.keyword.bpshort}} Actions API now supports bastion host connection with non-root user and the `ssh` in the [create](/apidocs/schematics/schematics#create-action) and [update](/apidocs/schematics/schematics#update-action) action `APIs`.</li><li>**{{site.data.keyword.bplong_notm}} job queue process:** For more information, about job queue process, see [Executing process of the {{site.data.keyword.bpshort}} job queue](/docs/schematics?topic=schematics-job-queue-process) and [FAQ](/docs/schematics?topic=schematics-workspaces-faq#job-queue-faq).</li><li>**{{site.data.keyword.bpshort}} Actions `APIs` enhances the credentials parameter:** You can now access the inventory username through the credentials parameter in the [create](/apidocs/schematics/schematics#create-action) and [update](/apidocs/schematics/schematics#update-action) action `APIs`.</li><li>**{{site.data.keyword.bpshort}} introduces compact flag in the workspace create and update API:** You can now download the `subfolders` from the GIT repositories through {{site.data.keyword.bpshort}}. For more information, see [How can I download `subfolders` from the GIT repositories through {{site.data.keyword.bpshort}}?](/docs/schematics?topic=schematics-workspaces-faq#compact-faq).</li><li>**Importance of location and URL endpoint in workspace creation:** [Why do {{site.data.keyword.bpshort}} Workspaces create through API fails?](/docs/schematics?topic=schematics-wks-create-api).</li></ul> |
{: caption="What's new in September" caption-side="top"}

## August 2021
{: #aug-2021}

|Date|Description|
|-----|---------|
|27 August 2021 |<ul><li>**Workspace update command enhancement:** The {{site.data.keyword.bplong_notm}} supports pull request flag in the [{{site.data.keyword.bpshort}} Workspaces update](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update) command.</li><li> **Terraform v1.0 support:** {{site.data.keyword.bplong_notm}} now supports Terraform v1.0 now. You can now select to run your infrastructure code with Terraform version `0.12` or `0.13` or `0.14`, `0.15` or `1.0`. Make sure that you use the syntax that is compatible with the Terraform version that you want to use. Note you can experience a unified console experience across all support platforms, and provides a provider based sensitivity and sensitive functions. For more information, about Terraform v1.0 availability from HashiCorp Language, see [Terraform v1.0 general availability](https://www.terraform.io/language/upgrade-guides/1-0).</li><li>**{{site.data.keyword.bplong_notm}} support job queue logs enhancement:** For more information, about viewing job queue logs, see [Reviewing the {{site.data.keyword.bpshort}} job details](/docs/schematics?topic=schematics-workspace-setup#job-logs).</li></ul> |
|11 August 2021 |<ul><li>**{{site.data.keyword.bplong_notm}} deprecates older version of Terraform:** The `end of marketing` and `end of support` of deprecating older version of Terraform provider in {{site.data.keyword.bplong_notm}}, refer to, [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version).</li></ul> |
{: caption="What's new in August" caption-side="top"}


## July 2021
{: #july-2021}

|Date|Description|
|-----|---------|
|30 July 2021 |<ul><li>**{{site.data.keyword.bplong_notm}} deprecates Terraform v0.11:** {{site.data.keyword.bplong_notm}} deprecates the support of Terraform v0.11 from July 2021. As HashiCorp Configuration Language deprecated Terraform v0.11 in Terraform providers.</li><li> **Terraform v0.15 support**: {{site.data.keyword.bplong_notm}} now supports Terraform v0.15 now. You can now select to run your infrastructure code with Terraform version `0.12` or `0.13` or `0.14` or `0.15`. Make sure that you use the syntax that is compatible with the Terraform version that you want to use. Note Terraform v0.15 provides you to remote state data sources, cross-compatible between the `Terraform v0.14.x` and higher version to move between Terraform versions. Also, you can experience a unified console experience across all support platforms, and provides provider-based sensitivity and sensitive functions. For more information, about Terraform v0.15 availability from HashiCorp Language, see [Terraform v0.15 general availability](https://www.hashicorp.com/blog/announcing-hashicorp-terraform-0-15-general-availability).</li><li> **Ansible v2.9.23 API and command-line support:** Ansible v2.9.23 and Ansible provisioner v2.3.3 are supported in the {{site.data.keyword.bplong_notm}} Action.</li></ul> |
|19 July 2021 |<ul> <li> **Support parallelism and other environment variables in {{site.data.keyword.bplong_notm}}:** {{site.data.keyword.bplong_notm}} supports setting a custom value for parallelism. For more information, see [Supporting parallelism and other Terraform environment variables in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-set-parallelism).</li></ul>|
{: caption="What's new in July" caption-side="top"}


## June 2021
{: #june-2021}

|Date|Description|
|-----|---------|
|30 June 2021 |<ul><li> **Support taint and untaint feature enhancement in {{site.data.keyword.bplong_notm}}:** You can run `ibmcloud schematics state list` command to view the tainted status of your resources. For more information, see [about taint and `untaint` command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-taint) and refer to, [Time out errors with {{site.data.keyword.bplong_notm}} blog](https://www.ibm.com/cloud/blog/timeout-errors-with-ibm-cloud-schematics){: external}.</li><li>**Documentation support to deploy resources in specific region or across multiple region:** For more information, see [Deploying {{site.data.keyword.cloud_notm}} resources in a specific region or across multiple regions {{site.data.keyword.cloud_notm}} resources](/docs/schematics?topic=schematics-multi-region-deployment).</li><li>**Documentation support to create workspace by using {{site.data.keyword.bplong_notm}} resources**: For more information, see [Setting up Terraform for {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-terraform-setup).</li><li>**One page view to create workspace by using `UI`, `CLI`, `API`, and `Terraform` switcher documentation:** For more information, about {{site.data.keyword.bplong_notm}} workspaces, creation, see [Setting up workspaces](/docs/schematics?topic=schematics-workspace-setup).</li><li>**Temporarily {{site.data.keyword.bplong_notm}} workspaces stop activity API is deactivated:** For more information, see [Stop an apply job](/apidocs/schematics/schematics#delete-workspace-activity) API.</li></ul>|
{: caption="What's new in June" caption-side="top"}


## May 2021
{: #may-2021}

|Date|Description|
|-----|---------|
|26 May 2021 |<ul><li> **Version constraints support in {{site.data.keyword.bplong_notm}}:** Specifying version constraints for Terraform and Ansible. For more information, see [specifying version constraints in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-version-constraints).</li><li>**Troubleshooting guide support:** For more information, about the debugging {{site.data.keyword.bpshort}} apply errors, see [Why do timeout failures result in tainted {{site.data.keyword.cloud_notm}} resources?](/docs/schematics?topic=schematics-server-errors), [Why am I getting 5xx HTTP errors?]/docs/schematics?topic=schematics-server-errors), [Why can't {{site.data.keyword.bpshort}} find the resource group?](/docs/schematics?topic=schematics-rg-not-found), and [How can I find the root cause of why {{site.data.keyword.bpshort}} apply is failing?](/docs/schematics?topic=schematics-nullresource-errors)</li><li>**{{site.data.keyword.bpshort}} supports sample solutions:** Sample solutions by using Terraform templates and modules to set up the infrastructure. For more information, see [Sample solutions for {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-sol-overview).</li></ul>|
{: caption="What's new in May" caption-side="top"}


## April 2021
{: #april-2021}

|Date|Description|
|-----|---------|
|14 April 2021 |**Ansible support in {{site.data.keyword.bplong_notm}} is now generally available:** Use {{site.data.keyword.bpshort}} Actions to run your `Ansible playbooks` in {{site.data.keyword.cloud_notm}}, and automate the configuration, operation, and management of cloud resources, or deploy multitiered app workloads. To get started, see [{{site.data.keyword.bpshort}} landing page](/docs/schematics?topic=schematics-getting-started), try out one of the [IBM-provided `Ansible playbooks`](/docs/schematics?topic=schematics-sample_actiontemplates) or learn more about how {{site.data.keyword.bpshort}} integrates Ansible in [About {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-how-it-works#how-to-actions).|
{: caption="What's new in April" caption-side="top"}


## March 2021
{: #march-2021}


|Date|Description|
|-----|---------|
|29 March 2021 |<ul><li> **Terraform v0.14 support**: {{site.data.keyword.bplong_notm}} now supports Terraform v0.14 now. You can now select to run your infrastructure code with Terraform version `0.11` or `0.12` or `0.13` or `0.14`. Make sure that you use the syntax that is compatible with the Terraform version that you want to use. `Terraform v0.14` introduced new sensitive attribute in the variable metadata configuration. {{site.data.keyword.bpshort}} do not detect this sensitive attribute. Users should continue to use the sensitive checkbox in the {{site.data.keyword.bplong_notm}} console, and use secure flag in the API payload. {{site.data.keyword.bpshort}} already supports masking the sensitive updated values. `Terraform v0.14` introduced a lock file for versions with the name `.terraform.lock.hcl`. This file is created during `terraform init`. If you use `.terraform.lock.hcl` file in the Terraform commands, the versions stored in `.terraform.lock.hcl` file are used. {{site.data.keyword.bpshort}} doesn't support this feature. {{site.data.keyword.bpshort}} will not store this file for subsequent actions. </li></ul> |
{: caption="What's new in March" caption-side="top"}

## February 2021
{: #February-2021}

|Date|Description|
|-----|---------|
|25 February 2021 |<ul><li> **{{site.data.keyword.bpshort}} supports the ability to store the user-defined file**: {{site.data.keyword.bplong_notm}} supports the ability to store the user-defined file for running the subsequent Terraform commands. For more information, see [store user-defined files in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-general-faq#persist-file).</li><li> **Allowed file extensions in {{site.data.keyword.bpshort}}**: Allowed and blocked file extensions support during cloning. For more information, see [allowed and blocked file extensions](/docs/schematics?topic=schematics-workspaces-faq#clone-file-extension).</li><li> **{{site.data.keyword.bpshort}} CLI plug-in and commands support in CLI documentation**: {{site.data.keyword.bplong_notm}} [command-line plug-in to install](/docs/cli?topic=schematics-cli-plugin-schematics-cli-reference), and [view command-line commands](/docs/cli?topic=schematics-cli-plugin-schematics-cli-reference) in the CLI documentation.</li></ul> |
|12 February 2021 |<ul><li> **Ansible open Beta release**: {{site.data.keyword.bplong_notm}} supports and releases Ansible open Beta version for the IBMers and customers. For more information, see [about Ansible](/docs/schematics?topic=schematics-getting-started-ansible), watch [video about Ansible](https://www.youtube.com/watch?v=fHO1X93e4WA), and see [{{site.data.keyword.cloud_notm}} Terraform provider updates and Ansible actions in {{site.data.keyword.bpshort}}](https://www.ibm.com/cloud/blog/announcements/ibm-cloud-terraform-provider-updates-and-closed-beta-of-ansible-actions-in-schematics) blog.</li> </ul> |
{: caption="What's new in February" caption-side="top"}


## January 2021
{: #January-2021}

|Date|Description|
|-----|---------|
|29 January 2021 |<ul><li>**Virtual Private Endpoint Gateways support**: {{site.data.keyword.bplong_notm}} supports Virtual Private Endpoint Gateways for secure connection. For more information, see [Virtual private endpoint gateways for {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-private-endpoints#endpoint-setup)</li></ul>|
|20 January 2021 |<ul><li> **Terraform commands API support**: {{site.data.keyword.bplong_notm}} supports Terraform commands API. For more information, see [Commands API](/apidocs/schematics/schematics)</li><li>**Terraform commands command-line support**: {{site.data.keyword.bplong_notm}} supports Terraform command-line commands. For more information, see [Terraform command-line commands](/docs/schematics?topic=schematics-schematics-cli-reference#tf-cmds).</li><li>**command-line Commands**: {{site.data.keyword.bplong_notm}} supports command-line workspace and state commands such as [import](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-import), [show](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-show), [state move](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-wks_statemv), [state remove](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-wks_staterm), [taint](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-taint), and [`untaint`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-untaint).</li></ul>|
|7 January 2021 |<ul><li>**Multiple SDK support**: {{site.data.keyword.bplong_notm}} supports `Java`, `Node`, `Python`, and `Go` SDK for the `APIs`. For more information, see [{{site.data.keyword.bpshort}} API documentation](/apidocs/schematics/schematics?code=go)</li></ul>|
{: caption="What's new in January" caption-side="top"}

## December 2020
{: #december-2020}

|Date|Description|
|-----|---------|
|9 December 2020 |<ul><li> **Ansible Beta release**: {{site.data.keyword.bplong_notm}} supports and releases Ansible Beta version for the IBMers. For more information, see [about Ansible](/docs/schematics?topic=schematics-getting-started-ansible) and watch [video about Ansible](https://www.youtube.com/watch?v=fHO1X93e4WA). **Beta:**  The open Beta release of Ansible support is now available in {{site.data.keyword.bplong_notm}} to IBM users. Contact your {{site.data.keyword.bplong_notm}} Technical Offering Manager [`Sai Vennam`](mailto:svennam@us.ibm.com), if you are interested in getting early access to this Beta offering.</li></ul> |
{: caption="What's new in December" caption-side="top"}

## November 2020
{: #november-2020}

|Date|Description|
|-----|---------|
|25 November 2020 |<ul><li> **Terraform v0.13 support**: {{site.data.keyword.bplong_notm}} now supports Terraform v0.13. You can now choose to run your infrastructure code with Terraform version `0.11` or `0.12` or `0.13`. With Terraform version 0.13, the syntax for configuration files changed. Make sure that you use the syntax that is compatible with the Terraform version that you want to use. </li></ul> |
{: caption="What's new in November" caption-side="top"}

## October 2020
{: #october-2020}

|Date|Description|
|-----|---------|
|16 October 2020 |<ul><li>**Monitoring**: {{site.data.keyword.bplong_notm}} now supports monitoring {{site.data.keyword.bpshort}} services by using {{site.data.keyword.cloud_notm}} Monitoring. For more information, about the monitoring {{site.data.keyword.bpshort}} Workspaces, see [Monitoring {{site.data.keyword.bpshort}} instances](/docs/schematics?topic=schematics-monitoring-instances).</li><li>**Files and resources for your workspace actions**: {{site.data.keyword.bplong_notm}} now performs the vulnerability check of the files and resources that are added for the first time to your repository.</li><li>**Creating a deploy to {{site.data.keyword.bplong_notm}} link**: {{site.data.keyword.bplong_notm}} now supports an efficient way to share your Git repository so that other people can experiment to create workspace by using {{site.data.keyword.bpshort}} without affecting your original code. For more information, about deploy to {{site.data.keyword.cloud_notm}}, see [create deploy to {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-workspace-setup#create-deploy-to-schematics).</li></ul> |
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
|14 August 2020|<ul><li>**Support for multiple Terraform provider**: {{site.data.keyword.bplong_notm}} now supports multiple Terraform provider versions. You need to add Terraform provider block with the right provider version. By default the provider executes latest version `1.21.0`, and previous four versions such as `1.20.1`, `1.20.0`, `1.19.0`, `1.18.0` are supported. For more information, about the provider configuration, see [Multiple Terraform Provider](/docs/schematics?topic=schematics-general-faq#provider-versions).</li><li>**Support for complex data types**: {{site.data.keyword.bplong_notm}} now supports complex data types of Terraform v0.12. For more information, about declaring complex data types, see [Configuring variables](/docs/schematics?topic=schematics-create-tf-config#configure-variables).</li><li>**Time out set for local-exec and remote-exec users**: If you run local-exec or remote-exec users, make sure the execution completes within 30 minutes. Otherwise execution times out automatically. </li><li>**`Bitbucket` is used as a template repository source**: {{site.data.keyword.bplong_notm}} supports public bit bucket repository as a template repository source. Private bit bucket repository needs a workaround. Download the files from the repository, then, you need to `tar` the files and upload in the {{site.data.keyword.bplong_notm}} workspace.</li></ul> |

## July 2020
{: #july-2020}

|Date|Description|
|-----|-----------|
|9 July 2020|<ul><li>**Stop apply support**: {{site.data.keyword.bplong_notm}} now supports stopping a {{site.data.keyword.bpshort}} apply action that currently runs against your workspace from the console and the API. For more information, see [Managing resource life cycles](/docs/schematics?topic=schematics-manage-lifecycle) or use the [DELETE /v1/workspaces/{id}/actions/{action_id}](/apidocs/schematics/schematics#stop-a-schematics-apply-action) API. </li><li>**New {{site.data.keyword.bpshort}} locations**: You can now create {{site.data.keyword.bpshort}} Workspaces in the Frankfurt or London location by using the location selector from the {{site.data.keyword.bpshort}} console or targeting the matching {{site.data.keyword.cloud_notm}} region from the CLI. For more information, see [Locations and service endpoints](/docs/schematics?topic=schematics-locations) and [Where is my data stored?](/docs/schematics?topic=schematics-secure-data#pi-location)</li></ul> |

## June 2020
{: #june-2020}

|Date|Description|
|-----|-----------|
|26 June 2020|<ul><li>**Ansible provisioner support**: You can now use the Ansible provisioner with {{site.data.keyword.bplong_notm}} to deploy software on {{site.data.keyword.cloud_notm}} resources or perform actions against your resources, such as shutting down a virtual server instance. For more information about how to use the Ansible provisioner, see the following blogs: </li><li>[Discover best practice VPC configuration for application deployment](https://developer.ibm.com/articles/secure-vpc-access-with-a-bastion-host-and-terraform/)</li><li>[Learn about repeatable and reliable end-to-end app provisioning and configuration](https://developer.ibm.com/articles/application-deployment-with-redhat-ansible-and-ibm-cloud-schematics/). </li></ul>   |
|25 June 2020|<ul><li>**Version 1.8.0 of the {{site.data.keyword.cloud_notm}} Provider plug-in available**: The {{site.data.keyword.terraform-provider_full_notm}} version 1.8.0 is now enabled in {{site.data.keyword.bplong_notm}}. For more information about the version, see the [release notes](https://github.com/IBM-Cloud/terraform-provider-ibm/releases/tag/v1.8.0){: external}. For an overview of supported {{site.data.keyword.cloud_notm}} resources and data sources, see the [{{site.data.keyword.cloud_notm}} Provider plug-in reference](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-resources-datasource-list).</li></ul>| 
|22 June 2020|<ul><li>**Upload Terraform templates as TAR files**: You can now provide your Terraform template by uploading a tap archive file from your local machine. This feature is supported from the command-line or API. For more information, see the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command or [`PUT /v1/workspaces/{id}/templates/{template_id}/template_repo_upload`](/apidocs/schematics/schematics#upload-a-tar-file-to-your-workspace) API.</li></ul>|

## May 2020
{: #may-2020}

|Date|Description|
|-----|-----------|
|8 May 2020|<ul><li>**New EU API endpoint**: You can now choose to create your {{site.data.keyword.bpshort}} Workspaces in the US or Europe. Depending on the location that you choose, your {{site.data.keyword.bpshort}} Actions run in either the US (`us-south` or `us-east`) or in Europe (`eu-de` or `eu-gb`). The location that you choose for your workspace is independent from the location where you want to provision your resources. For more information, see [Locations and service endpoints](/docs/schematics?topic=schematics-locations).</li></ul> |

## April 2020
{: #april-2020}

|Date|Description|
|-----|-----------|
|17 April 2020|<ul><li>**Terraform v0.12 support**: You can now choose to run your infrastructure code with Terraform version 0.11 or 0.12. With Terraform version 0.12, the syntax for configuration files changed. Make sure that you use the syntax that is compatible with the Terraform version that you want to use. To migrate your Terraform configuration files from version 0.11 to version 0.12, see [Migrating your Terraform configuration files from version 0.11 to version 0.12](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-migration-versioncontrol).  </li><li>**New workspace creation flow**: The workspace creation flow is now [a two-step process](/docs/schematics?topic=schematics-workspace-setup#create-workspace). First, you create the workspace without connecting it to a GitHub or GitLab repository. Then, you add the details of your GitHub or GitLab repository, retrieve input variables, and let {{site.data.keyword.bpshort}} scan your Terraform configuration files for syntax errors. With the change of the workspace creation flow, [new workspace states](/docs/schematics?topic=schematics-workspace-setup#wks-state) are introduced as well.</li></ul> |


