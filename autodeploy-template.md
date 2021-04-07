---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-07"

keywords: schematics action deployment, automation, schematics workspace,  schematics workspace creation, auto deploy

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



# Creating a {{site.data.keyword.bpshort}} action URL to deploy to {{site.data.keyword.cloud_notm}}
{: #auto-deploy-url}

You can use {{site.data.keyword.bpshort}} action to configure your {{site.data.keyword.cloud}} resources, and to perform operations on the configured resources. 

If you notice the `Deploy to IBM Cloud` clickable button in the [Sample Ansible playbook for {{site.data.keyword.cloud_notm}}](/docs/schematics?topic=schematics-sample_actiontemplates), it rapidly accessed the {{site.data.keyword.bpshort}} action and pre-populated with an **Action name**, **Repository URL** directly from the documentation page. Following steps are used to create a `Deploy to IBM Cloud` clickable button for the Git repository. 
{: shortdesc}

1. Create a template by using [sample Ansible Playbooks](https://github.com/Cloud-Schematics/?q=Ansible&type=&language=&sort=){: external}, and publish playbook in a Git repository.
2. Copy the Git repository URL, for example, `https://cloud.ibm.com/schematics/actions/create?name=ansible-app-deploy&url=https://github.com/Cloud-Schematics/ansible-app-deploy`.
3. Use the following syntax and example to automatically deploy the {{site.data.keyword.bpshort}} action.

  **Syntax**

  ```
  https://cloud.ibm.com/schematics/actions/create?name=<name of the action>&url=<template Git repository example url>
  ```
  {: codeblock}

  **Example**

  ```
  https://cloud.ibm.com/schematics/actions/create?name=ansible-app-deploy&url=https://github.com/Cloud-Schematics/ansible-app-deploy
  ```
  {: codeblock}

 The URL contains the name of an action and the Git repository URL that you want to pre-populate on the {{site.data.keyword.bpshort}} action create page as parameters. If you do not provide both parameters, the `Deploy to {{site.data.keyword.cloud_notm}}` link defaults to the **Create an action** page without pre-populating an action name or the Git repository URL.
 {: important}

4. You can also enter the example URL in the browser to view the {{site.data.keyword.bplong_notm}} action UI page with the pre-populated values for  **Action name**, and **Repository URL**.
5. Verify the parameters in the {{site.data.keyword.bpshort}} action console. Then, click the **Create** button.

## Adding an image on deployment to {{site.data.keyword.cloud_notm}} hyper link
{: #add_an_image}

You can add an image button with `Deploy to {{site.data.keyword.cloud_notm}}` text. You can use `draw.io`, or any tool to create an image. Save the image in `.png` exension. 

Record the coordinates of the image to make the image clickable by using object mapping.
{: note}  

**Syntax**

The object mapping syntax to create a clickable image.

```
<img usemap="#<USEMAP_NAME>" src="images/autodeploy_button.png"><map name="<USEMAP_NAME>" alt="<ATERNATIVE_TEXT>">
  <area alt="<ALT_TEXT>" title="<TITLE>" href="<SCHEMATICS_ACTION_UI_QUERYSTRINGS>" target="_blank" coords="" shape="rect">
</map>
```
{: codeblock}

**Example**

The object mapping example to create a clickable image.

```
<img usemap="#deploybutton_map" src="images/autodeploy_button.png"><map name="deploybutton_map" alt="This image leads to create an action.">
  <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-is-instance-actions&url=https://github.com/Cloud-Schematics/ansible-is-instance-actions" target="_blank" coords="1,3,139,20" shape="rect">
</map>
```
{: codeblock}

**Output**

<img usemap="#deploybutton_map" src="images/autodeploy_button.png"><map name="deploybutton_map" alt="This image leads to create an action.">
  <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-is-instance-actions&url=https://github.com/Cloud-Schematics/ansible-is-instance-actions" target="_blank" coords="1,3,139,20" shape="rect">
</map>

To view the sample template examples, refer [Sample action templates and deploy to {{site.data.keyword.cloud_notm}}](/docs/schematics?topic=schematics-sample_actiontemplates).
