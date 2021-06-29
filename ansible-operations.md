---

copyright:
  years: 2017, 2021
lastupdated: "2021-06-29"

keywords: ansible playbook, automate ansible playbook, {{site.data.keyword.cloud_notm}}

subcollection: schematics

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:beta: .beta}
{:table: .aria-labeledby="caption"} 
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}

---

## Automate {{site.data.keyword.cloud_notm}} operations by using {{site.data.keyword.bpshort}} action
{: #ansible-how-it-works}
{: help}
{: support}


With {{site.data.keyword.bplong_notm}}, you can automate the provisioning, management, and operation of your {{site.data.keyword.cloud}} infrastructure, service, and application stack across cloud environments by leveraging open source technologies, such as [Terraform](https://www.terraform.io/){: external} and [Red Hat Ansible](https://www.ansible.com/){: external}.  
{: shortdesc}

Review how {{site.data.keyword.bplong_notm}} automates cloud operations for your {{site.data.keyword.cloud_notm}} resources with Ansible. 
{: shortdesc}

<img src="images/ansible_flow.png" alt="Automating cloud operations for {{site.data.keyword.cloud_notm}} resources" width="600" style="width: 600px; border-style: none"/>

1. **Create your Ansible playbook**: Create a YAML file and specify the set of instructions that you want to run against one or multiple {{site.data.keyword.cloud_notm}} resources. To make playbooks accessible in {{site.data.keyword.bpshort}}, you must store them in a GitHub or GitLab repository and create a {{site.data.keyword.bpshort}} action that references this repository. 
2. **Create and define your action**: Create a {{site.data.keyword.bpshort}} action that points to your Ansible playbook in GitHub or GitLab. Then, you specify the {{site.data.keyword.cloud_notm}} resource inventory where you want to run your playbook and any more environment or runtime variables that are required for the playbook. Your {{site.data.keyword.cloud_notm}} resource inventory can include resources that you provisioned with {{site.data.keyword.bpshort}} and Terraform, or resources that you manually created. 
3. **Launch the action and run cloud operations**: To run your Ansible playbook, you must launch the action that you created. By launching the action, {{site.data.keyword.bpshort}} securely connects to the {{site.data.keyword.cloud_notm}} resource that you specified and starts executing the tasks that you defined in your playbook. For example, you might install more software on a virtual server or perform cloud operations, such as reloading the virtual server instance.


