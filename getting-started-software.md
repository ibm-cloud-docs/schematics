---

copyright:
  years: 2017, 2021
lastupdated: "2021-09-07"

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
{:audio: .audio}
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
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:objectc: data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:release-note: data-hd-content-type='release-note'}
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


# Getting started with software deployment in {{site.data.keyword.bplong_notm}}
{: #get-started-software}

Try out one of the IBM-provided software templates to quickly spin up a classic virtual server instance (VSI), and automatically configure the instance to connect to an {{site.data.keyword.databases-for-postgresql_full}} instance. 
{: shortdesc}

With {{site.data.keyword.bplong_notm}}, you can choose from a wide variety of [software and infrastructure templates](https://cloud.ibm.com/catalog#software){: external} that you can use to set up {{site.data.keyword.cloud_notm}} services, and to install IBM and 3rd party software. The templates are applied by using the built-in `Terraform`, `Ansible`, `Helm`, `CloudPak`, and `Operator` capabilities in {{site.data.keyword.bpshort}}.

As part of this getting started tutorial, you create a {{site.data.keyword.bpshort}} workspace that points to the [VSI database](https://cloud.ibm.com/catalog/content/VSI-database#about){: external} template. Then, you run this template and watch {{site.data.keyword.bpshort}} provision your VSI and your {{site.data.keyword.databases-for-postgresql_full_notm}} instance. {{site.data.keyword.databases-for-postgresql_full_notm}} is a fully managed database offering in {{site.data.keyword.cloud_notm}} that supports storing of non-relational and relational data types. For more information about this offering, see [What is PostgreSQL?](https://www.ibm.com/cloud/learn/postgresql){: external}. 

This getting started tutorial incurs costs. You must have an {{site.data.keyword.cloud_notm}} Pay-As-You-Go or Subscription account to proceed. Make sure that you review pricing information for [classic VSIs](https://cloud.ibm.com/gen1/infrastructure/provision/vs){: external} and [PostgreSQL](https://cloud.ibm.com/databases/databases-for-postgresql/create){: external}. 
{: important}

## Before you begin
{: #vsi-postgres-prereq}

Before you can use this template, you must complete the following tasks.

- Make sure that you have the permissions to [create classic virtual servers](/docs/virtual-servers?topic=virtual-servers-managing-device-access). 
- [Create a classic API key](/docs/account?topic=account-classic_keys#create-classic-infrastructure-key) and retrieve your classic infrastructure username. This username and API key are used to verify that you have sufficient permissions to create classic infrastructure. 
- Make sure that you have the permissions to create an [{{site.data.keyword.databases-for-postgresql_full}} instance](/docs/databases-for-postgresql?topic=cloud-databases-iam). 


## Setting up and configuring a classic VSI to run PostgreSQL with {{site.data.keyword.bpshort}}
{: #vsi-postgres}

Use one of the IBM-provided software templates to set up and configure a classic virtual server instance so that you can store data in an instance of {{site.data.keyword.databases-for-postgresql_full_notm}}. 
{: shortdesc}

1. Open the [**VSI database** software template](https://cloud.ibm.com/catalog/content/VSI-database){: external} from the {{site.data.keyword.cloud_notm}} catalog. 
2. In the **Configure your workspace** section, enter a name for your {{site.data.keyword.bpshort}} workspace and select the resource group where you want to create the workspace.
3. In the **Set the deployment values** section, enter the following information. 
    1. Enter a username and password that you want to use to log in to your PostgreSQL instance. The username must be between 10 and 32 characters long. 
    2. Enter the classic infrastructure username and API key that you retrieved earlier. For more information about how to retrieve this information, see [Creating a classic infrastructure API key](/docs/account?topic=account-classic_keys#create-classic-infrastructure-key). 
    3. Select the resource group where you want to provision your virtual server and `PostregSQL` instance. 
4. Accept the license agreement, and click **Install**. You are redirected to the {{site.data.keyword.bpshort}} workspace **Activity** page where you can monitor the progress of your VSI and PostgreSQL setup. Note that it takes a few minutes for the setup to complete. 
5. Verify your virtual server and PostgreSQL setup. 
    1. From the workspace **Resources** page, find the virtual server and PostgreSQL instance that were created for you. 
    2. Click the link to see the details of your instances. 
6. Optional: Remove your {{site.data.keyword.bpshort}} workspace and all related {{site.data.keyword.cloud_notm}} resources. 
    1. From the **Actions** menu, click **Delete**. 
    2. Select the **Delete workspace** and **Delete all associated resources** option.
    3. Enter the name of your workspace, and click **Delete**. 

Congratulations! You used the capabilities of {{site.data.keyword.bpshort}} to provision {{site.data.keyword.cloud_notm}} infrastructure and database services, and automatically configured your services to allow network communication. 

## What's next? 
{: #whats-next}

- Explore the capabilities of [{{site.data.keyword.databases-for-postgresql_full_notm}}](/docs/databases-for-postgresql?topic=databases-for-postgresql-getting-started).
- Browse other [software and infrastructure templates](https://cloud.ibm.com/catalog#software){: external} that you can apply with {{site.data.keyword.bpshort}}.  
- Learn more about the [built-in capabilities in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-about-schematics).  
- Set up the {{site.data.keyword.bpshort}} [CLI](/docs/schematics?topic=schematics-setup-cli) or [API](/docs/schematics?topic=schematics-setup-api) to start automating the provisioning and management of {{site.data.keyword.cloud_notm}} resources. 




