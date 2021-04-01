---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-01"

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

The deploy to {{site.data.keyword.cloud}} URL is an efficient way to enable users to deploy solutions on {{site.data.keyword.cloud_notm}} from a public Git repository sample configuration. The URL requires minimal configuration and you can insert it anywhere in your documentation that supports markup. When the user clicks the hyper link, they are taken directly to the {{site.data.keyword.bpshort}} action setup page and only need to click the create button for action creation in {{site.data.keyword.bpshort}}.
{: shortdesc}

Follow these steps to create a URL to deploy to Terraform v0.13 template example in {{site.data.keyword.bplong_notm}}.

1. Create a template example by using Terraform provider and publish in the public Git repository. To create example, refer [Sample template example](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples){: external}.
2. Copy the public Git repository URL, for example, `https://cloud.ibm.com/schematics/actions/create?name=ansible-app-deploy&url=https://github.com/Cloud-Schematics/ansible-app-deploy`.
3. Use this syntax to auto deploy the Schematics action creation in the {{site.data.keyword.cloud_notm}}.

  **Syntax**

  ```
  https://cloud.ibm.com/schematics/actions/create?name=<name of the action>&url=<template public Git repository example url>
  ```
  {: pre}

  **Example**

  ```
  https://cloud.ibm.com/schematics/actions/create?name=ansible-app-deploy&url=https://github.com/Cloud-Schematics/ansible-app-deploy
  ```
  {: pre}

  The URL contains two parameters, first parameter is provided with the action name as `your action name` and second parameter is provided with the Git URL repository link as `https://github.com/Cloud-Schematics/ansible-app-deploy`. If you do not provide any parameters or ignore one parameter, the `Deploy to {{site.data.keyword.cloud_notm}}` link defaults to the create an action page.
  {: important}

4. You can also copy, and paste the example URL in the browser to view the {{site.data.keyword.bplong_notm}} action UI page with the create button.
5. Cross-check the parameters in the {{site.data.keyword.bpshort}} action UI and click `Create` button.

## Adding an image on deployment to {{site.data.keyword.cloud_notm}} hyper link
{: #add_an_image}

You can add an image button with `Deploy to {{site.data.keyword.cloud_notm}}` text. You can use `draw.io`, or any tool to create an image. Save the image in `.png` exension. 

Record the coordinates of the image to make the image clickable by using object mapping.
{: note}  

**Syntax**

```
<a href="https://cloud.ibm.com/schematics/actions/create?name=<name of the action>&url=<template public Git repository example url">Deploy to {{site.data.keyword.bplong_notm}} <img src=<image location>></a>
```
{: pre}

```
<img usemap="#<USEMAP_NAME>" src="images/autodeploy_button.png"><map name="<USEMAP_NAME>" alt="<ATERNATIVE_TEXT>">
  <area alt="<ALT_TEXT>" title="<TITLE>" href="<SCHEMATICS_ACTION_UI_QUERYSTRINGS>" target="_blank" coords="" shape="rect">
</map>
```
{: pre}

**Example**

```
<img usemap="#deploybutton_map" src="images/autodeploy_button.png"><map name="deploybutton_map" alt="This image leads to create an action.">
  <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-is-instance-actions&url=https://github.com/Cloud-Schematics/ansible-is-instance-actions" target="_blank" coords="1,3,139,20" shape="rect">
</map>
```
{: pre}

**Output**

</map><br><br><img usemap="#deploybutton_map" src="images/autodeploy_button.png"><map name="deploybutton_map" alt="This image leads to create an action.">
  <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-is-instance-actions&url=https://github.com/Cloud-Schematics/ansible-is-instance-actions" target="_blank" coords="1,3,139,20" shape="rect">
</map>

To view about the sample template examples, refer [Sample action templates and deploy to {{site.data.keyword.cloud_notm}}](/docs/schematics?topic=schematics-sample_actiontemplates).
