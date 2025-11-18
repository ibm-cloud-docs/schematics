---

copyright:
  years: 2017, 2025
lastupdated: "2025-11-18"

keywords: schematics ansible, schematics action, create schematics actions, run ansible playbooks

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Creating an Ansible playbook
{: #create-playbook}

To create your Ansible playbook for use with {{site.data.keyword.bpshort}}, follow these [prerequisites](/docs/schematics?topic=schematics-create-playbook#plan-ansible-playbook) and the general steps.
{: shortdesc}

Want to use existing Ansible playbooks to get started? Try out one of the [IBM-provided Ansible playbooks](/docs/schematics?topic=schematics-sample_actiontemplates) or browse existing Ansible collections and roles in [Ansible Galaxy](https://galaxy.ansible.com/){: external}
{: tip}

- [Prepare your resource inventory where you intend to run your Ansible playbook](/docs/schematics?topic=schematics-inventories-setup). You can also use {{site.data.keyword.bpshort}}'s built-in Terraform capabilities to provision target hosts. For more information, see [Infrastructure deployment with {{site.data.keyword.bpshort}} workspaces](/docs/schematics?topic=schematics-how-it-works#how-to-workspaces).
- Develop your Ansible playbook. Use one of the [IBM-provided playbooks](/docs/schematics?topic=schematics-sample_actiontemplates) or explore existing roles and collections in [Ansible Galaxy](https://galaxy.ansible.com/){: external}. Reference these [roles](/docs/schematics?topic=schematics-ansible-roles-galaxy#main-file) and collections](/docs/schematics?topic=schematics-create-playbook#schematics-collections) in your playbook as needed.
- Establish a GitHub or GitLab repository, and organize your Ansible playbook, modules, roles, and collections according to the required directory structure. A sample structure can be found in this [sample playbook](https://github.com/Cloud-Schematics/ansible-app-deploy-iks/blob/master/site.yml){: external}.
- Upload your Ansible playbook, modules, roles, and collections to your GitHub repository.
- [Create a {{site.data.keyword.bpshort}} action](/docs/schematics?topic=schematics-action-working&interface=ui) by using the uploaded playbook.

Ensure that your playbook adheres to the necessary structure and references any required roles and collections for seamless execution in {{site.data.keyword.bpshort}}. For more information, see the [Ansible documentation](https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_intro.html){: external} or [playbook creation](https://docs.ansible.com/projects/ansible/latest/getting_started/get_started_playbook.html){: external}.

## Referencing Ansible collections in your playbook
{: #schematics-collections}

Ansible collections are groups of reusable Ansible resources, such as playbooks, modules, and roles, that you can install and use in your playbook. Collections are available in the [Ansible Galaxy](https://galaxy.ansible.com/){: external} repository.
{: shortdesc}

Similar to [Ansible roles](/docs/schematics?topic=schematics-ansible-roles-galaxy#main-file), collections require a specific folder structure in your GitHub repository.

Follow these steps to use collections in your {{site.data.keyword.bpshort}} playbook

1. Browse [Ansible Galaxy](https://galaxy.ansible.com/){: external} to find the collection that you want to use in your playbook.
2. Create a `requirements.yml` file to specify the collections you want to install from Ansible Galaxy. The file structure should follow the [Ansible documentation](https://docs.ansible.com/projects/ansible/latest/galaxy/user_guide.html#installing-collections). Here's an example by using the `community.kubernetes` collection.

    ```yaml
    collections:
      - name: community.kubernetes
        version: 0.9.0
    ```
    {: codeblock}

3. Add a `collections` folder to your GitHub repository, relative to your playbook, and place the `requirements.yml` file inside this folder.

    ```text
    ├── collections
            └── requirements.yml
    ├── playbook.yaml
    ├── README.md
    ```
    {: screen}

4. Reference a resource from your collection in your playbook. For more information, see the [Ansible documentation](https://docs.ansible.com/projects/ansible/2.9/user_guide/collections_using.html#using-collections-in-a-playbook){: external}. Ensure your playbook's folder structure adheres to the requirements and properly references the collections for seamless execution in {{site.data.keyword.bpshort}}.

## Preparing Your Ansible Playbook for {{site.data.keyword.bpshort}}
{: #plan-ansible-playbook}

Before running your Ansible playbook in {{site.data.keyword.bpshort}}, consider the following points:
{: shortdesc}

- Store your Ansible playbook in a GitHub or GitLab repository.
- Be aware of the limitations when specifying target hosts for your Ansible resource inventory. For more information, refer to the guidelines on [Creating resource inventories for {{site.data.keyword.bpshort}} actions](/docs/schematics?topic=schematics-inventories-setup).
- Ensure that your playbooks are compatible with an Ansible version that is supported in {{site.data.keyword.bpshort}}. To check supported versions, run the [`ibmcloud schematics version`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-version) command.
- Optionally, explore Ansible playbook capabilities in {{site.data.keyword.bpshort}} by using one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook).

## Next steps
{: #create-playbook-nextsteps}

After understanding the prerequisites and preparation steps for your Ansible playbook, the next step is to [create a {{site.data.keyword.bpshort}} action](/docs/schematics?topic=schematics-action-working). This process involves specifying your Ansible playbook, configuring the resource inventory, and setting up any necessary credentials or variables. Follow the guide on creating a {{site.data.keyword.bpshort}} action to proceed.
