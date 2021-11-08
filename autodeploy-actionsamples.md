---

copyright:
  years: 2017, 2021
lastupdated: "2021-11-04"

keywords: action templates, schematics template, terraform template

subcollection: schematics

---


{{site.data.keyword.attribute-definition-list}}



# Sample Ansible playbook templates for {{site.data.keyword.bpshort}} actions
{: #sample_actiontemplates}

Try out one of the IBM-provided Ansible playbooks to perform cloud operations on target hosts or to get started with {{site.data.keyword.bplong_notm}} actions.
{: shortdesc}

Use the links on this page as follows: 
- `View GitHub repo`: Click on this link to open the Git repository where the template is stored. You can review the file structure, the Ansible playbook instructions, and the `README` file that contains the steps to use the template in {{site.data.keyword.bpshort}}.
- `Deploy to IBM Cloud`: This link takes you to the **Create an action** page with the **GitHub repository URL** and the **Action name** pre-populated.  


## Running cloud operations on {{site.data.keyword.vsi_is_short}}
{: #ansible-vpc}

<table>
    <thead>
    <th>Name</th>
    <th>Description</th>
    <th>Access</th>
    </thead>
    <tbody>
        <tr>
        <td><code>ansible-is-instance-actions</code></td>
        <td>Use this Ansible playbook to perform day 2 operations such as start, stop, and reboot for {{site.data.keyword.vsi_is_full}}. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/ansible-is-instance-actions/blob/master/README.md).</td>
        <td> <img src="images/viewgithubrepo.png" alt="View GitHub repository" usemap="#viewgithubimage_map1">
<map name="viewgithubimage_map1">
    <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/ansible-is-instance-actions" target="_blank" coords="3,1,140,20" shape="rect">
</map><br><br><img usemap="#deploybutton_map1" alt="Auto deployment button" src="images/autodeploy_button.png"><map name="deploybutton_map1" alt="This image leads to create an action.">
    <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-is-instance-actions&url=https://github.com/Cloud-Schematics/ansible-is-instance-actions" target="_blank" coords="1,3,139,20" shape="rect">
</map></td>
    </tr>
    </tbody>
    </table>


## Provisioning a LAMP stack on {{site.data.keyword.vsi_is_short}}
{: #ansible-lamp-stack}

<table>
    <thead>
    <th>Name</th>
    <th>Description</th>
    <th>Access</th>
    </thead>
    <tbody>
        <tr>
        <td><code>lamp-simple</code></td>
        <td>Use this Ansible playbook to deploy the LAMP stack components on {{site.data.keyword.vsi_is_short}} by following a simple deployment architecture. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/lamp-simple/blob/master/README.md).</td>
        <td> <img src="images/viewgithubrepo.png"  alt="View GitHub repository" usemap="#viewgithubimage_map2">
<map name="viewgithubimage_map2">
    <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/lamp-simple" target="_blank" coords="3,1,140,20"  shape="rect">
</map><br><br><img usemap="#deploybutton_map2" alt="Auto deployment button"  src="images/autodeploy_button.png"><map name="deploybutton_map2" alt="This image leads to create an action.">
    <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=lamp-simple&url=https://github.com/Cloud-Schematics/lamp-simple" target="_blank" coords="1,3,139,20"  shape="rect"></map></td>
    </tr>
    </tbody>
    </table>


## Configuring {{site.data.keyword.databases-for-postgresql_full_notm}} with WAL2JSON plug-in
{: #ansible-databases}

<table>
    <thead>
    <th>Name</th>
    <th>Description</th>
    <th>Access</th>
    </thead>
    <tbody>
        <tr>
        <td><code>ansible-icd-postgres-actions</code></td>
        <td>Use this Ansible playbook to configure the WAL2JSON plug-in on your {{site.data.keyword.databases-for-postgresql_full_notm}} service instance. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/ansible-icd-postgres-actions/blob/master/README.md).</td>
        <td> <img src="images/viewgithubrepo.png"  alt="View GitHub repository" usemap="#viewgithubimage_map3">
<map name="viewgithubimage_map3">
    <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/ansible-icd-postgres-actions" target="_blank" coords="3,1,140,20"  shape="rect">
</map><br><br><img usemap="#deploybutton_map3" alt="Auto deployment button" src="images/autodeploy_button.png"><map name="deploybutton_map3" alt="This image leads to create an action.">
    <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-icd-postgres-actions&url=https://github.com/Cloud-Schematics/ansible-icd-postgres-actions" target="_blank" coords="1,3,139,20" shape="rect"></map></td>
    </tr>
    </tbody>
    </table>


## Automating app deployments in {{site.data.keyword.containerfull_notm}}
{: #ansible-iks-deploy}

<table>
    <thead>
    <th>Name</th>
    <th>Description</th>
    <th>Access</th>
    </thead>
    <tbody>
        <tr>
        <td><code>ansible-app-deploy-iks</code></td>
        <td>Use this Ansible playbook to deploy Hackathon Starter on an {{site.data.keyword.containerfull}} cluster. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/ansible-app-deploy-iks/blob/master/README.md).</td>
        <td><img src="images/viewgithubrepo.png"  alt="View GitHub repository" usemap="#viewgithubimage_map4">
<map name="viewgithubimage_map4">
    <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/ansible-app-deploy-iks" target="_blank" coords="3,1,140,20"  shape="rect">
</map><br><br><img usemap="#deploybutton_map4" alt="Auto deployment button" src="images/autodeploy_button.png"><map name="deploybutton_map4" alt="This image leads to create an action.">
    <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-app-deploy-iks&url=https://github.com/Cloud-Schematics/ansible-app-deploy-iks" target="_blank" coords="1,3,139,20" shape="rect"></map></td>
    </tr>
    </tbody>
    </table>

## Installing `kubectl` on {{site.data.keyword.vsi_is_short}}
{: #ansible-kubectl}

<table>
    <thead>
    <th>Name</th>
    <th>Description</th>
    <th>Access</th>
    </thead>
    <tbody>
    </tr>
        <tr>
        <td><code>ansible-kubectl</code></td>
        <td>Use this Ansible playbook to install the Kubernetes CLI <code>kubectl</code> on {{site.data.keyword.vsi_is_short}} by using a role from Ansible Galaxy. To configure and run the {{site.data.keyword.bpshort}} action by using the CLI or console, see the template [README file](https://github.com/Cloud-Schematics/ansible-kubectl/blob/master/README.md).</td>
        <td> <img src="images/viewgithubrepo.png"  alt="View GitHub repository" usemap="#viewgithubimage_map5">
<map name="viewgithubimage_map5">
    <area alt="View GitHub repo" title="View GitHub repo" href="https://github.com/Cloud-Schematics/ansible-kubectl" target="_blank" coords="3,1,140,20" shape="rect">
</map><br><br><img usemap="#deploybutton_map5" alt="Auto deployment button" src="images/autodeploy_button.png"><map name="deploybutton_map5" alt="This image leads to create an action.">
    <area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-kubectl&url=https://github.com/Cloud-Schematics/ansible-kubectl" target="_blank" coords="1,3,139,20"  shape="rect"></map></td>
    </tr>
    </tbody>
    </table>


