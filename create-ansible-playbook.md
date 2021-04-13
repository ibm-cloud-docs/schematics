---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-13"

keywords: schematics ansible, schematics action, create schematics actions, run ansible playbooks

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


# Creating Ansible playbooks 
{: #create-playbooks}

An [Ansible playbook](https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook){: external} is a set of instructions or automation tasks that you can run on a single target host or a group of target hosts. These hosts are also referred to as the resource inventory. 
{: shortdesc}

With Ansible playbooks, you can run operations on cloud resources, such as starting or stopping a virtual server, install software packages, deploy apps, or even provision cloud resources and configure them to your needs. A playbook is written in YAML format and consists of multiple plays that each define the tasks that you want to run on your target hosts. Common automation tasks can be grouped to an Ansible module and made available so that they can be reused in other playbooks. You can also group multiple playbooks to an Ansible role. 

## Preparing your Ansible playbook to run in {{site.data.keyword.bpshort}} 
{: #plan-ansible-playbook}

To prepare your Ansible playbook for {{site.data.keyword.bpshort}}, review the following considerations. 
{: shortdesc}

- Your Ansible playbook must be stored in a GitHub or GitLab repository. 
- You are limited in how you can specify the target hosts for your Ansible resource inventory. For more information, see [Creating resource inventories for {{site.data.keyword.bpshort}} actions](/docs/schematics?topic=schematics-inventories-setup). 
- All playbooks must be compatible to run with an Ansible version that is supported in {{site.data.keyword.bpshort}}. To find supported versions, run `ibmcloud schematics version`. 
- To try out the Ansible capabilities in {{site.data.keyword.bpshort}}, you can try out one of the [IBM-provided Ansible playbooks](/docs/schematics?topic=schematics-sample_actiontemplates). 

## Creating an Ansible playbook
{: #create-playbook}

Follow these general steps to create your Ansible playbook. For detailed information about how to structure your playbook, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/user_guide/playbooks_intro.html){: external} or this [blog](https://www.ansible.com/blog/getting-started-writing-your-first-playbook){: external}. 
{: shortdesc}

Want to use existing Ansible playbooks to get started? Try out one of the [IBM-provided Ansible playbooks](/docs/schematics?topic=schematics-sample_actiontemplates) or browse existing Ansible collections and roles in [Ansible Galaxy](https://galaxy.ansible.com/){: external}
{: tip}

1. [Create your resource inventory where you want to run your Ansible playbook](/docs/schematics?topic=schematics-inventories-setup). You can also use the built-in Terraform capabilities in {{site.data.keyword.bpshort}} to provision your target hosts. For more information, see [Infrastructure deployment with {{site.data.keyword.bpshort}} workspaces](/docs/schematics?topic=schematics-about-schematics#how-to-workspaces). 

2. Create your Ansible playbook. Use one of the [IBM-provided playbooks](/docs/schematics?topic=schematics-sample_actiontemplates) to get started or browse [Ansible Galaxy](https://galaxy.ansible.com/){: external} to find existing roles and collections. You can then reference these [roles](#schematics-roles) and [collections](#schematics-collections) in your playbook.

3. Create a repository in GitHub or GitLab, and build the Ansible playbook directory and file structure. Depending on whether or not you use Ansible roles and collections, this directory structure might vary. To find a sample structure, refer to this [sample playbook](https://github.com/Cloud-Schematics/ansible-app-deploy-iks/blob/master/site.yml){: external}. 

4. Upload your Ansible playbook, modules, roles, and collections to your GitHub repository. 
5. [Create a {{site.data.keyword.bpshort}} action](/docs/schematics?topic=schematics-action-setup#create-action).


## Referencing Ansible roles in your playbook
{: #schematics-roles}

An [Ansible role](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html){: external} can be used to segregate one big Ansible playbook into smaller reusable pieces called roles. A role defines a set of tasks that you want to run on your target hosts. To run these tasks on your hosts, you must reference the role in your Ansible playbook. 

Roles can be created by using two main paths: 
- `main.yml`
- `requirements.yml`

### Specifying a role by using a `main.yml` file
{: #main-file}

Roles must be stored in a `roles` directory relative to your Ansible playbook. You can create subdirectories to specify different roles. The tasks that you want to run for each role must be stored in a `main.yml` file inside a `tasks` directory as shown in this example.

**Sample role file structure**: 

```
  ├── roles
      └── db
          └── tasks
              └── main.yml
  ├── playbook.yaml
  ├── README.md
```
{: codeblock}

**Sample main.yml file**: 
```
- name: Download MySQL Community Repo
    get_url:
      url: https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
      dest: /tmp
    tags: db
```
{: codeblock}

**Reference the role in your Ansible playbook**: 

```
- name: deploy MySQL and configure the databases
  hosts: all
  remote_user: root

  roles:
    - db
```
{: codeblock}

For more information about other files and conditions that you can add to your role, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html#role-directory-structure){: external}

### Using `requirements.yml` files
{: #requirements-file}


You can create your own roles by separating out tasks and put them into a `main.yml` file inside a `roles` directory. You can also browse existing roles in the [Ansible Galaxy](https://galaxy.ansible.com/){: external} repository.



as seen in the following example. For more information about the role file structure, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html#id2){: external}

```
   ├── roles
      └── requirements.yaml or requirements.yml
   ├── playbook.yaml
   ├── README.md
```
{: codeblock}


The reusable Ansible roles are dynamically retrieved from Ansible Galaxy by using a `requirements.yml` file that must be present in a `roles` directory in your GitHub repository. 

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
