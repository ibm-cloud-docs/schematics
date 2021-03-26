---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-26"

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




# Sample action templates to auto deploy to {{site.data.keyword.bplong_notm}}
{: #sample_actiontemplates}


The deploy to {{site.data.keyword.cloud}} URL is an efficient way for you to enable users to deploy solutions on {{site.data.keyword.cloud_notm}} from a public Git repository sample configuration. The URL requires minimal configuration and you can insert it anywhere in your documentation that supports markup. When the user clicks the hyper link, they are taken directly to the {{site.data.keyword.bpshort}} **Create an action** page and only need to click the **Create** button for action creation in {{site.data.keyword.bpshort}}.
{: shortdesc}

When you click the `Deploy to {{site.data.keyword.cloud_notm}}`, these steps occur.

  1. Your {{site.data.keyword.cloud_notm}} account is accessed. If the user does not have an active {{site.data.keyword.cloud_notm}} account, you must create a trial account or a real account.

  2. You can edit the **Action name**, **Repository URL**, **Description**, **Tags**, **Resource group**, and **location**. Make sure, you select the correct location, you cannot update the location and region once an action is created.

  3. The auto deploy link sets the create action in the {{site.data.keyword.bplong_notm}}. 

  4. You can use `README` file from the template to configure and run the {{site.data.keyword.bpshort}} action.


## VM operations on VPC template
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
      <td>Create an [Ansible virtual server instance](https://github.com/Cloud-Schematics/ansible-is-instance-actions) application that illustrates to perform start, stop, and reboot operations for {{site.data.keyword.cloud_notm}} VSI on VPC. To configure and run the {{site.data.keyword.bpshort}} action, see [readme file](https://github.com/Cloud-Schematics/ansible-is-instance-actions/blob/master/README.md).</td>
      <td><a href="https://github.com/Cloud-Schematics/ansible-is-instance-actions"><img src="/images/viewgithub.png"></a><br><br><a href="https://cloud.ibm.com/schematics/actions/create?name=ansible-is-instance-actions&url=https://github.com/Cloud-Schematics/ansible-is-instance-actions"><img src="/images/deploytoschematics.png"></a></td>
 </tr>
 </tbody>
 </table>

## LAMP stack template
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
      <td>Create a [LAMP stack](https://github.com/Cloud-Schematics/lamp-simple) application that illustrates to deploy LAMP stack components on {{site.data.keyword.cloud_notm}} VSI by using {{site.data.keyword.bplong_notm}} actions through user interface. To configure and run the {{site.data.keyword.bpshort}} action, see [readme file](https://github.com/Cloud-Schematics/lamp-simple/blob/master/README.md).</td>
      <td><a href="https://github.com/Cloud-Schematics/lamp-simple"><img src="/images/viewgithub.png"></a><br><br><a href="https://cloud.ibm.com/schematics/actions/create?name=lamp-simple&url=https://github.com/Cloud-Schematics/lamp-simple"><img src="/images/deploytoschematics.png"></a></td>
 </tr>
 </tbody>
 </table>

## {{site.data.keyword.databases-for-postgresql_full_notm}} template
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
      <td>Create an [Ansible {{site.data.keyword.cloud_notm}} database postgres actions](https://github.com/Cloud-Schematics/ansible-icd-postgres-actions) that illustrates to perform PostgreSQL database operations by using {{site.data.keyword.bplong_notm}} actions, Ansible playbook, and roles. To configure and run the {{site.data.keyword.bpshort}} action, see [readme file](https://github.com/Cloud-Schematics/ansible-icd-postgres-actions/blob/master/README.md).</td>
      <td><a href="https://github.com/Cloud-Schematics/lamp-simple"><img src="/images/viewgithub.png"></a><br><br><a href="https://cloud.ibm.com/schematics/actions/create?name=lamp-simple&url=https://github.com/Cloud-Schematics/lamp-simple"><img src="/images/deploytoschematics.png"></a></td>
 </tr>
 </tbody>
 </table>

## {{site.data.keyword.containerfull_notm}} deployment template
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
      <td>Create an [sample IKS](https://github.com/Cloud-Schematics/ansible-app-deploy-iks) application deployment that illustrates to create `IKS Cluster` by using Ansible playbook, and roles. To configure and run the {{site.data.keyword.bpshort}} action, see [readme file](https://github.com/Cloud-Schematics/ansible-app-deploy-iks/blob/master/README.md).</td>
      <td><a href="https://github.com/Cloud-Schematics/ansible-app-deploy-iks"><img src="/images/viewgithub.png"></a><br><br><a href="https://cloud.ibm.com/schematics/actions/create?name=ansible-app-deploy-iks&url=https://github.com/Cloud-Schematics/ansible-app-deploy-iks"><img src="/images/deploytoschematics.png"></a></td>
 </tr>
 </tbody>
 </table>

## {{site.data.keyword.containerfull_notm}} by using Ansible Galaxy template
{: #ansible-galaxy-iks-deploy}

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
      <td>Create an Ansible playbook to install [kubectl on virtual machine](https://github.com/Cloud-Schematics/ansible-kubectl) application. To configure and run the {{site.data.keyword.bpshort}} action, see [readme file](https://github.com/Cloud-Schematics/ansible-kubectl/blob/master/README.md).</td>
      <td><a href="https://github.com/Cloud-Schematics/ansible-app-deploy-iks"><img src="/images/viewgithub.png"></a><br><br><a href="https://cloud.ibm.com/schematics/actions/create?name=ansible-kubectl&url=https://github.com/Cloud-Schematics/ansible-kubectl"><img src="/images/deploytoschematics.png"></a></td>
 </tr>
  </tbody>
  </table>