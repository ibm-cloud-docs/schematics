---

copyright:
  years: 2017, 2021
lastupdated: "2021-07-20"

keywords: schematics multi region, deploy across regions schematics, multi location deployment, multi region deployment

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
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
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
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
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
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
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


# Deploying {{site.data.keyword.cloud_notm}} resources in a specific region or across multiple regions
{: #multi-region-deployment}

{{site.data.keyword.bplong}} enables you to deploy resources in any {{site.data.keyword.cloud_notm}} location or region globally. The region where you save and execute your {{site.data.keyword.bpshort}} `Workspaces` and `Actions` is independent of the region where your {{site.data.keyword.cloud_notm}} resources are deployed or configured.

{{site.data.keyword.bplong_notm}} executes your jobs from your selected {{site.data.keyword.bpshort}} region and remotely access the services to provision resources in the target regions determined by your Terraform templates. It is unaffected by network latency between regions. For example, If you create a workspace in `us-south`, you can use this workspace to provision services in any supported {{site.data.keyword.cloud_notm}} region, such as `us-east` or `eu-gb`. The location of your workspace determines where your workspace data is stored and where your workspace requests are run. For more information, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location)

## Deploying services in a specific region
{: #single-region}

If all of your services can be deployed to the same region, you can specify the region in the `provider` block of your Terraform template. 

If no region is specified in the `provider` block, {{site.data.keyword.bpshort}} automatically attempts to create your {{site.data.keyword.cloud_notm}} resources in the `us-south` region.

1. Open the `provider.tf` file or the Terraform configuration file that contains the `provider` block. 
2. Specify the region where you want to deploy your services. Make sure that the region that you enter is supported by the service that you want to deploy with {{site.data.keyword.bpshort}}.
   ```
   provider "ibm" {
     region = "<region_name>"
   }
   ```
   {: pre}
   
3. Check if the service that you want to deploy requires a `location` parameter to be set in addition to the `region` parameter. For example, if you use the [`ibm_database` Terraform resource](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/database), you must set both the `region` parameter in the `provider` block and the `location` parameter in the `ibm_database` resource definition. 

4. Follow the [steps](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources) to deploy your {{site.data.keyword.cloud_notm}} resources. 


## Deploying services across regions
{: #across-regions}

You can add multiple multiple provider configurations to the `provider` block to specify the regions where you want to deploy your {{site.data.keyword.cloud_notm}} resources. For more information, see [Multiple Provider Instances](https://www.terraform.io/docs/language/providers/configuration.html#alias-multiple-provider-configurations){: external}.
{: shortdesc}

1. In the `provider` block of your Terraform configuration file or the `provider.tf` file, create multiple provider blocks with the same provider name. The provider configuration without an alias is considered the default provider configuration and is used for every resource where you do not specify a specific provider configuration. If you add more provider configurations, you must include an alias so that you can reference this provider from your resource definition in the Terraform configuration file. In the following example, the default provider configuration deploys resources in `us-south` while the provider configuration with the alias `east` deploys all resources in `us-east`.
   ```
   provider "ibm" {
     region = "us-south"
   }
   
   provider "ibm" {
     alias = "east"
     region = "us-east"
   }
   ```
   {: codeblock}
   
2. In your resource definition of your Terraform configuration file, specify the provider configuration that you want to use. If you do not specify a provider, the default provider configuration is used.
   ```
   resource "ibm_container_cluster" "cluster" {
     provider = ibm.east
   ...
   }
   
   resource ibm_is_vpc "vpc" {
     name = "myvpc"
   }
   ```
   {: codeblock}

3. Follow the [steps](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources) to deploy your {{site.data.keyword.cloud_notm}} resources. 


