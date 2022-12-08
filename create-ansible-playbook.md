---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-07"

keywords: schematics ansible, schematics action, create schematics actions, run ansible playbooks

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Creating an Ansible playbook
{: #create-playbook}
 
Follow these [prerequisites](#plan-ansible-playbook) and general steps to create your Ansible playbook. For detailed information about how to structure your playbook, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html){: external} or this [blog](https://www.ansible.com/blog/getting-started-writing-your-first-playbook){: external}. 
{: shortdesc}

Want to use existing Ansible playbooks to get started? Try out one of the [IBM-provided Ansible playbooks](/docs/schematics?topic=schematics-sample_actiontemplates) or browse existing Ansible collections and roles in [Ansible Galaxy](https://galaxy.ansible.com/){: external}
{: tip}

1. [Create your resource inventory where you want to run your Ansible playbook](/docs/schematics?topic=schematics-inventories-setup). You can also use the built-in Terraform capabilities in {{site.data.keyword.bpshort}} to provision your target hosts. For more information, see [Infrastructure deployment with {{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-how-it-works#how-to-workspaces). 

2. Create your Ansible playbook. Use one of the [IBM-provided playbooks](/docs/schematics?topic=schematics-sample_actiontemplates) to get started or browse [Ansible Galaxy](https://galaxy.ansible.com/){: external} to find existing roles and collections. You can then reference these [roles](/docs/schematics?topic=schematics-ansible-roles-galaxy#main-file) and [collections](#schematics-collections) in your playbook.

3. Create a repository in GitHub or GitLab, and build the Ansible playbook directory and file structure. Depending on whether you use Ansible roles and collections, this directory structure might vary. To find a sample structure, see this [sample playbook](https://github.com/Cloud-Schematics/ansible-app-deploy-iks/blob/master/site.yml){: external}. 

4. Upload your Ansible playbook, modules, roles, and collections to your GitHub repository. 
5. [Create a {{site.data.keyword.bpshort}} Actions](/docs/schematics?topic=schematics-action-working#create-action).

## Referencing Ansible collections in your playbook
{: #schematics-collections}

Ansible collections group different reusable Ansible resources, such as playbooks, modules, and roles so that you can install them and use them in your playbook. Collections are stored in the [Ansible Galaxy](https://galaxy.ansible.com/){: external} repository.

Similar to [Ansible roles](/docs/schematics?topic=schematics-ansible-roles-galaxy#main-file), collections require a specific folder structure in your GitHub repository. 

1. Browse [Ansible Galaxy](https://galaxy.ansible.com/){: external} to find the collection that you want to use in your playbook.
2. Create a `requirements.yml` file where you specify the collections that you want to install from Ansible Galaxy. For more information about how to structure this file, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#installing-collections){: external}. The following example uses the `community.kubernetes` collection.
    ```yaml
    collections:
      - name: community.kubernetes
        version: 0.9.0
    ```
    {: codeblock}

3. Add a `collections` folder to your GitHub repository that is relative to your playbook, and store the `requirements.yml` file in this folder as shown in this example. 
    ```text
    ├── collections
            └── requirements.yml
    ├── playbook.yaml
    ├── README.md
    ```
    {: screen}

4. Reference a resource from your collection in your playbook. For more information, see the [Ansible documentation](https://docs.ansible.com/ansible/2.9/user_guide/collections_using.html#using-collections-in-a-playbook){: external}

## Preparing your Ansible playbook to run in {{site.data.keyword.bpshort}} 
{: #plan-ansible-playbook}

To prepare your Ansible playbook for {{site.data.keyword.bpshort}}, review the following considerations. 
{: shortdesc}

- Your Ansible playbook must be stored in a GitHub or GitLab repository. 
- You are limited in how you can specify the target hosts for your Ansible resource inventory. For more information, see [Creating resource inventories for {{site.data.keyword.bpshort}} Actions](/docs/schematics?topic=schematics-inventories-setup). 
- All playbooks must be compatible to run with an Ansible version that is supported in {{site.data.keyword.bpshort}}. To find supported versions run [`ibmcloud schematics version`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-version) command. 
- Optionally, to explore Ansible playbook capabilities in {{site.data.keyword.bpshort}}. You can try to use one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook){: external}.
