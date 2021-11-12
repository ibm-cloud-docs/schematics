---

copyright:
  years: 2017, 2021
lastupdated: "2021-11-12"

keywords:  ansible, ansible playbook

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Creating Ansible playbooks 
{: #create-playbooks}

Ansible playbooks helps to run operations on cloud resources, such as starting or stopping a virtual server, install software packages, deploy apps, or even provision cloud resources and configure them to your needs. A playbook is written in YAML format and consists of multiple plays that each define the tasks that you want to run on your target hosts. Common automation tasks can be grouped to an Ansible module and made available so that they can be reused in other playbooks. You can also group multiple playbooks to an Ansible role.
{: shortdesc}

## Preparing your Ansible playbook to run in {{site.data.keyword.bpshort}} 
{: #plan-ansible-playbook}

To prepare your Ansible playbook for {{site.data.keyword.bpshort}}, review the following considerations. 
{: shortdesc}

- Your Ansible playbook must be stored in a GitHub or GitLab repository. 
- You are limited in how you can specify the target hosts for your Ansible resource inventory. For more information, see [Creating resource inventories for {{site.data.keyword.bpshort}} actions](/docs/schematics?topic=schematics-inventories-setup). 
- All playbooks must be compatible to run with an Ansible version that is supported in {{site.data.keyword.bpshort}}. To find supported versions run [`ibmcloud schematics version`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-version) command. 
- Optionally, to explore Ansible playbook capabilities in {{site.data.keyword.bpshort}}. You can try to use one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook){: external}. 