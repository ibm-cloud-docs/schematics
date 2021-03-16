---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-16"

keywords: getting started with ansible, ansible tutorial, schematics ansible how to, run playbooks with schematics

subcollection: schematics

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"} 
{:codeblock: .codeblock}
{:tip: .tip}
{:beta: .beta}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}


# Getting started with {{site.data.keyword.bplong_notm}} and Ansible 
{: #getting-started-ansible}

   The open beta release of Ansible support is now available in {{site.data.keyword.bplong_notm}} to IBM users. Contact your IBM Cloud Schematics Technical Offering Manager [Sai Vennam](mailto:svennam@us.ibm.com), if you are interested in getting early access to this beta offering. For more information, see [Beta limitations](/docs/schematics?topic=schematics-schematics-limitations#beta-limitations).
   {: beta}

Enable Infrastructure as Code (IaC) with {{site.data.keyword.bplong_notm}}, by running Ansible playbooks against your inventory of {{site.data.keyword.cloud}} resources. Use Ansible to install software packages, and application code on VSIs. Or use it to perform post provisioning configuration of {{site.data.keyword.cloud_notm}} resources, deployed by using {{site.data.keyword.bplong_notm}} and Terraform. 

## What is Ansible?
{: #about-ansible}

[Ansible ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ansible.com/) is a configuration management,and provisioning tool, similar to [Chef ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.chef.io/products/chef-infra/) and [Puppet ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://puppet.com/), and is designed to automate multitier app deployments and provisioning in the cloud. Written in Python, Ansible uses YAML syntax to describe automation tasks, which makes Ansible easy to learn and use. 

 ## How does Ansible work?
 {: #ansible-work}

Ansible does not use agents or a custom security infrastructure that must be present on a target machine to work properly. Instead, Ansible connects to compute hosts over the public network by using SSH keys and a bastion host to create a secure connection. The SSH keys to be used for VSI and bastion host access are configured on your virtual service instances when you order. You can choose from your own SSH keys and upload SSH key to your {{site.data.keyword.cloud_notm}} portal. For more information, on SSH key, refer to [SSH Keys](/docs/vpc?topic=vpc-ssh-keys).

Ansible models software packages, configuration, and services as resources on a managed host to ensure that the resource is in a specific state. To bring a resource to the required state, Ansible pushes modules to the managed host to run the required tasks. After the tasks are executed, the result is returned to the Ansible server and the module is removed from the managed host. You can use Ansible modules to execute a specific operation or group scripts and configurations in the Ansible playbook that you want to execute. Ansible modules are idempotent such that executing the same playbook or operation multiple times returns the same result as resources are changed only if required. 

- [A quick video about how Ansible works?](https://www.youtube.com/watch?v=fHO1X93e4WA){: external}

For more information, about Ansible, see:
- [Ansible documentation ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://docs.ansible.com)
- [Ansible introduction ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.tutorialspoint.com/ansible/ansible_introduction.htm)
- [Ansible2 tutorial ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://serversforhackers.com/c/an-ansible2-tutorial)

## How do Ansible and Schematics work together?
{: #ansible-in-schematics}

Ansible and Schematics are complementary solutions, each addressing a key area of application and IaC management. Schematics provides workspace, Terraform resources, module templates to manage the infrastructure, whereas Ansible helps you to provision use case, config management and deploy an app as a secured application.

The functions of the Schematics action in {{site.data.keyword.cloud_notm}} are:

 - Describes the input configuration for your Ansible playbook.
 - You can use the Schematics action definition to deploy multitier apps, set up firewall rules. 
 - You need to only specify the tasks that you want to run and let {{site.data.keyword.bpshort}} securely connect and complete the tasks on your {{site.data.keyword.cloud_notm}} resources.
 - Runs the playbook in a repeatable, reliable and consistent manner.

{{site.data.keyword.bpshort}} actions specifies:

 - How you point your action to the Ansible playbook in your GitHub repository?
 - It defines your resource inventory where you want to run your playbook.
 - It runs your Ansible playbook in {{site.data.keyword.bpshort}}.


## How do {{site.data.keyword.bpshort}} support Ansible Galaxy?
{: #ansible-galaxy}

Ansible Galaxy is a tool to retrieve the Ansible roles from the requirements file and invoke your Ansible playbook to setup the configured resources. This is used to streamline your automation tasks, even the fresh system administrator can start automating by using Ansible.

{{site.data.keyword.bplong_notm}} supports `/roles` to specify the requirements to process in through `requirements.yaml` or `requirements.yml` file. The requirements file uses the Ansible Galaxy repository to execute the process and invokes your Ansible playbook from the Git repository to execute the configured resources.

For more information, about the sample Git repository that installs `kubectl` on virtual machine by using Ansible Galaxy role, refer to [Ansible playbook by using Ansible Galaxy](https://github.com/Cloud-Schematics/ansible-kubectl). Following is the directory structure of the Git repository supporting Ansible Galaxy.
{: shortdesc}

```
   ├── kubectl.yaml
   ├── README.md
   ├── roles
      └── requirements.yaml or requirements.yml
```
{: screen}


## How do Schematics support Ansible collections?
{: #ansible-collections}

Ansible collections includes playbooks, roles, modules, and plug-ins. As modules move from the core Ansible repository into collections, the module documentation move the collections pages. For more information, about Ansible collections, refer to [Collection support](https://docs.ansible.com/ansible-tower/latest/html/userguide/projects.html#collections-support){: external}.

IBM Cloud Schematics supports Ansible collections by processing the requirements through `requirements.yaml` or `requirements.yml` file. The `requirements.yaml` file is stored in the `/collections` folder in your Git repository. It’s designed to be the hub for all of your automation tasks. It logs all of your jobs, and has a browseable REST API.  Provisioning callbacks provide great support for autoscaling topologies. Collections are stored in the Ansible root folder as `ansiblecollections`.
{: shortdesc}

The sample folder structure with the collections structure in the Git repository is:

```
   ├── site.yaml
   |——- roles
      └── requirements.yaml 
   ├── collections    
      └── requirements.yaml or requirements.yml       
   |── tasks                
   |── main.yml
```
{: screen}





## Creating and running Ansible playbooks for IBM Cloud
{: #ansible-playbook}

To create and run Ansible playbook by using {{site.data.keyword.bplong_notm}}, refer to [create and run Ansible playbook](/docs/schematics?topic=schematics-create-playbooks).


