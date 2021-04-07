---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-06"

keywords: action templates, schematics template, terraform template

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




# Sample Ansible playbook templates for {{site.data.keyword.cloud_notm}}
{: #sample_actiontemplates}

You can use {{site.data.keyword.bpshort}} actions to configure your {{site.data.keyword.cloud}} resources, and to perform operations on the configured resources. Here are the sample Ansible Playbooks, that you can explore by using the `View GitHub repo` clickable link, and create the {{site.data.keyword.bpshort}} action by using `Deploy to IBM Cloud` clickable link.
{: shortdesc}

The usage of the clickable links are:
- `View GitHub repo` link opens the [Git repository](https://github.com/Cloud-Schematics) where the respective template is stored. You can review the file structure, the Ansible playbook instructions, and the `README` file that contains the steps to use the template in {{site.data.keyword.bpshort}}.

- `Deploy to IBM Cloud` link takes you to the **Create an action** page with the **GitHub repository URL** and the **Action name** pre-populated.  

Make sure you select the correct location. After an action is created, you cannot update the location and region.
{: note}

## Operations for {{site.data.keyword.BluVirtServers}} on VPC
{: #ansible-vpc}

<table>
   <thead>
    <th style="width:60px">Name</th>
    <th style="width:250px">Description</th>
    <th style="width:150px">Access</th>
  </thead>
  <tbody>
       <tr>
      <td><code>ansible-is-instance-actions</code></td>
      <td>Use this Ansible playbook to perform day 2 operations such as start, stop, and reboot for {{site.data.keyword.vsi_is_short}}. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/ansible-is-instance-actions/blob/master/README.md).</td>
      <td> <img src="images/viewgithubrepo.png" usemap="#viewgithubimage_map">
<map name="viewgithubimage_map">
  <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/ansible-is-instance-actions" target="_blank" coords="3,1,140,20" shape="rect">
</map><br><br><img usemap="#deploybutton_map" src="images/autodeploy_button.png"><map name="images/deploybutton_map" alt="This image leads to create an action.">
  <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-is-instance-actions&url=https://github.com/Cloud-Schematics/ansible-is-instance-actions" target="_blank" coords="1,3,139,20" shape="rect">
</map></td>
 </tr>
 </tbody>
 </table>


## Provisioning a LAMP stack on {{site.data.keyword.vsi_is_short}}
{: #ansible-lamp-stack}

<table>
   <thead>
    <th style="width:60px">Name</th>
    <th style="width:250px">Description</th>
    <th style="width:150px">Access</th>
  </thead>
  <tbody>
     <tr>
      <td><code>lamp-simple</code></td>
      <td>Use this Ansible playbook to deploy the LAMP stack components on a set of {{site.data.keyword.vsi_is_short}} by following a simple deployment architecture. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/lamp-simple/blob/master/README.md).</td>
      <td> <img src="images/viewgithubrepo.png" usemap="#viewgithubimage_map">
<map name="viewgithubimage_map">
  <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/lamp-simple" target="_blank" coords="3,1,140,20"  shape="rect">
</map><br><br><img usemap="#deploybutton_map" src="images/autodeploy_button.png"><map name="images/deploybutton_map" alt="This image leads to create an action.">
  <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=lamp-simple&url=https://github.com/Cloud-Schematics/lamp-simple" target="_blank" coords="1,3,139,20"  shape="rect"></map></td>
 </tr>
 </tbody>
 </table>


## Configuring {{site.data.keyword.databases-for-postgresql_full_notm}} with WAL2JSON plugin
{: #ansible-databases}

<table>
   <thead>
    <th style="width:60px">Name</th>
    <th style="width:250px">Description</th>
    <th style="width:150px">Access</th>
  </thead>
  <tbody>
     <tr>
      <td><code>ansible-icd-postgres-actions</code></td>
      <td>Use this Ansible playbook to configure your {{site.data.keyword.databases-for-postgresql_full_notm}} instance with `WAL2JSON` plugin. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/ansible-icd-postgres-actions/blob/master/README.md).</td>
      <td> <img src="images/viewgithubrepo.png" usemap="#viewgithubimage_map">
<map name="viewgithubimage_map">
  <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/ansible-icd-postgres-actions" target="_blank" coords="3,1,140,20"  shape="rect">
</map><br><br><img usemap="#deploybutton_map" src="images/autodeploy_button.png"><map name="deploybutton_map" alt="This image leads to create an action.">
  <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=lamp-simple&url=https://github.com/Cloud-Schematics/ansible-icd-postgres-actions" target="_blank" coords="1,3,139,20" shape="rect"></map></td>
 </tr>
 </tbody>
 </table>


## Automating application deployment on {{site.data.keyword.containerfull_notm}}
{: #ansible-iks-deploy}

<table>
   <thead>
    <th style="width:60px">Name</th>
    <th style="width:250px">Description</th>
    <th style="width:150px">Access</th>
  </thead>
  <tbody>
       <tr>
      <td><code>ansible-app-deploy-iks</code></td>
      <td>Use this Ansible playbook to deploy a sample `Node.js` applicaton on a {{site.data.keyword.containerfull}} cluster. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/ansible-app-deploy-iks/blob/master/README.md).</td>
      <td><img src="images/viewgithubrepo.png" usemap="#viewgithubimage_map">
<map name="viewgithubimage_map">
  <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/ansible-app-deploy-iks" target="_blank" coords="3,1,140,20"  shape="rect">
</map><br><br><img usemap="#deploybutton_map" src="images/autodeploy_button.png"><map name="deploybutton_map" alt="This image leads to create an action.">
  <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-app-deploy-iks&url=https://github.com/Cloud-Schematics/ansible-app-deploy-iks" target="_blank" coords="1,3,139,20"  shape="rect"></map></td>
 </tr>
 </tbody>
 </table>

## Deploy kubectl on a {{site.data.keyword.BluVirtServers}}
{: #ansible-kubectl}

<table>
   <thead>
    <th style="width:60px">Name</th>
    <th style="width:250px">Description</th>
    <th style="width:150px">Access</th>
  </thead>
  <tbody>
  </tr>
       <tr>
      <td><code>ansible-kubectl</code></td>
      <td>Use this Ansible playbook to deploy `kubectl` on {{site.data.keyword.vsi_is_short}} by using a role from Ansible Galaxy. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/ansible-kubectl/blob/master/README.md).</td>
      <td> <img src="images/viewgithubrepo.png" usemap="#viewgithubimage_map">
<map name="viewgithubimage_map">
  <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/ansible-kubectl" target="_blank" coords="3,1,140,20" shape="rect">
</map><br><br><img usemap="#deploybutton_map" src="images/autodeploy_button.png"><map name="deploybutton_map" alt="This image leads to create an action.">
  <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-kubectl&url=https://github.com/Cloud-Schematics/ansible-kubectl" target="_blank" coords="1,3,139,20"  shape="rect"></map></td>
 </tr>
  </tbody>
  </table>
