---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-26"

keywords: get started with schematics, infrastructure management, infrastructure as code, iac, schematics cloud environment, schematics infrastructure, schematics terraform, terraform provider
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



# Getting started with infrastructure and cloud service deployment in {{site.data.keyword.bpshort}}
{: #get-started-terraform}

Use one of the IBM-provided templates to create an [{{site.data.keyword.cos_full}} service instance]() that you can use to persistently store your data in {{site.data.keyword.cloud_notm}}. 
{: shortdesc}

A {{site.data.keyword.bplong_notm}} template is a set of files that define the {{site.data.keyword.cloud_notm}} resources that you want to create, update, or delete. You create a {{site.data.keyword.bpshort}} workspace that points to your template and use the built-in capabilities of the {{site.data.keyword.cloud_notm}} provider plug-in for Terraform to provision your {{site.data.keyword.cloud_notm}} resources. For more information about the provider and how {{site.data.keyword.bpshort}} spins up your {{site.data.keyword.cloud_notm}} resources, see [Infrastructure deployment with {{site.data.keyword.bpshort}} workspaces](/docs/schematics?topic=schematics-about-schematics#how-to-workspaces). 

## Before you begin
{: #prereq}

Before you can use this template, you must complete the following tasks. 
{: shortdesc}

- Make sure that you have the permissions to [create a {{site.data.keyword.bpshort}} workspace](/docs/schematics?topic=schematics-access#access-roles). 
- Make sure that you have the permissions to [create an {{site.data.keyword.cos_full_notm}} instance](/docs/cloud-object-storage?topic=cloud-object-storage-iam). 

## Creating an {{site.data.keyword.cos_full_notm}} instance with {{site.data.keyword.bpshort}}
{: #create-cos}

Use the IBM-provided Terraform template to provision an {{site.data.keyword.cos_full_notm}} instance with a {{site.data.keyword.bpshort}} workspace. 
{: shortdesc}

1. From the [{{site.data.keyword.bpshort}} workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, click **Create workspace**. 
2. Enter a name for your workspace, the resource group, and region where you want to create the workspace. Then, click **Create**. 
3. In the **Import your Terraform template** section, enter the following information: 
   1. In the **GitHub, GitLab, or Bitbucket repository URL** field, enter `https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-resource-instance`. 
   2. Select `terraform_v0.12` from the **Terraform version** drop down. 
   3. Click **Save template information** to save the information that you entered. 
4. Enter your template variables. 
   1. Enter a name for your {{site.data.keyword.cos_full_notm}} instance. 
   2. Review the default values for the plan and the resource group name. 
      
      If you already have an existing {{site.data.keyword.cos_full_notm}} instance in your account, you must enter `standard` in the **plan** field. 
      {: tip}
      
   3. Click **Save changes** to save your variable values. 
5. From the workspace **Settings** page, click **Generate plan**. After you click this button, the workspace **Activity** page opens and {{site.data.keyword.bpshort}} gathers the actions that need to run to provision your Terraform template. Click **View logs** to find detailed information about the actions that {{site.data.keyword.bpshort}} identified. 
6. From the workspace **Activity** page, click **Apply plan**. After you click this button, {{site.data.keyword.bpshort}} starts to provision your {{site.data.keyword.cos_full_notm}} instance as specified in your Terraform template. This process might take a few minutes to complete. Click **View logs** to see the details of the provisioning process.  
7. Check out your {{site.data.keyword.cos_full_notm}} instance. 
   1. From the [{{site.data.keyword.cloud_notm}} resource list](https://cloud.ibm.com/resources), select the instance that you created. 
   2. From the menu, select **Getting started** to find more information about {{site.data.keyword.cos_full_notm}} and how you can create your first bucket to start storing your data. 
   

Congratulations! You used the built-in Terraform capabilities of {{site.data.keyword.bpshort}} to create an {{site.data.keyword.cos_full_notm}} service instance in your {{site.data.keyword.cloud_notm}} account. 
   
 
## What's next? 
{: #whats-next}

Now that you created your first {{site.data.keyword.cloud_notm}} resource with {{site.data.keyword.bpshort}}, you can explore the following options: 

- Try out this [IBM-provided template](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cos-bucket){: external} to create a bucket in the {{site.data.keyword.cos_full_notm}} instance that you created with {{site.data.keyword.bpshort}}. 
- Learn how to [create your own Terraform template](/docs/schematics?topic=schematics-create-tf-config). 
- Explore other [IBM-provided templates](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples){: external}.
- Set up the {{site.data.keyword.bpshort}} [CLI](/docs/schematics?topic=schematics-setup-cli) or [API](/docs/schematics?topic=schematics-setup-api) to start automating {{site.data.keyword.cloud_notm}} resources. 

