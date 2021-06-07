---

copyright:
  years: 2017, 2021
lastupdated: "2021-06-03"

keywords: schematics, schematics timeout, terraform timeout, tainted resources, untaint, taint

subcollection: schematics
content-type: troubleshoot

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


# Why do timeout failures result in tainted {{site.data.keyword.cloud_notm}} resources?
{: #tainted-resources}

{: tsSymptoms}
You attempted to create an {{site.data.keyword.cloud_notm}} resource that takes a long time to fully provision, such as {{site.data.keyword.messagehub}} or {{site.data.keyword.databases-for}}. When you run the {{site.data.keyword.bpshort}} apply action, the action fails due to timeouts resulting in a tainted resource. 

{: tsCauses}
The {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform sets certain timeouts when the provisioning, update, or deletion of a resource must be completed before it is considered failed. Because some resources such as {{site.data.keyword.messagehub}} or {{site.data.keyword.databases-for}} take longer to fully provision, they might exceed these timeouts. If the provisioning cannot be completed before the timeout is reached, the {{site.data.keyword.cloud_notm}} Provider plug-in marks the provisioning process as failed and taints the resource. However, the actual provisioning of the resource is not canceled and continues in the background which can result in a successfully provisioned resource after all. Because the resource is tainted, the resource is automatically deleted and re-created when you run the next {{site.data.keyword.bpshort}} apply action. 

{: tsResolve}
To avoid that a successfully provisioned resource is deleted and re-created, you must untaint the resource. 

1. List the workspaces in your account and note the ID of the workspace that includes the failed resource. 
   ```
   ibmcloud schematics workspace list
   ```
   {: pre}
   
2. Refresh your workspace. A refresh action validates the {{site.data.keyword.cloud_notm}} resources in your account against the state that is stored in the Terraform statefile of your workspace. This process might take a few minutes to complete.
   ```
   ibmcloud schematics refresh --id <workspace_ID>
   ```
   {: pre}
   
3. Retrieve the template ID of your workspace. To template ID is shown as a string after the **Template Variables for: <template_ID>** section of your CLI output. 
   ```
   ibmcloud schematics workspace get --id <workspace_ID>
   ```
   {: pre}
   
4. Retrieve the [Terraform statefile](/docs/schematics?topic=schematics-schematics-cli-reference#state-list) for your workspace and note the name of the resource that is tainted.
   ```
   ibmcloud schematics state pull --id <workspace_ID> --template <template_ID>
   ```
   {: pre}
   
5. Verify that the [tainted resource](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-taint) is successfully provisioned and in a healthy state by using the {{site.data.keyword.cloud_notm}} console, CLI, or API. For example, if you tried to provision an {{site.data.keyword.containerlong_notm}} cluster, check that the cluster is in a `Normal` state and that you can successfully connect to the cluster. 

6. Untaint the resource. Enter the name of the tainted resource that you retrieved from the statefile in the `--address` parameter. For example, a cluster resource name from a statefile might look like this: `ibm_container_vpc_cluster.mycluster`. 
   ```
   ibmcloud schematics workspace untaint --id <workspace_ID> --address <resource_name>
   ```
   {: pre}
   
7. Retrieve the Terraform statefile for your workspace again and verify that your resource is marked as [`untainted`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-untaint).  
   ```
   ibmcloud schematics state pull --id <workspace_ID> --template <template_ID>
   ```
   {: pre}
   
