---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-12"

keywords: terraform version deprecation, deprecation, terraform support schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deprecating Terraform versions 
{: #deprecate-tf-version}

{{site.data.keyword.bplong}} continues to update the service with the latest version of Terraform provider support, and accordingly deprecates the older versions of Terraform provider. {{site.data.keyword.bpshort}} has an ongoing plan for deprecating older versions of Terraform that can enable you to be prepared to migrate to the latest versions of Terraform providers. This is a tentative timeline for the deprecation of Terraform versions.
{: shortdesc}

## Phases
{: #deprecate-phase}

The deprecation of each Terraform version follows these phase:
1. **Restrict workspace creation** You cannot create the {{site.data.keyword.bpshort}} Workspaces with that older version, but can continue to manage the {{site.data.keyword.cloud_notm}} resources by using the existing {{site.data.keyword.bplong_notm}} Workspaces.

2. **Restrict workspace execution** You can no longer manage {{site.data.keyword.cloud_notm}} resources with these {{site.data.keyword.bplong_notm}} Workspaces with the deprecated Terraform version. You can read the {{site.data.keyword.bplong_notm}} Workspaces contents.

If you choose not to upgrade to the latest version of Terraform beyond the **restrict workspace execution** phase:
- Your {{site.data.keyword.bpshort}} Workspaces data will continue to stay in {{site.data.keyword.bpshort}} till you **delete**.
- You cannot **destroy** the {{site.data.keyword.cloud_notm}} resources, managed by the IBM {{site.data.keyword.bplong_notm}} Workspace.

## Schedule
{: #deprecate-timeline} 

You are recommended to migrate from your current version of Terraform to the latest available version at the right time. The latest version is always the most appropriate version. You can see the latest version of the Terraform in the drop down list of the {{site.data.keyword.bpshort}} Workspaces configuration page.
{: shortdesc}

We announce the timeline for the deprecation of Terraform **versions**, the related **end of marketing** date and **end of support** date of the {{site.data.keyword.bplong_notm}} service. The Month provided in the table represents the last day of the Month to restrict workspace creation and execution. These depreciation timeline might change over time to meet your business needs. 


| Versions | Phase 1: Restrict workspace creation (End of marketing) | Phase 2: Restrict workspace execution (End of support)|
| ----- | ------ | ----- |
| `Terraform v0.11` | October 2021 | December 2021 |
| `Terraform v0.12` | January 2022 | March 2022 |
| `Terraform v0.13` | March 2022 | May 2022 |
| `Terraform v0.14` | May 2022 | July 2022 |
| `Terraform v0.15` | July 2022 | December 2023 |
| `Terraform v1.0`  | March 2023 | December 2024 |
{: caption="Deprecation timeline as on 11th August, 2021." caption-side="top"}


## User actions
{: #user-action}

Follow these steps to continue working with the latest versions of Terraform in the {{site.data.keyword.bplong_notm}}.

1. **Identification**: Identify the version of Terraform in your {{site.data.keyword.bplong_notm}} Workspaces. The {{site.data.keyword.bpshort}} Workspaces list indicates the versions of the Terraform provider that you are using. Also, individual {{site.data.keyword.bpshort}} Workspaces [settings page](/docs/schematics?topic=schematics-workspace-setup#import-template) in the console indicates the Terraform version for that workspace. If you are using the command-line run [`ibmcloud schematics workspace list`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-list) command to list the Terraform version.

2. **Migration**: Migrate an older Terraform version to the supported versions, in case you want to deploy by using {{site.data.keyword.bplong_notm}} after the version's `end of support`. For more information about migrating Terraform version, see [Upgrade Terraform version in {{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-migrating-terraform-version#migrate-steps12).

3. **Verification**: You can verify that the workspaces are properly migrated by accessing the list of Terraform version that the target version you want to access in {{site.data.keyword.bpshort}} Workspace. Then run the [`ibmcloud schematics refresh`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-refresh) and [`ibmcloud schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) commands, to verify the migrated Terraform version works properly.

Now you are at a latest version of the Terraform provider, and can continue by using the {{site.data.keyword.bplong_notm}} Workspaces as usual.

</staging>

## Schedule draft
{: #new-schedule}

{{site.data.keyword.bpshort}} follows a rolling update policy inline with HashiCorp Configuration Language (HCL) Terraform release cycle. {{site.data.keyword.bpshort}} maintains at least the latest two major releases in line with HCL support and end-of-life policy https://support.hashicorp.com/hc/en-us/articles/360021185113-Support-Period-and-End-of-Life-EOL-Policy {: external}.
{: shortdesc}

You are recommended always to migrate from your current in use version of Terraform to the latest available version and to remain on HCL supported Terraform versions. You can see the latest version of the Terraform in the drop down list of the [{{site.data.keyword.bpshort}} Workspaces](https://cloud.ibm.com/schematics/workspaces/create){: external} configuration page. 

{{site.data.keyword.bpshort}} announces the timeline for the deprecation of Terraform versions, the related end of marketing date, and end of support date of the {{site.data.keyword.bplong_notm}} service. The Month provided in the table represents the last day of the Month to restrict workspace creation and execution. The depreciation timeline might change over time to meet your business needs.

|Versions | Phase 1: Restrict workspace creation (End of marketing)|    Phase 2: Restrict workspace execution (End of support)|      End of HCL end maintenance and security support|
| -- | -- | --| --|
| Terraform v0.15 | July 2022  |	December 2023	|	July 2021 |
| Terraform v1.0 |	Sept 2023 |   December 2023	|	May 2022 |
| Terraform v1.1 |  Sept 2023	|	  December 2023	|	Sept 2022 |
| Terraform v1.2 |  Sept 2023	|	  December 2023	|	March 2023 |
| Terraform v1.3 |  Earliest March 2024	|  Earliest	July 2024	|	Expected before end 2023 |
| Terraform v1.4 |	Earliest Sept 2024  |  Earliest December 2023 |	Expected before end 2024 |
{: caption="Deprecation timeline as on 12th April, 2023." caption-side="top"}

</staging>
