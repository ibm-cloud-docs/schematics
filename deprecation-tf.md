---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-18"

keywords: terraform version deprecation, deprecation, terraform support schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Terraform version deprecation 
{: #deprecate-tf-version}

{{site.data.keyword.bpshort}} follows a rolling update policy inline with Terraform release cycle. {{site.data.keyword.bpshort}} maintains at a minimum the latest two major releases in line with Terraform support and end-of-life policy https://support.hashicorp.com/hc/en-us/articles/360021185113-Support-Period-and-End-of-Life-EOL-Policy {: external}. Due to compliance and security considerations it is recommended to always remain on a currently in support Terraform version.  
{: shortdesc}

## Phases
{: #deprecate-phase}

After the end of Terraform maintenance and security support, the deprecation of use of each Terraform version in {{site.data.keyword.bpshort}} follows these phases:
1. **Restrict workspace creation** You cannot create new {{site.data.keyword.bpshort}} workspaces. Existing workspaces and resources can continue to be managed using the old version while the workspace is upgraded to the latest Terraform version. 

2. **Restrict workspace execution** You will no longer be able to manage {{site.data.keyword.cloud_notm}} resources with these {{site.data.keyword.bplong_notm}} workspaces with the deprecated Terraform version until the workspace is upgraded to a supported version. {{site.data.keyword.bplong_notm}} Workspaces contents can be read. 

If you choose not to upgrade to the latest version of Terraform beyond the **restrict workspace execution** phase:
- Your {{site.data.keyword.bpshort}} Workspaces data will continue to stay in {{site.data.keyword.bpshort}} until they are **deleted**.
- You cannot **destroy** the {{site.data.keyword.cloud_notm}} resources, managed by the IBM {{site.data.keyword.bplong_notm}} Workspace.

## Schedule
{: #deprecate-timeline} 

You are recommended always to migrate from your current in use version of Terraform to the latest available version and to remain on supported Terraform versions. You can see the latest in use version of Terraform in the drop down list of the [{{site.data.keyword.bpshort}} Workspaces](https://cloud.ibm.com/schematics/workspaces/create){: external} configuration page. 
{: shortdesc}

{{site.data.keyword.bpshort}} announces the timeline for the deprecation of Terraform versions, the related end of marketing date, and end of support date of the {{site.data.keyword.bplong_notm}} service. The Month provided in the table represents the last day of the Month to restrict workspace creation and execution. The depreciation timeline will be revised as new Terraform versions are released.  

|Versions | Phase 1: Restrict workspace creation (End of marketing)|    Phase 2: Restrict workspace execution (End of support)|      Terraform of end maintenance and security support|
| -- | -- | --| --|
| Terraform v0.15 | July 2022  |	December 2023	|	July 2021 |
| Terraform v1.0 |	Sept 2023 |   December 2023	|	May 2022 |
| Terraform v1.1 |  Sept 2023	|	  December 2023	|	Sept 2022 |
| Terraform v1.2 |  Sept 2023	|	  December 2023	|	March 2023 |
| Terraform v1.3 |  Earliest March 2024	|  Earliest	July 2024	|	Expected before end 2023 |
| Terraform v1.4 |	Earliest Sept 2024  |  Earliest December 2023 |	Expected before end 2024 |
{: caption="Deprecation timeline as on 12th April, 2023." caption-side="top"}



## User actions
{: #user-action}

Follow these steps to continue working with the latest versions of Terraform in the {{site.data.keyword.bplong_notm}}.

1. **Identification**: Identify the version of Terraform in your {{site.data.keyword.bplong_notm}} Workspaces. The {{site.data.keyword.bpshort}} Workspaces list indicates the version Terraform in use. Also, individual {{site.data.keyword.bpshort}} workspace [settings pages](/docs/schematics?topic=schematics-workspace-setup#import-template) in the console indicate the in use Terraform version. If you are using the command-line run [`ibmcloud schematics workspace list`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-list) command to list the Terraform version.

2. **Migration**: Migrate older Terraform versions to the latest supported version. For more information about migrating Terraform version, see [Upgrading the Terraform workspace version](/docs/schematics?topic=schematics-migrating-terraform-version#migrate-steps12).

3. **Verification**: You can verify that the workspaces are properly migrated by accessing the list of Terraform version that the target version you want to access in {{site.data.keyword.bpshort}} Workspace. Then run the [`ibmcloud schematics refresh`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-refresh) and [`ibmcloud schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) commands, to verify the migrated Terraform version works properly.

Now you are at a latest version of Terraform, and can continue using the {{site.data.keyword.bplong_notm}} Workspaces.

