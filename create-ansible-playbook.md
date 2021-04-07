---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-07"

keywords: schematics ansible, schematics action, create schematics actions, run ansible playbooks

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

# Creating Ansible playbooks 
{: #create-playbooks}

An [Ansible playbook](https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook){: external} is a set of instructions or automation tasks that you can configure to run on a single target, or group of target hosts also referred to as inventory. It includes tasks, roles, policies, or steps to deploy your resources in the target hosts. You can run your automation tasks in the order in which you want to configure and perform managed operations on the {{site.data.keyword.cloud}} resource.

For example, you can include instructions to install a software on a virtual server, or to perform managed operations, such as reload or take down a virtual server instance.

Ansible playbooks must be stored in a GitHub or GitLab repository so that, you can run them in {{site.data.keyword.bpshort}}. For more information, about how to write your playbook by using Ansible, refer to [Writing your playbook](https://www.ansible.com/blog/getting-started-writing-your-first-playbook){: external}.
{: shortdesc}

## Planning your Ansible playbook
{: #plan-ansible-playbook}

You need to plan for these check list to create a playbook:
- The target or group of target hosts where you want to run through the Ansible playbook. You can either provide a static list of IP addresses for the target hosts or dynamically fetch the IP addresses from an existing {{site.data.keyword.bpshort}} workspace.
- Plan the tasks to provision infrastructure, deploy applications you want to perform regularly through roles. For more information, about roles and its usage, see [referencing Ansible roles in a playbook](#schematics-roles).
- Plan for a comprehensive package of automation by using multiple playbooks, and roles by using Ansible collections. For more information, about roles and its usage, see [referencing Ansible collections in a playbook](#schematics-collections).

## Creating your Ansible playbook
{: #create-playbook}

Follow these steps to create an Ansible playbook.

Create a YAML file that contains all the target host, roles, tasks, files, policies,   and steps you want to configure and deploy your resources. For more information, about YAML syntax, refer to [YAML Syntax](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html){: external}.  Refer to [a sample YAML file](https://github.com/Cloud-Schematics/ansible-app-deploy-iks/blob/master/site.yml){: external} that describes how to use Ansible playbook to deploy the Hackathon starter web application.

   You can configure the tasks in `requirements.yaml` file and store in the [roles](#schematics-roles) folder for your playbook execution. You can also configure all of your automation and autoscaling topologies in the `requirements.yaml` file and store in the [collections](#schematics-collections) folder for your playbook execution.
   {: note}

Store the YAML files in your Git repository. For more information, about the location to store the YAML files, review this [sample file structure](https://github.com/Cloud-Schematics/ansible-app-deploy-iks){: external} that describes the how to structure the directories and files to deploy the application.



## Referencing Ansible roles in your playbook
{: #schematics-roles}

An [Ansible roles](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html){: external} is a set of instructions or automation tasks that are used to configure a host or group of hosts, deploy software or perform managed operations. Ansible roles are defined by using `YAML` files with a predefined directory structure.

An [Ansible Galaxy](https://galaxy.ansible.com/){: external} is a repository of reusable Ansible roles that can be used in your Ansible playbook. The reusable Ansible roles are dynamically retrieved from Ansible Galaxy by using the requirements files before invoking your Ansible playbook to setup the configured resources. This is used to streamline your automation tasks so that even new users or administrator can start automating tasks by using Ansible.

{{site.data.keyword.bplong_notm}} supports `/roles` to specify the requirements to process in through `requirements.yaml` or `requirements.yml` file. The requirements file uses the Ansible Galaxy repository to execute the process and invokes your Ansible playbook from the Git repository to execute the configured resources.

The sample folder structure with the `roles` in the Git repository.

```
   ├── kubectl.yaml
   ├── README.md
   ├── roles
      └── requirements.yaml or requirements.yml
```
{: screen}

The roles directory can have a sub directory such as **/roles/web/** or **/roles/db/** describing the tasks in the 'main.yaml' file.
{: note}

**Sample**

For more information, about the sample Ansible playbook that uses a role from Ansible Galaxy to install `kubectl` on a virtual machine, see [ansible-kubectl](https://github.com/Cloud-Schematics/ansible-kubectl){: external}.

The sample `requirements.yaml` file for installing `kubectl` on virtual machine from Ansible Galaxy role `andrewrothstein.kubectl`. 

```
---
roles:
  - name: andrewrothstein.kubectl
```
{: screen}

The sample `kubectl.yaml` playbook to start the role from your Git repository. 

```
---
- hosts: all
  roles:
    - role: andrewrothstein.kubectl
```
{: screen}


## Referencing Ansible collections in your playbook
{: #schematics-collections}

An [Ansible collections](https://www.ansible.com/blog/getting-started-with-ansible-collections){: external} include playbooks, roles, modules, and plug-ins. As modules move from the core Ansible repository into collections, the module documentation move the collections pages. 

An [Ansible Galaxy](https://galaxy.ansible.com/){: external} hosts many reusable Ansible Collection that can be used in your Ansible playbook.

{{site.data.keyword.bplong_notm}} supports Ansible collections by processing the requirements file. It’s designed to be a collective package for all of your automation task related to playbooks, and roles. The requirements file is stored in the `/collections` folder of your Git repository. Collections are stored in the Ansible root folder as `ansiblecollections`.
{: shortdesc}

The sample folder structure with the `collections` in the Git repository.

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
