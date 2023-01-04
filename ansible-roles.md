---

copyright:
  years: 2017, 2023
lastupdated: "2023-01-04"

keywords: schematics ansible roles, schematics action, create schematics galaxy, ansible playbooks

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Creating Ansible roles and galaxy
{: #ansible-roles-galaxy}
 
An [Ansible role](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html){: external} can be used to separate one significant Ansible playbook into smaller reusable pieces called roles. A role defines a set of tasks that you want to run on your target hosts. To run these tasks on your hosts, you must reference the role in your Ansible playbook. 

[Ansible Galaxy](https://galaxy.ansible.com/docs/?extIdCarryOver=true&sc_cid=701f2000001OH6uAAG){: external} is a repository for Ansible roles that are available to drop directly into your Playbooks to streamline your automation projects. A new sysadmin might start automating with Ansible in a matter of a few hours.
{: shortdesc}

You can [create your own roles](/docs/schematics?topic=schematics-ansible-roles-galaxy#main-file) or [use existing roles from Ansible Galaxy](/docs/schematics?topic=schematics-ansible-roles-galaxy#requirements-file). 

## Creating your own roles in Ansible 
{: #main-file}

To streamline your Ansible playbook, you can decide to separate out playbook tasks by creating roles and referencing them in your playbook. Use it to automatically load related variables, files, tasks, handlers, and other Ansible artifacts based on a known file structure.
{: shortdesc}

1. Identify the tasks in your playbook that you want to reuse across multiple hosts. For example, you can group tasks that you want to run on all your hosts, and tasks that you want to run on your web servers and your databases. Each group of tasks can become its own role. 

2. Create the Ansible role structure in your GitHub repository. Roles must be stored in a `roles` directory relative to your Ansible playbook. The roles directory can have a subdirectory such as `/roles/db/` describing the tasks in the `main.yml` file.
    ```text
    ├── roles
        └── db
            └── tasks
                └── main.yml
    ├── playbook.yaml
    ├── README.md
    ```
    {: screen}

3. Add the tasks that you want to run to a `main.yml` file. In the following example, you separate out the task to download the MySQL community repo from your main playbook and put it into a `main.yml` file. 
    ```yaml
    - name: Download MySQL Community Repo
        get_url:
        url: https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
        dest: /tmp
    ```
    {: codeblock}

4. Reference the role in your Ansible playbook.
   ```yaml
    - name: deploy MySQL and configure the databases
      hosts: all
      remote_user: root

      roles:
        - db
      tasks:
        - shell: "hostname"
    ```
    {: codeblock}

For more information about other files and conditions that you can add to your role, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html#role-directory-structure){: external}.

## Installing roles from Ansible Galaxy
{: #requirements-file}

You can choose to use existing roles from [Ansible Galaxy](https://galaxy.ansible.com/){: external} in your playbook. Ansible Galaxy offers pre-packaged units of work from the Ansible community that are made available as roles and collections.

1. Browse the [Ansible Galaxy](https://galaxy.ansible.com/){: external} repository to find the roles that you want.
2. Create a `requirements.yml` file where you specify all the roles that you need. For an overview of how to reference Ansible Galaxy roles, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#install-multiple-collections-with-a-requirements-file){: external}. In the following example, you want to use the role `andrewrothstein.kubectl` from Ansible Galaxy. 
    ```yaml
    ---
    roles:
      - name: andrewrothstein.kubectl
    ```
    {: codeblock}

3. Add a `roles` folder to your GitHub repository that is relative to the playbook, and store the `requirements.yml` file in this folder as shown in this example.

    ```text
        ├── roles
            └── requirements.yml
        ├── playbook.yaml
        ├── README.md
    ```
    {: screen}

4. Reference the role in your Ansible playbook. In this example, the role with the name `andrewrothstein.kubectl` is used.
    
    ```yaml
    - hosts: all
      roles:
        - role: andrewrothstein.kubectl
    ```
    {: codeblock}

For more information about Ansible playbook examples, see that [IBM provided Ansible playbook](https://github.com/Cloud-Schematics/ansible-kubectl){: external}
{: tip}

