---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-07"

keywords: terraform version deprecation, deprecation, terraform support schematics

subcollection: schematics

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
{:c#: .ph data-hd-programlang='c#'}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: #curl .ph data-hd-programlang='curl'}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: .external target="_blank"}
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
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:middle: .ph data-hd-position='middle'}
{:navgroup: .navgroup}
{:new_window: target="_blank"}
{:node: .ph data-hd-programlang='node'}
{:note .note}
{:note: .note}
{:note:.deprecated}
{:objectc data-hd-programlang="objectc"}
{:objectc: .ph data-hd-programlang='Objective C'}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:right: .ph data-hd-position='right'}
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
{:step: data-tutorial-type='step'} 
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:topicgroup: .topicgroup}
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


# Deprecating Terraform versions in {{site.data.keyword.bplong_notm}}
{: #deprecate-tf-version}

{{site.data.keyword.bplong}} continues to update the service with the latest Terraform provider version support, and accordingly will be deprecating some of the older versions of Terraform providers. This announcement is to publish an ongoing plan and timeline, and prepare you to migrate to the more current versions of Terraform providers. By moving to the newer versions of Terraform, you can leverage the latest features and capabilities of the {{site.data.keyword.cloud}} service providers.
{: shortdesc}

## Steps
{: #deprecate-steps}

The deprecation of each Terraform version follows these step:
1. **Restrict workspace creation** You cannot create the {{site.data.keyword.bplong_notm}} workspace with that older version, but can continue to manage the {{site.data.keyword.cloud_notm}} resources by using the existing {{site.data.keyword.bplong_notm}} workspaces.

2. **Restrict workspace execution** You can no longer manage {{site.data.keyword.cloud_notm}} resources with these {{site.data.keyword.bplong_notm}} workspaces with the deprecated Terraform version. You can only read the {{site.data.keyword.bplong_notm}} workspaces contents.

If you choose not to upgrade to the latest version of Terraform beyond the **restrict workspace execution** step:
- Your {{site.data.keyword.bplong_notm}} workspace data will continue to stay in {{site.data.keyword.bpshort}} until you **delete**.
- You cannot **destroy** the {{site.data.keyword.cloud_notm}} resources, managed by the IBM {{site.data.keyword.bplong_notm}} workspace.


## Timeline
{: #deprecate-timeline} 

You are recommended to migrate from your current version of Terraform to the latest available version at the right time. The latest version is always the most appropriate version. You can see the latest version of the Terraform in the drop down list of the {{site.data.keyword.bplong_notm}} workspace configuration page.
{: shortdesc}

We announce the timeline for the deprecation of Terraform **versions**, the related **end of marketing** date and **end of support** date of the {{site.data.keyword.bplong_notm}} service. The Month provided in the table represents the last day of the Month to restrict workspace creation and execution.

| Versions | Phase 1: Restrict workspace creation (End of marketing) | Phase 2: Restrict workspace execution (End of support)|
| ----- | ------ | ----- |
| Terraform v0.11 | October 2021 | December 2021 |
| Terraform v0.12 | January 2021 | March 2022 |
| Terraform v0.13 | March 2022 | May 2022 |
| Terraform v0.14 | May 2022 | July 2022 |
| Terraform v0.15 | July 2022 | December 2023 |
| Terraform v1.0  | March 2023 | June 2024 |
{: caption="Deprecation timeline of Terraform version" caption-side="top"}


## User actions
{: #user-action}

Follow these steps to continue working with the latest versions of Terraform in the {{site.data.keyword.bplong_notm}}.

1. **Identification** Identify the version of Terraform in your {{site.data.keyword.bplong_notm}} workspaces. The {{site.data.keyword.bplong_notm}} workspace list indicates the versions of the Terraform provider that you are using. Also, individual {{site.data.keyword.bplong_notm}} workspace [settings page](/docs/schematics?topic=schematics-workspace-setup&interface=ui#import-template) in the console indicates the Terraform version for that workspace. If you are using the command line run [`ibmcloud schematics workspace list`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-list) command to list the Terraform version.

2. **Migration** Migrate an older Terraform version to the supported versions, in case you want to deploy by using {{site.data.keyword.bplong_notm}} after the version's `end of support`. For more information, about migrating Terraform version, see [Upgrade Terraform version in {{site.data.keyword.bpshort}} workspace](/docs/schematics?topic=schematics-migrating-terraform-version#migrate-steps).

3. **Verification** You can verify that the workspaces are properly migrated by accessing the list of Terraform version that the target version you want to access in {{site.data.keyword.bpshort}} workspace. Then run the [`ibmcloud schematics refresh`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-refresh) and [`ibmcloud schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) commands, to verify the migrated Terraform version works properly.

Now you are at a newer version of the Terraform provider, you can continue using {{site.data.keyword.bplong_notm}} workspaces normally.