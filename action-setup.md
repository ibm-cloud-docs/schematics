---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-12"

keywords: schematics, schematics action, create schematics actions, run ansible playbooks, delete schematics action, 

subcollection: schematics

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"} 
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:beta: .beta}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}

# Setting up {{site.data.keyword.bpshort}} actions
{: #action-setup}

   The open beta release of Ansible support is now available in {{site.data.keyword.bplong_notm}} to IBM users.Contact your IBM Cloud Schematics Technical Offering Manager [Sai Vennam](mailto:svennam@us.ibm.com), if you are interested in getting early access to this beta offering. For more information, see [Beta limitations](/docs/schematics?topic=schematics-schematics-limitations#beta-limitations).
  {: beta}

With {{site.data.keyword.bplong_notm}} actions, you can specify the Ansible playbook that you want to run against one or more {{site.data.keyword.cloud_notm}} resources. An Ansible playbook is a configuration file that includes all the tasks, roles, policies, or steps that you want to run and the order in which you want to execute them. 
{: shortdesc}

## Creating and running {{site.data.keyword.bpshort}} action
{: #create-action}

Create a {{site.data.keyword.bpshort}} action and specify the Ansible playbook that you want to run against your {{site.data.keyword.cloud_notm}} resources.  

Before you begin: 

- Create an Ansible playbook and store the playbook in a GitHub or GitLab repository. 
- Make sure that you have the [required permissions](/docs/schematics?topic=schematics-action-setup) to create an action. 

The URL provided for required permission need to be edited when published.
{: note}

To create an action:

1. From the [{{site.data.keyword.bpshort}} actions dashboard](https://cloud.ibm.com/schematics/actions), click **Create action**.
2. Configure your action. 
   1. Enter a name and an optional description for your action. The name can be up to 128 characters long and can include alphanumeric characters, spaces, dashes, and underscores. 
   2. Optional: Enter any tags that you want to add to your action. Tags can help you find an action more easily later.
   3. Select the resource group where you want to create the action.
   4. Decide where you want to create your action. The location determines where your action runs and your action data is stored. You can choose between a geography, such as North America, or a metro city, such as Frankfurt or London. If you select a geography, {{site.data.keyword.bpshort}} determines the location based on availability. If you select a metro city, your workspace is created in this location. For more information about where your data is stored, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location). The location that you choose is independent from the region or regions where the {{site.data.keyword.cloud_notm}} resources reside where you want to run your Ansible playbook.
3. Import your Ansible playbook. 
   1. Enter the URL to your GitHub or GitLab repository where you store the Ansible playbook that you want to run in the **Template**. The URL can point to the master branch, any other branch, or a subdirectory. Your action can point to one playbook at a time. If you want to run multiple playbooks, create a separate action for each playbook. 
      - Example for master branch: https://github.com/myorg/myrepo
      - Example for other branches: https://github.com/myorg/myrepo/tree/mybranch
      - Example for subdirectory: https://github.com/mnorg/myrepo/tree/mybranch/mysubdirectory
   2. If you want to use a private GitHub repository, enter your personal access token. The personal access token is used to authenticate with your GitHub repository to access your Ansible playbook. For more information, see [Creating a personal access token for the command line](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/creating-a-personal-access-token){: external}.
   3. Select the Ansible version that your playbook is written in.
       
       Your action can point to one playbook at a time. If you want to run multiple playbooks, create a separate action for each playbook.
       {: note}

   4. Click **Retrieve playbooks**. {{site.data.keyword.bpshort}} connects to your repository and retrieves a list of executable playbooks from the repository.
   5. Select the Ansible playbook that you want to run. 
   6. Choose if you want to run your Ansible playbook (**Run Job**) or if you want to preview a summary of all the steps that Ansible wants to run without executing these steps in your cloud environment (**Check**). 
   7. Select the detail level for your Ansible logs from the **Verbosity** drop down list. The logs are shown when you run the playbook in {{site.data.keyword.bpshort}}. For example, if you want to debug your playbook or want to include a detailed summary for each task that Ansible executes, select a high verbosity level.
   8. Enter the tags for the Ansible tasks or roles that you do not want to include when you run your playbook in the **Skip Ansible tags** field. Tags are often used in complex playbooks to identify {{site.data.keyword.bpshort}} actions, policies or steps that must be executed together. When you enter a tag, {{site.data.keyword.bpshort}} scans your playbook for these tags and excludes all parts of the playbook that are marked with this tag. To enter multiple tags, separate them with a comma (`,`). 
   9. Choose if you want to enable privilege escalation. This feature allows you to use a different user than the one who logs in to the {{site.data.keyword.cloud_notm}} resource where you want to run the playbook. For more information, see [Understanding Privilege Escalation](https://docs.ansible.com/ansible/2.5/user_guide/become.html){: external}. 
   10. Click **Next**. 
4. Choose your {{site.data.keyword.cloud_notm}} resource inventory. 
   1. Enter the host or the IP address where you want to run your Ansible playbook in the  **Bastion host IP** field. 
   2. Enter the {{site.data.keyword.cloud_notm}} resource inventory hostnames or the IP addresses by using a `,` or `comma` separator in the **IBM cloud inventory IP addresses**. These resources are referred to as the resource inventory. You can use an existing resource inventory, or create a new one by using the inventory selector wizard or uploading a file that includes the IP addresses or hostnames of the {{site.data.keyword.cloud_notm}} hosts that you want to connect to.
   3. Enter your webserver host, Operating System, region, network, or the database host name with the IP addressed in the **{{site.data.keyword.cloud_notm}} inventory host groups** as shown in the example. For more information, about an inventory host group syntax, refer to [Inventory host groups](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-inventory-host-grps).

      **Example** 

       ```
        [webserverhost]
        178.54.68.78
        [dbhost]
        174.45.86.87
       ```
       {: codeblock}

   4. Select the host credentials to be as a proxy between an SSH client and the Ansible inventory resources where you want to run an Ansible playbook in the **IBM cloud resource inventory SSH key**. This setup adds an layer of security to your {{site.data.keyword.cloud_notm}} resources and minimized the surface of potential vulnerabilities.
   5. Select Check or clear in the **Use the same key for {{site.data.keyword.cloud_notm}} resource inventory and Bastion host** field. In case unchecked the field, you need to enter a SSH key in the **bastion host SSH key** field.
   6. Click **Next**.

   As a temporary feature in the beta release, enter the target IP addresses and SSH keys for creating an action. 
   {: note}

5. Define your variables.
   1. You can create an optional extra command line fields for your playbook by entering the input values with the key and value pairs. to make a secret value for your variable, you can select the sensitive option.
   2. Wait for the playbook, resource inventory and variables to process for few minutes and click **Launch action** to land into the Jobs page.

      Before your launch action, you can also observe the log items in the `Jobs` page, that is polled by the APIs to create {{site.data.keyword.bpshort}} actions. Some of these jobs are polled by the asynchronous API calls. Every time you execute the patch action, the `JOB.new-action.ansible` job lists are created.
      {: note}



## Editing a {{site.data.keyword.bpshort}} actions in {{site.data.keyword.bpshort}}
{: #editing-ansible-actions}

You can edit a {{site.data.keyword.bpshort}} action by clicking the `edit` icon from the `Settings` page. Once the edit is completed, you can relaunch an action, by clicking `Launch action` button. 

## Deleting a {{site.data.keyword.bpshort}} actions in {{site.data.keyword.bpshort}}
{: #delete-ansible-actions}

Delete a {{site.data.keyword.bpshort}} action and specify the Ansible playbook that you want to run against your {{site.data.keyword.cloud_notm}} resources.
1. From the [{{site.data.keyword.bpshort}} actions dashboard](https://cloud.ibm.com/schematics/actions), click **Action** tab. 
2. Select the action you want to delete, click `Actions > Delete` from the right tab. You will receive a confirmation dialog box to delete the selected action.

   You cannot delete or stop the job item from an on going execution of an action defined in the playbook. You can repeat the execution of same job, whenever you patch the actions.
   {: note}


## Schematics action state diagram
{: #action-state-diagram}

The {{site.data.keyword.bpshort}} system state indicates the results of creating and processing template. The Schematics user state is set by the administrator or the template manager.  While processing the action document, Schematics will run jobs that can be in any of the following exemplary status. The table represents the Schematics action state and description.
{: shortdesc}

|State|Description|
|------|-------|
|Normal|Administrator publishes action to enable visibility to end users and user execution.|
|Disabled|Disallows user execution.|
|Locked|After configuration is in `Normal` state. Action can be locked by an administrator to stop further change.|
|Critical| When the template is unable to download the repository, or the repository name is invalid, the template fails and changes the action state as critical.|
{: caption="Schematics action state" caption-side="top"}

The following table represents the state diagram flow of the Schematic action.

<table>
   <thead>
    <th style="width:50px">Workspace / Action</th>
    <th style="width:200px">State diagram</th>
    <th style="width:250px">Description</th>
  </thead>
  <tbody>
    <tr>
      <td><code>Create action</code></td>
      <td><img src="images/createaction.png" alt="Create action"  width="800" style="width: 800px; border-style: none"/></td>
      <td>When processing the create action, the {{site.data.keyword.bpshort}} download the document and change the action into draft state. From the draft state you can connect to the infrastructure template in your source repository to see pending state. From pending state, the {{site.data.keyword.bpshort}} downloads the job to execute and reach the normal state or disable state. If the template manager sets disable to stop the usage of the user execution, the action state changes to disabled state. Or if the template execution fails, the action state changes into critical state.</td>
   </tr>
     <tr>
      <td><code>Delete action</code></td>
      <td><img src="images/deleteaction.png" alt="Delete action"  width="800" style="width: 800px; border-style: none"/></td>
      <td>When the action is in delete state, the {{site.data.keyword.bpshort}} will run the Job and the action state changes into pending state. From the pending state if the template deletion fails, the action state changed into critical state.</td>
   </tr>
    <tr>
      <td><code>Update action</code></td>
      <td><img src="images/updateaction.png" alt="Update action" width="800" style="width: 800px; border-style: none"/></td>
      <td>When processing the update action, the {{site.data.keyword.bpshort}} download the document and change the action state into pending state. If the template execution is success you can see the normal state. If the template manager sets disable to stop the usage of the user execution, the action state changes to disabled state. Or if the template execution fails, the action state changes into critical state.
      </td>
   </tr>
  </tbody>
  </table>

 