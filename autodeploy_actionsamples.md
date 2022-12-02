---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-02"

keywords: action templates, schematics template, terraform template

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Sample Ansible playbook templates for {{site.data.keyword.bpshort}} Actions
{: #sample_actiontemplates}

Explore the IBM provided Ansible playbooks to run cloud operations on target hosts or to get started with {{site.data.keyword.bpshort}} Actions.
{: shortdesc}

Use the links on the page as follows: 
- `View GitHub repo`: Click the link to open the Git repository where the template is stored. You can review the file structure, the Ansible playbook instructions, and the `README` file that contains the steps to use the template in {{site.data.keyword.bpshort}}.
- `Deploy to {{site.data.keyword.cloud_notm}}`: Button takes you to **Create an action** page with the **GitHub repository URL** and the **Action name** pre-populated. 

## Running cloud operations on {{site.data.keyword.vsi_is_short}}
{: #ansible-vpc}

Description
:    Use `ansible-is-instance-actions` Ansible playbook to perform Day 2 operations such as start, stop, and restart for {{site.data.keyword.vsi_is_full}}. To configure and run the {{site.data.keyword.bpshort}} Actions by using the CLI or console, see [readme file](https://github.com/Cloud-Schematics/ansible-is-instance-actions/blob/master/README.md){: external} template file.

Source
:    [View on GitHub](https://github.com/Cloud-Schematics/ansible-is-instance-actions){: external}

Deploy
:   <img usemap="#deploybutton_map1" alt="Auto deployment button" src="https://cloud.ibm.com/media/docs/images/icons/Deploy_to_cloud.svg"><map name="deploybutton_map1" alt="This image leads to create an action.">
    <area alt="Deploy to {{site.data.keyword.cloud_notm}}" title="Deploy to {{site.data.keyword.cloud_notm}}" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-is-instance-actions&repository=https://github.com/Cloud-Schematics/ansible-is-instance-actions" target="_blank" coords="1,3,139,20" shape="rect">
</map>

## Provisioning a LAMP stack on {{site.data.keyword.vsi_is_short}}
{: #ansible-lamp-stack}

Description
:    Use `lamp-simple` Ansible playbook to deploy the LAMP stack components on {{site.data.keyword.vsi_is_short}} by following a simple deployment architecture. To configure and run the {{site.data.keyword.bpshort}} Actions by using the CLI or console, see [readme file](https://github.com/Cloud-Schematics/lamp-simple/blob/master/README.md){: external} template file.

Source
:    [View on GitHub](https://github.com/Cloud-Schematics/lamp-simple){: external}

Deploy
:   <img usemap="#deploybutton_map2" alt="Auto deployment button"  src="https://cloud.ibm.com/media/docs/images/icons/Deploy_to_cloud.svg"><map name="deploybutton_map2" alt="This image leads to create an action.">
    <area alt="Deploy to {{site.data.keyword.cloud_notm}}" title="Deploy to {{site.data.keyword.cloud_notm}}" href="https://cloud.ibm.com/schematics/actions/create?name=lamp-simple&repository=https://github.com/Cloud-Schematics/lamp-simple" target="_blank" coords="1,3,139,20"  shape="rect"></map>


## Configuring {{site.data.keyword.databases-for-postgresql_full_notm}} with WAL2JSON plug-in
{: #ansible-databases}

Description
:    Use `ansible-icd-postgres-actions` Ansible playbook to configure the WAL2JSON plug-in on your {{site.data.keyword.databases-for-postgresql_full_notm}} service instance. To configure and run the {{site.data.keyword.bpshort}} Actions by using the CLI or console, see [readme file](https://github.com/Cloud-Schematics/ansible-icd-postgres-actions/blob/master/README.md){: external} template file.

Source
:    [View on GitHub](https://github.com/Cloud-Schematics/ansible-icd-postgres-actions){: external}

Deploy
:   <img usemap="#deploybutton_map3" alt="Auto deployment button" src="https://cloud.ibm.com/media/docs/images/icons/Deploy_to_cloud.svg"><map name="deploybutton_map3" alt="This image leads to create an action.">
    <area alt="Deploy to {{site.data.keyword.cloud_notm}}" title="Deploy to {{site.data.keyword.cloud_notm}}" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-icd-postgres-actions&repository=https://github.com/Cloud-Schematics/ansible-icd-postgres-actions" target="_blank" coords="1,3,139,20" shape="rect"></map>

## Automating app deployments in {{site.data.keyword.containerfull_notm}}
{: #ansible-iks-deploy}

Description
:    Use `ansible-app-deploy-iks` Ansible playbook to deploy `Hackathon Starter` on an {{site.data.keyword.containerfull}} cluster. To configure and run the {{site.data.keyword.bpshort}} Actions by using the CLI or console, see [readme file](https://github.com/Cloud-Schematics/ansible-app-deploy-iks/blob/master/README.md){: external} template file.

Source
:    [View on GitHub](https://github.com/Cloud-Schematics/ansible-app-deploy-iks){: external}

Deploy
:   <img usemap="#deploybutton_map4" alt="Auto deployment button" src="https://cloud.ibm.com/media/docs/images/icons/Deploy_to_cloud.svg"><map name="deploybutton_map4" alt="This image leads to create an action.">
    <area alt="Deploy to {{site.data.keyword.cloud_notm}}" title="Deploy to {{site.data.keyword.cloud_notm}}" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-app-deploy-iks&repository=https://github.com/Cloud-Schematics/ansible-app-deploy-iks" target="_blank" coords="1,3,139,20" shape="rect"></map>

## Installing `kubectl` on {{site.data.keyword.vsi_is_short}}
{: #ansible-kubectl}

Description
:    Use `ansible-kubectl` Ansible playbook to install the Kubernetes CLI <code>kubectl</code> on {{site.data.keyword.vsi_is_short}} by using a role from Ansible Galaxy. To configure and run the {{site.data.keyword.bpshort}} Actions by using the CLI or console, see [readme file](https://github.com/Cloud-Schematics/ansible-kubectl/blob/master/README.md){: external} template file.

Source
:    [View on GitHub](https://github.com/Cloud-Schematics/ansible-kubectl){: external}

Deploy
:   <img usemap="#deploybutton_map5" alt="Auto deployment button" src="https://cloud.ibm.com/media/docs/images/icons/Deploy_to_cloud.svg"><map name="deploybutton_map5" alt="This image leads to create an action.">
    <area alt="Deploy to {{site.data.keyword.cloud_notm}}" title="Deploy to {{site.data.keyword.cloud_notm}}" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-kubectl&repository=https://github.com/Cloud-Schematics/ansible-kubectl" target="_blank" coords="1,3,139,20"  shape="rect"></map>

Looking for more code examples? Check out the [samples for {{site.data.keyword.bplong_notm}} GitHub repositories](https://github.com/Cloud-Schematics?q=Ansible&type=all&language=&sort=){: external}.
{: tip}
