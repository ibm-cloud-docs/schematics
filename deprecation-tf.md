---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-07"

keywords: terraform version deprecation, deprecation, terraform support schematics

subcollection: schematics

---

{:new_window: target="_blank"}
{:table: .aria-labeledby="caption"}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}
{:codeblock: .codeblock}
{:external: target="_blank" .external}
{:important: .important}
{:note: .note}
{:pre: .pre}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:tip: .tip}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}
{:reactnative: .ph data-hd-programlang='React Native'}
{:csharp: .ph data-hd-programlang='csharp'}
{:ios: .ph data-hd-programlang='iOS'}
{:android: .ph data-hd-programlang='Android'}
{:cordova: .ph data-hd-programlang='Cordova'}
{:xml: .ph data-hd-programlang='xml'}
{:curl: .ph data-hd-programlang='curl'}
{:node: .ph data-hd-programlang='node'}

## Deprecating Terraform versions in {{site.data.keyword.bplong_notm}}
{: #deprecate-tf-version}

{{site.data.keyword.bplong}} continues to update the service with the latest Terraform provider version support, and accordingly will be deprecating some of the older versions of Terraform providers. This announcement is to publish an ongoing plan and timeline, and prepare you to migrate to the more current versions of Terraform providers. By moving to the newer versions of Terraform, you can leverage the latest features and capabilities of the {{site.data.keyword.cloud}} service providers.
{: shortdesc}

## Deprecating Steps
{: #deprecate-steps}

The deprecation of each Terraform version follow these steps:
    1. **Restrict creation workspace** You cannot create the {{site.data.keyword.bplong_notm}} workspace with that older version, but can continue to manage the {{site.data.keyword.cloud_notm}} resources by using the existing {{site.data.keyword.bplong_notm}} workspaces.
    2. **Restrict workspace execution** You cannot manage {{site.data.keyword.cloud_notm}} resources with these {{site.data.keyword.bplong_notm}} workspaces with the deprecated Terraform version. You can only read the {site.data.keyword.bplong_notm}} workspaces contents.

## Deprecating timeline
{: #deprecate-timeline}

The following table lists the timeline for the deprecation of Terraform provider versions from the {{site.data.keyword.bplong_notm}} service. 

Months listed below are the last day of that month.
{: note}

| Versions | Phase 1 Restrict creation workspace (End of marketing) | Phase 2 Restrict workspace execution (End of support)|
| ----- | ------ | ----- |
| Terraform v0.11 | October 2021 | December 2021 |
| Terraform v0.12 | January 2021 | March 2022 |
| Terraform v0.13 | March 2022 | May 2022 |
| Terraform v0.14 | May 2022 | July 2022 |
| Terraform v0.15 | July 2022 | December 2022 |
| Terraform v1.0  | March 2023 | June 2023 |
{: caption="Deprecation timeline of Terraform provider" caption-side="top"}

You are suppose to migrate from your current version of Terraform to the latest available version at the right time. You can view the {{site.data.keyword.bplong_notm}} workspace configuration page and select the Terraform version that your Terraform configuration files are written.

{{site.data.keyword.bplong_notm}} announces the depreciation of Terraform v0.11 from `July 2021`. The `end of marketing` (you cannot deploy the deprecated version) of Terraform v0.11 from `October 2021` and `end of support` (you cannot get the support of the deprecated version) of Terraform v0.11 from `December 2021`.

## Working with the latest version of Terraform
{: #working-new-version}

Follow these steps to continue working with the latest versions of Terraform in the {{site.data.keyword.bplong_notm}}.

1. **Identification** Identify the version of Terraform in your {{site.data.keyword.bplong_notm}} workspaces. The {{site.data.keyword.bplong_notm}} workspace list indicates the versions of the Terraform provider that you are using. Also, individual {{site.data.keyword.bplong_notm}} workspace `settings` page in the console indicates the Terraform version for that workspace. If you are using the command line run [`ibmcloud schematics workspace list`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-list) command to list the Terraform version.

2. **Migration** Migrate an older Terraform version to the supported versions, in case you want to deploy by using {{site.data.keyword.bplong_notm}} after the version's `end of support`. For more information, about migrating Terraform version, see [Upgrade Terraform version in {{site.data.keyword.bpshort}} workspace](/docs/schematics?topic=schematics-migrating-terraform-version#migrate-steps).

3. **Verification** You can verify that the workspaces are properly migrated by accessing the list of Terraform version that the target version you want to access in {{site.data.keyword.bpshort}} workspace. Then run the [`ibmcloud schematics refresh`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-refresh) and [`ibmcloud schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) commands, to verify the migrated Terraform version works properly.

Once you are at a newer version of the Terraform provider, you can continue using {{site.data.keyword.bplong_notm}} workspaces normally. 


