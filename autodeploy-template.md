---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-11"

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



# Creating an auto-deploy button for {{site.data.keyword.bpshort}} actions
{: #auto-deploy-url}

Use the instructions on this page to create a button that opens the {{site.data.keyword.bpshort}} action create page and pre-poluates an action name and the GitHub repository URL that stores your Ansible playbook. You can use this button to create {{site.data.keyword.bpshort}} actions more quickly. 
{: shortdesc}

For a sample button, see the `Deploy to IBM Cloud` button on the [Sample Ansible playbook for {{site.data.keyword.cloud_notm}}](/docs/schematics?topic=schematics-sample_actiontemplates) page.
{: tip}

You can use {{site.data.keyword.bpshort}} action to configure your {{site.data.keyword.cloud}} resources, and to perform operations on the configured resources. 
1. Create an Ansible playbook and publish the playbook in a GitHub repository. If you do not have a playbook, you can use one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics/?q=Ansible&type=&language=&sort=){: external}.
2. Copy the Git repository URL, such as `https://github.com/Cloud-Schematics/ansible-app-deploy`. 
3. Use the following syntax to create the URL to automatically pre-populate an action name and the Git repository URL on the {{site.data.keyword.bpshort}} action create page. If you do not provide the name and Git repository URL, the `Deploy to {{site.data.keyword.cloud_notm}}` link defaults to the **Create an action** page without pre-populating an action name or the Git repository URL.

  **Syntax**
  ```
  https://cloud.ibm.com/schematics/actions/create?name=<action_name>&url=<git_repository_url>
  ```
  {: codeblock}

  **Example**
  ```
  https://cloud.ibm.com/schematics/actions/create?name=ansible-app-deploy&url=https://github.com/Cloud-Schematics/ansible-app-deploy
  ```
  {: codeblock}
  
4. Open your web browser and enter the URL.
5. Verify that the {{site.data.keyword.bplong_notm}} action create page opens and that the **Action name** and **Repository URL** are pre-populated.

## Adding an image to your URL to create the auto-deploy button
{: #add_an_image}

You can add an image to your URL to create your `Deploy to {{site.data.keyword.cloud_notm}}` button.

1. Use `draw.io` or any other tool to create an image for your button. Save the image in `.png` extension.
2. Create an image map. 

   **Syntax**: 
   ```
   <img usemap="#<image_map_ID>" src="<path_to_image>"><map name="<image_map_name>" alt="<alt_text>"><area alt="<alt_text>" title="<button_title>" href="<schematics_action_url>" target="_blank" coords="<image_coordinates>" shape="rect"></map>
   ```
   {: codeblock}
   
   **Example**: 
   ```
   <img usemap="#deploybutton_map" src="images/autodeploy_button.png"><map name="deploybutton_map" alt="This image creates a Schematics action."><area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-is-instance-actions&url=https://github.com/Cloud-Schematics/ansible-is-instance-actions" target="_blank" coords="1,3,139,20" shape="rect"></map>
   ```
   {: codeblock}

   For a sample button, see the `Deploy to IBM Cloud` button on the [Sample Ansible playbook for {{site.data.keyword.cloud_notm}}](/docs/schematics?topic=schematics-sample_actiontemplates) page.
{: tip}
