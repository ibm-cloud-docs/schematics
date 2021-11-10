---

copyright:
  years: 2017, 2021
lastupdated: "2021-11-10"

keywords: schematics ansible, schematics action, create schematics actions, run ansible playbooks

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

An [Ansible role](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html){: external} can be used to separate one big Ansible playbook into smaller reusable pieces called roles. A role defines a set of tasks that you want to run on your target hosts. To run these tasks on your hosts, you must reference the role in your Ansible playbook. 

You can [create your own roles](#main-file) or [use existing roles from Ansible Galaxy](#requirements-file). 

### Creating your own roles in Ansible 
{: #main-file}

To streamline your Ansible playbook, you can decide to separate out playbook tasks by creating roles and referencing them in your playbook.  
{: shortdesc}

1. Identify the tasks in your playbook that you want to reuse across multiple hosts. For example, you can group tasks that you want to run on all of your hosts, and tasks that you want to run on your web servers and your databases. Each group of tasks can become its own role. 

2. Create the Ansible role structure in your GitHub repository. Roles must be stored in a `roles` directory relative to your Ansible playbook. The roles directory can have a subdirectory such as  **/roles/db/** describing the tasks in the `main.yml` file.
    ```
    ├── roles
        └── db
            └── tasks
                └── main.yml
    ├── playbook.yaml
    ├── README.md
    ```
    {: screen}

3. Add the tasks that you want to run to a `main.yml` file. In the following example, you separate out the task to download the MySQL community repo from your main playbook and put it into a `main.yml` file. 
    ```
    - name: Download MySQL Community Repo
        get_url:
        url: https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
        dest: /tmp
    ```
    {: codeblock}

4. Reference the role in your Ansible playbook.
   ```
    - name: deploy MySQL and configure the databases
      hosts: all
      remote_user: root

      roles:
        - db
    ```
    {: codeblock}

For more information about other files and conditions that you can add to your role, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/user_guide/playbooks_reuse_roles.html#role-directory-structure){: external}

### Installing roles from Ansible Galaxy
{: #requirements-file}

You can choose to use existing roles from [Ansible Galaxy](https://galaxy.ansible.com/){: external} in your playbook. Ansible Galaxy offers pre-packaged units of work from the Ansible community that are made available as roles and collections.

1. Browse the [Ansible Galaxy](https://galaxy.ansible.com/){: external} repository to find the roles that you want.
2. Create a `requirements.yml` file where you specify all the roles that you need. For an overview of how to reference Ansible Galaxy roles, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#install-multiple-collections-with-a-requirements-file){: external}. In the following example, you want to use the role `andrewrothstein.kubectl` from Ansible Galaxy. 
    ```
    ---
    roles:
      - name: andrewrothstein.kubectl
    ```
    {: codeblock}

3. Add a `roles` folder to your GitHub repository that is relative to the playbook, and store the `requirements.yml` file in this folder as shown in this example. 
    ```
        ├── roles
            └── requirements.yml
        ├── playbook.yaml
        ├── README.md
    ```
    {: screen}

4. Reference the role in your Ansible playbook. In this example, the role with the name `andrewrothstein.kubectl` is used.
    ```
    ---
    - hosts: all
      roles:
        - role: andrewrothstein.kubectl
    ```
    {: codeblock}

Want to see an example? See [this IBM-provided Ansible playbook](https://github.com/Cloud-Schematics/ansible-kubectl){: external}
{: tip}


## Referencing Ansible collections in your playbook
{: #schematics-collections}

Ansible collections group different reusable Ansible resources, such as playbooks, modules, and roles so that you can install them and use them in your playbook. Collections are stored in the [Ansible Galaxy](https://galaxy.ansible.com/){: external} repository.

Similar to [Ansible roles](#schematics-roles), collections require a specific folder structure in your GitHub repository. 

1. Browse [Ansible Galaxy](https://galaxy.ansible.com/){: external} to find the collection that you want to use in your playbook.
2. Create a `requirements.yml` file where you specify the collections that you want to install from Ansible Galaxy. For more information about how to structure this file, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#installing-collections){: external}. The following example uses the `community.kubernetes` collection.
    ```
    collections:
      - name: community.kubernetes
        version: 0.9.0
    ```
    {: codeblock}

3. Add a `collections` folder to your GitHub repository that is relative to your playbook, and store the `requirements.yml` file in this folder as shown in this example. 
    ```
    ├── collections
            └── requirements.yml
    ├── playbook.yaml
    ├── README.md
    ```
    {: screen}

4. Reference a resource from your collection in your playbook. For more information, see the [Ansible documentation](https://docs.ansible.com/ansible/2.9/user_guide/collections_using.html#using-collections-in-a-playbook){: external}




