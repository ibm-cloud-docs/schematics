---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-13"

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




# Sample Ansible playbook templates for {{site.data.keyword.bpshort}} actions
{: #sample_actiontemplates}

Try out one of the IBM-provided Ansible playbooks to perform cloud operations on target hosts or to get started with {{site.data.keyword.bplong_notm}} actions.
{: shortdesc}

Use the links on this page as follows: 
- `View GitHub repo`: Click on this link to open the Git repository where the template is stored. You can review the file structure, the Ansible playbook instructions, and the `README` file that contains the steps to use the template in {{site.data.keyword.bpshort}}.
- `Deploy to IBM Cloud`: This link takes you to the **Create an action** page with the **GitHub repository URL** and the **Action name** pre-populated.  


## Running cloud operations on {{site.data.keyword.vsi_is_short}}
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
        <td>Use this Ansible playbook to perform day 2 operations such as start, stop, and reboot for {{site.data.keyword.vsi_is_full}}. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/ansible-is-instance-actions/blob/master/README.md).</td>
        <td> <img src="images/viewgithubrepo.png" alt="View GitHub repository" usemap="#viewgithubimage_map1">
<map name="viewgithubimage_map1">
    <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/ansible-is-instance-actions" target="_blank" coords="3,1,140,20" shape="rect">
</map><br><br><img usemap="#deploybutton_map1" alt="Auto deployment button" src="images/autodeploy_button.png"><map name="deploybutton_map1" alt="This image leads to create an action.">
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
        <td>Use this Ansible playbook to deploy the LAMP stack components on {{site.data.keyword.vsi_is_short}} by following a simple deployment architecture. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/lamp-simple/blob/master/README.md).</td>
        <td> <img src="images/viewgithubrepo.png"  alt="View GitHub repository" usemap="#viewgithubimage_map2">
<map name="viewgithubimage_map2">
    <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/lamp-simple" target="_blank" coords="3,1,140,20"  shape="rect">
</map><br><br><img usemap="#deploybutton_map2" alt="Auto deployment button"  src="images/autodeploy_button.png"><map name="deploybutton_map2" alt="This image leads to create an action.">
    <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=lamp-simple&url=https://github.com/Cloud-Schematics/lamp-simple" target="_blank" coords="1,3,139,20"  shape="rect"></map></td>
    </tr>
    </tbody>
    </table>


## Configuring {{site.data.keyword.databases-for-postgresql_full_notm}} with WAL2JSON plug-in
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
        <td>Use this Ansible playbook to configure the WAL2JSON plug-in on your {{site.data.keyword.databases-for-postgresql_full_notm}} service instance. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/ansible-icd-postgres-actions/blob/master/README.md).</td>
        <td> <img src="images/viewgithubrepo.png"  alt="View GitHub repository" usemap="#viewgithubimage_map3">
<map name="viewgithubimage_map3">
    <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/ansible-icd-postgres-actions" target="_blank" coords="3,1,140,20"  shape="rect">
</map><br><br><img usemap="#deploybutton_map3" alt="Auto deployment button" src="images/autodeploy_button.png"><map name="deploybutton_map3" alt="This image leads to create an action.">
    <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-icd-postgres-actions&url=https://github.com/Cloud-Schematics/ansible-icd-postgres-actions" target="_blank" coords="1,3,139,20" shape="rect"></map></td>
    </tr>
    </tbody>
    </table>


## Automating app deployments in {{site.data.keyword.containerfull_notm}}
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
        <td>Use this Ansible playbook to deploy Hackathon Starter on an {{site.data.keyword.containerfull}} cluster. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/ansible-app-deploy-iks/blob/master/README.md).</td>
        <td><img src="images/viewgithubrepo.png"  alt="View GitHub repository" usemap="#viewgithubimage_map4">
<map name="viewgithubimage_map4">
    <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/ansible-app-deploy-iks" target="_blank" coords="3,1,140,20"  shape="rect">
</map><br><br><img usemap="#deploybutton_map4" alt="Auto deployment button" src="images/autodeploy_button.png"><map name="deploybutton_map4" alt="This image leads to create an action.">
    <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-app-deploy-iks&url=https://github.com/Cloud-Schematics/ansible-app-deploy-iks" target="_blank" coords="1,3,139,20" shape="rect"></map></td>
    </tr>
    </tbody>
    </table>

## Installing `kubectl` on {{site.data.keyword.vsi_is_short}}
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
        <td>Use this Ansible playbook to install the Kubernetes CLI <code>kubectl</code> on {{site.data.keyword.vsi_is_short}} by using a role from Ansible Galaxy. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/ansible-kubectl/blob/master/README.md).</td>
        <td> <img src="images/viewgithubrepo.png"  alt="View GitHub repository" usemap="#viewgithubimage_map5">
<map name="viewgithubimage_map5">
    <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/ansible-kubectl" target="_blank" coords="3,1,140,20" shape="rect">
</map><br><br><img usemap="#deploybutton_map5" alt="Auto deployment button" src="images/autodeploy_button.png"><map name="deploybutton_map5" alt="This image leads to create an action.">
    <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-kubectl&url=https://github.com/Cloud-Schematics/ansible-kubectl" target="_blank" coords="1,3,139,20"  shape="rect"></map></td>
    </tr>
    </tbody>
    </table>


