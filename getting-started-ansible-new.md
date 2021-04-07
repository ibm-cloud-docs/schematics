---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-07"

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


# Getting started with configuration management in {{site.data.keyword.bplong_notm}}
{: #get-started-ansible}

Use one of the IBM-provided Ansible playbooks to start and stop a VPC Gen2 virtual server. 
{: shortdesc}

An Ansible playbook is a set of instructions that you can run on a single target host or a group of hosts. You create a {{site.data.keyword.bpshort}} action that points to your playbook and use the built-in Ansible capabilities in {{site.data.keyword.bplong_notm}} to run the instructions in your playbook. For more information about how {{site.data.keyword.bpshort}} runs your Ansible playbooks, see [Configuration management with {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-about-schematics#how-to-actions). 

Ansible support in {{site.data.keyword.bplong_notm}} is currently in beta and subject to change. Do not use {{site.data.keyword.bpshort}} actions for production workloads. 
{: beta}

## Before you begin
{: #ansible-prereq}

Before you can use this Ansible playbook, you must complete the following tasks:

- Make sure that you have the permissions to [create a {{site.data.keyword.bpshort}} action](/docs/schematics?topic=schematics-access#access-roles). 
- Create a VPC for Generation 2 compute infrastructure and a virtual server instance in that VPC. For more information, see [Getting started with Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started). Note the private or public IP address of your virtual server instance. 

## Starting and stopping a VPC Gen2 Virtual Server with {{site.data.keyword.bpshort}}
{: #ansible-vsi}

1. From the [{{site.data.keyword.bpshort}} actions](https://cloud.ibm.com/schematics/actions){: external} page, click **Create action**. 
2. Enter a name for your action, for example, `Stop_VSIaction`, resource group, and the region where you want to create the action. Then, click **Create**. 
3. In the **Define your Action action** section, enter `https://github.com/Cloud-Schematics/ansible-is-instance-actions` in the **GitHub or GitLab repository URL** field. 
4. Click **Retrieve playbooks**. 
5. Select the **stop-vsi-playbook.yaml** playbook.
6. Expand the **Advanced options**. 
7. In the **Define your variables** section, enter `instance_ip` as the **key** and the public or private IP address of your VPC Gen2 virtual server as the **value**. 
8. Click **Next**. 
9. Click **Check action** to verify your action details. The **Jobs** page opens automatically and you can view the results of this check by looking at the logs. 
10. Click **Run action** to stop the virtual server instance. You can monitor the progress of this action by reviewing the logs on the **Jobs** page. 
11. Verify that your virtual server instance stopped. 
    1. From the [Virtual server instances for VPC dashboard](https://cloud.ibm.com/vpc-ext/compute/vs){: external}, find your virtual server instance. 
    2. Verify that your instance shows a `Stopped` status. 
12. Optional: Repeat the steps in this getting started tutorial to create another {{site.data.keyword.bpshort}} action, and select the **start-vsi-playbook.yaml** Ansible playbook to start your virtual server instance again. 

Congratulations! You used the built-in Ansible capabilities of {{site.data.keyword.bpshort}} to start and stop a VPC Gen2 virtual server instance. 

## What's next? 
{: ansible-whats-next}

Now that you ran your first operation on an {{site.data.keyword.cloud_notm}} resource, you can explore the following options:

- Learn how to [create your own Ansible playbooks](/docs/schematics?topic=schematics-create-playbooks).
- Explore other [IBM-provided playbooks](https://github.com/Cloud-Schematics){: external}.
- Set up the {{site.data.keyword.bpshort}} [CLI](/docs/schematics?topic=schematics-setup-cli) or [API](/docs/schematics?topic=schematics-setup-api) to start automating the configuration of {{site.data.keyword.cloud_notm}} resources.
