---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-27"

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


With {{site.data.keyword.bplong_notm}} actions, you can specify the Ansible playbook that you want to run against one or more {{site.data.keyword.cloud}} resources. An Ansible playbook is a configuration file that includes all the tasks, roles, policies, or steps that you want to run and the order in which you want to execute them. 
{: shortdesc}

## Creating and running the {{site.data.keyword.bpshort}} action
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
   4. Decide where you want to create your action. The location determines where your action runs and your action data is stored. You can choose between a geography, such as North America, Frankfurt or London. If you select a geography, {{site.data.keyword.bpshort}} determines the location based on availability. Make sure of your location, you cannot update the location and region once an action is created. For more information, about where your data is stored, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location). The location that you choose is independent from the region or regions where the {{site.data.keyword.cloud_notm}} resources reside where you want to run your Ansible playbook.
   5. Click **Create** to create an action. Your action is created with a `Normal` state and you are directed to the `Settings` page.
3. Import your Ansible playbook. 
   1. Enter the URL to your GitHub or GitLab repository where you store the Ansible playbook that you want to run. The URL can point to the master branch, any other branch, or a subdirectory. Your action can point to one playbook at a time only. If you want to run multiple playbooks, create a separate action for each playbook. 
      - Example for master branch: https://github.com/myorg/myrepo
      - Example for other branches: https://github.com/myorg/myrepo/tree/mybranch
      - Example for subdirectory: https://github.com/mnorg/myrepo/tree/mybranch/mysubdirectory
      
      Don't have a playbook that you can use? Try out one of our [sample playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook){: external}. 
      {: tip}
      
   2. If you want to use a private GitHub repository, enter your personal access token. The personal access token is used to authenticate with your GitHub repository to access your Ansible playbook. For more information, refer to [creating a personal access token for the command line](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token){: external}.
   3. Review the default Ansible version that is displayed. If you use your own Ansible playbook, make sure that your playbook can be run with the Ansible version that is displayed. 
   4. Click **Retrieve playbooks**. {{site.data.keyword.bpshort}} connects to your repository, and a retrieves a list of Ansible playbooks that are found in the repository.
   5. Select the playbook that you want to run. 
   6. Select the **Verbosity** level that you want. The verbosity level determines how much information is written to the logs when your Ansible playbook is executed. The supported values are `0 (Normal)`, `1 (verbose)`, `2 (More Verbose)`, `3 (Debug)`, `4 (Connection Debug`). For example, if you want to debug your playbook or want to include a detailed summary for each task that Ansible executes, select a high verbosity level. You can see the logs in {{site.data.keyword.bpshort}} when you run your playbook. 
   7. Optional: Click the **Advanced options** to define command line variables that you want to pass to the playbook. Command line variables must be entered as key-value pairs. If the variable contains sensitive information, enable the **Sensitive** option so that the value is hidden from the users who look at your action after it is created. 
   8. Click **Next** to save the action details. {{site.data.keyword.bpshort}} verifies the YAML file and displays the action settings page to configure the {{site.data.keyword.cloud_notm}} resource inventory where you want to run your Ansible playbook. 
4. Choose the {{site.data.keyword.cloud_notm}} resource inventory where you want to run your Ansible playbook. 
   1. Click the `edit` icon. 
   2. Enter the host or the IP address where you want to run your Ansible playbook in the  **Bastion host IP** field. 
   3. Enter the {{site.data.keyword.cloud_notm}} resource inventory hostnames or the IP addresses by using a `comma` separator in the **IBM Cloud inventory IP addresses**. These resources are referred to as the resource inventory. You can use an existing resource inventory, or create a new one by using the inventory selector wizard or uploading a file that includes the IP addresses or hostnames of the {{site.data.keyword.cloud_notm}} hosts that you want to connect to.
   3. Enter your web server host, Operating System, region, network, or the database host name with the IP addressed in the **{{site.data.keyword.cloud_notm}} inventory host groups** as shown in the example. For more information, about an inventory host group syntax, refer to [Inventory host groups](/docs/schematics?topic=schematics-schematics-cli-reference#inventory-host-grps).

      **Example** 

       ```
        [webserverhost]
        178.54.68.78

        [dbhost]
        174.45.86.87
       ```
       {: codeblock}      
   4. Enter the host credentials to be as a proxy between a SSH client and the {{site.data.keyword.cloud_notm}} inventory resources where you want to run an Ansible playbook in the **IBM cloud resource inventory SSH key** field. This set up adds a layer of security to your {{site.data.keyword.cloud_notm}} resources, and minimize the surface of potential vulnerabilities. **Note** Currently {{site.data.keyword.bplong_notm}} actions supports only `one SSH key` for all virtual server instances.
   5. Click **Next** to save the {{site.data.keyword.cloud_notm}} resource inventory details.
   6. Click **Check action** to validate the configuration and **Run action** to execute the configured actions. For more information, about Check action and Run action, refer to [{{site.data.keyword.bpshort}} action settings](/docs/schematics?topic=schematics-action-setup#action-settings).

      Before your launch action, you can observe the log items in the `Jobs` page, that is polled by the APIs to create {{site.data.keyword.bpshort}} actions. Some of these jobs are polled by the asynchronous API calls. Every time you execute the patch action, the `JOB.new-action.ansible` job lists are created.
      {: note}



## Editing the {{site.data.keyword.bpshort}} actions in {{site.data.keyword.bpshort}}
{: #editing-ansible-actions}

You can edit a {{site.data.keyword.bpshort}} action by clicking the `edit` icon from the `Settings` page. Once the edit is completed, you can relaunch an action, by using  `Launch action` button. 

## Deleting the {{site.data.keyword.bpshort}} actions in {{site.data.keyword.bpshort}}
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


## Viewing the {{site.data.keyword.bpshort}} action jobs
{: #action-jobs}

The {{site.data.keyword.bpshort}} action user interface provides the **Jobs** and **Settings** feature to analyze your action execution. When the create action executes, you are directed to the user job page to view the job activity messages. The list of options are displayed in the Action List pane on the left navigation pane of the window.

The **Jobs** lists the activity stream that are performed when the action were created or updated. The list of options are displayed in the Action List pane on the left side of the window and on the right side of the window you can view the quick access to **Adjust your settings**, **Get help from the documentation**, and **Learn more about Schematics**.

Jobs are classified into:
- **System jobs** These jobs are created during the create and update action. The **All** tab in the user interface represents System jobs. For example, `playbook run`, `playbook check`. 
{: shortdesc}
- **User jobs** These are the jobs that gets created with an user action. The summary of the system jobs are shown in the following status in the **User** tab. In the user interface, there is no limit set to display the job logs. However, you can use [ibmcloud schematics job list](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-list-job) command to view the complete job logs of your action.

|Status|Description|
|-----|---------|
|`ok` |Accesses the remote machine and perform the playbook action successfully. This displays the success count of, number of systems that actions got performed. |
|`changed` | Accesses the remote machine and perform the playbook action successfully. Displays the list of machines where changes got implied.|
|`failed` |Total count of machines that were failed to apply changes. |
|`skipped` |Total count of machines that were skipped as the playbook changes have already applied.|
|`unreachable` |Total number of machines not able to reach out the machine to imply changes on it. |
{: caption="Job status" caption-side="top"}

## Editing the {{site.data.keyword.bpshort}} action settings
{: #action-settings}

The **Settings** option allows you to edit the action **Details**, **Ansible action**, and an **{{site.data.keyword.cloud_notm}} resource inventory** parameters. Then, you can click `Save` button to save the edited configuration. Following are the details for the action parameters.

1. **Details**

   You can click the **Edit details** icon to edit the Action description, Resource group, Location and click Save.

2. **Ansible action**

   You can click the **Edit import** icon to edit the GitHub or GitLab repository URL, Personal access token, Playbook name, Verbosity, Advanced options to define your variables.

3. **{{site.data.keyword.cloud_notm}} resource inventory**

   You can click the **Edit inventory** icon to edit the Bastion host IP, {{site.data.keyword.cloud_notm}} inventory host groups, and {{site.data.keyword.cloud_notm}} resource inventory SSH key.

Finally, you can click **Run action** or **Check action** to validate and reexecute your action playbook.

|action|Description|
|----|-----|
|`Run action`|Executes the updated action configuration in a check or non check mode. Check mode is a simulation, does not generate an output for the tasks that you have conditional variables. This task will ignore errors in check mode.|
|`check action`|Validates the configuration management playbooks that runs on a single node at a time.|
{: caption="Action settings" caption-side="top"}

