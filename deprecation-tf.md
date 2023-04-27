---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-27"

keywords: terraform version deprecation, deprecation, terraform support schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Terraform release deprecation lifecycle 
{: #deprecate-tf-version}

It is recommended to always upgrade to the latest Terraform release supported by {{site.data.keyword.bpshort}}. For compliance and security considerations remain on a Terraform release with Hashicorp provided maintenance and security fixes. Refer to the table below for information about of Terraform fix support and {{site.data.keyword.bpshort}} end of marketing dates for Terraform releases, and end of support.     

For more information about updating the in use Terraform release, see [Upgrading the Terraform workspace version](/docs/schematics?topic=schematics-migrating-terraform-version#migrate-steps12). Terraform v1.0 was a major release for Terraform, marking the transition to a stable 1.x release. Hashicorp made [compatibility promises for the 1.x releases](https://developer.hashicorp.com/terraform/language/v1-compatibility-promises), that for core Terraform features and function, no changes are required to HCL templates to upgrade through the 1.x releases. 
{: shortdesc} 

## {{site.data.keyword.bpshort}} Terraform depreciation lifecycle 
{: #deprecate-phase}

The table outlines the timetable of support provided by {{site.data.keyword.bpshort}} for Terraform releases. Refer to the sections below for more details.   


|Timescale | {{site.data.keyword.bpshort}} <br> operations | Terraform Maintenance <br> and security fixes | Upgrade | 
| -- | -- | --| --| 
| 0-6 months  | Full operations | Yes | Recommended |
| 6-12 months | Full operations | No  | Recommended. Upgrade if Terraform fix required  |
| 12 - 24 months |	Workspace creation restricted | No  | Recommended. Upgrade if Terraform fix required | 	
| 24 months | Workspace execution restricted | No | Required | 

### Terraform maintenance and fixes 
{{site.data.keyword.bpshort}} supports Terraform releases in line with [Terraform support and end-of-life policy ](https://support.hashicorp.com/hc/en-us/articles/360021185113-Support-Period-and-End-of-Life-EOL-Policy){: external}. {{site.data.keyword.bpshort}} always supports at least one Terraform release with maintenance and security fix support. 

After the end of Terraform maintenance and security fixes, {{site.data.keyword.bpshort}} maintains full operational support for 24 months from release GA. If an issue is identified in Terraform that requires a fix, the user is required to update to a release with current maintenance and security fixes. 

After end of Terraform maintenance {{site.data.keyword.bpshort}} will move to only supporting operations with the final point (patch) Terraform release. 

The deprecation of use of each Terraform version in {{site.data.keyword.bpshort}} follows the phases below. 

### Restrict workspace creation 
After 12 months from GA, users cannot create new {{site.data.keyword.bpshort}} workspaces with this release. Existing workspaces and resources using the release can continue to be managed using the release. It is recommended to update to the latest supported Terraform release.  

During this time {{site.data.keyword.bpshort}} will only support operations with the final point (patch) Terraform release.   

### Restrict workspace execution
After 24 months from GA, users will no longer be able to manage {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bplong_notm}} workspaces using this release. The workspace must first be updated to use a release of Terraform with Hashicorp provided maintenance and security fixes. The content of these workspaces remains accessible and the Terraform release can be updated in {{site.data.keyword.bpshort}} to re-enable operations.     

If you choose not to upgrade to the latest version of Terraform beyond the **restrict workspace execution** date:
- Your {{site.data.keyword.bpshort}} workspace data will continue to stay in {{site.data.keyword.bpshort}} until you delete these workspaces.
- You cannot perform operations on, or **destroy** the {{site.data.keyword.cloud_notm}} resources using {{site.data.keyword.bpshort}}. The resources can still be deleted via the {{site.data.keyword.cloud_notm}} console or CLI. 

## Depreciation Schedule
{: #deprecate-timeline} 

You are recommended always to migrate from your in use version of Terraform to the latest available version and to remain on Terraform versions with maintenance and security fixes. You can see the latest in use version of Terraform in the drop down list of the [{{site.data.keyword.bpshort}} Workspaces](https://cloud.ibm.com/schematics/workspaces/create) configuration page. 
{: shortdesc}

{{site.data.keyword.bpshort}} announces the timeline for the deprecation of Terraform versions, the related end of marketing date, and end of support date when using the {{site.data.keyword.bplong_notm}} service. The month provided in the table represents the last day of the Month to restrict workspace creation and execution. The depreciation timeline will change as new Terraform versions are released.  

|Versions | Terraform of end maintenance and security support | Phase 1: Restrict workspace creation <br> (End of marketing)|    Phase 2: Restrict workspace execution <br> (End of support)|      
| -- | -- | --| --|
| Terraform v0.x  | 2021 and earlier | May 2022 |  July 2022 |     
| Terraform v0.15 | July 2021 | September 2023  |	September 2024	|	
| Terraform v1.0 |	May 2022 | September 2023 | September 2024	|	
| Terraform v1.1 |  Sept 2022 | September 2023 | September 2024	|	
| Terraform v1.2 |  March 2023 | March 2024	|	March 2025	|	
| Terraform v1.3 |  Expected July 2023 | Earliest July 2024	|  Earliest	July 2025	|	
| Terraform v1.4 |	Expected September 2023 | Earliest September 2024  |  Earliest September 2025 |	
{: caption="Deprecation timeline as on 27th April, 2023." caption-side="top"}

## User actions
{: #user-action}

Follow these steps to upgrade to continue working with the latest versions of Terraform in {{site.data.keyword.bplong_notm}}.

1. **Identification**: Identify the version of Terraform in your {{site.data.keyword.bplong_notm}} Workspaces. The {{site.data.keyword.bpshort}} Workspaces list indicates the version Terraform in use. Also, individual {{site.data.keyword.bpshort}} workspace [settings pages](/docs/schematics?topic=schematics-workspace-setup#import-template) in the console indicate the in use Terraform version. If you are using the command-line run [`ibmcloud schematics workspace list`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-list) command to list the Terraform version.

2. **Migration**: Migrate older Terraform versions to the latest supported version. For more information about migrating Terraform version, see [Upgrading the Terraform workspace version](/docs/schematics?topic=schematics-migrating-terraform-version#migrate-steps12).

3. **Verification**: You can verify that the workspaces are properly migrated by accessing the list of Terraform version that the target version you want to access in {{site.data.keyword.bpshort}} Workspace. Then run the [`ibmcloud schematics refresh`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-refresh) and [`ibmcloud schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) commands, to verify the migrated Terraform version works properly.

Now you are at a latest version of Terraform, and can continue using the {{site.data.keyword.bplong_notm}} Workspaces.

