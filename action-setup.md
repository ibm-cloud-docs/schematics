---

copyright:
  years: 2017, 2021
lastupdated: "2021-11-08"

keywords: schematics, schematics action, create schematics actions, run ansible playbooks, delete schematics action, 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Setting up {{site.data.keyword.bpshort}} actions
{: #action-setup}

Run your Ansible playbook in {{site.data.keyword.cloud_notm}} by using {{site.data.keyword.bpshort}} actions. 

An [Ansible playbook](https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook){: external} is a set of instructions or automation tasks that you can configure to run on a single target, or a group of target hosts. These target hosts are also referred to as inventory. Ansible playbook includes tasks, roles, policies, or steps to deploy your resources in the target hosts. You can run your automation tasks in the order in which you want them to run to perform managed operations on the {{site.data.keyword.cloud}} resource.
{: shortdesc}

## Creating and running the {{site.data.keyword.bpshort}} action
{: #create-action}

Create a {{site.data.keyword.bpshort}} action and specify the Ansible playbook that you want to run against your {{site.data.keyword.cloud_notm}} resources. 
{: shortdesc}

**Before you begin**: 

- Create an Ansible playbook and store the playbook in a GitHub or GitLab repository. You can also try out one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook){: external}. 
- Make sure that you have the [required permissions](/docs/schematics?topic=schematics-access) to create a {{site.data.keyword.bpshort}} action. 

Ensure the `location` and the `url` endpoint are pointing to the same region when you create or update the {{site.data.keyword.bpshort}} workspace and actions. For more information, about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
{: note}

**To create an action**:

1. From the [{{site.data.keyword.bpshort}} actions dashboard](https://cloud.ibm.com/schematics/actions), click **Create action**.
2. Configure your action. 
    1. Enter an **Action name** and an optional **Action description** for your action. The name can be up to 128 characters long and can include alphanumeric characters, spaces, dashes, and underscores. 
    2. Optional: Enter the tags that you want to add to your action. Tags can help you find an action more easily later.
    3. Select the resource group where you want to create the action. 
    4. Select the **Location** where you want to create the action. The location determines where your action runs and action data are stored. You can choose between a geography, such as `North America`, or a location, such as `Frankfurt` or `London`. If you select a geography, {{site.data.keyword.bpshort}} decides on a location within this geography based on availability. Be sure that you can store your action data in this location as you cannot change the location after the action is created. For more information, about where your data is stored? see [where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location). Note that the location of your action is independent from the location of your {{site.data.keyword.cloud_notm}} resource where you want to run your Ansible playbook.
    5. Click the **Create** to create an action. Your action is created with a `Normal` state, and you are directed to the `Details` section.
3. In the **Ansible playbook** section, click **Edit icon** to import your Ansible playbook. 
    1. Enter the **Repository URL** of your GitHub or GitLab repository where your Ansible playbook is stored. The URL can point to the master branch, any other branch, or a subdirectory. If your repository stores multiple playbooks, you must select the playbook that you want to run later. A {{site.data.keyword.bpshort}} action can point to one playbook at a time. To run multiple playbooks, you must create a separate action for each playbook. 
        - Example for master branch - `https://github.com/myorg/myrepo`
        - Example for other branches - `https://github.com/myorg/myrepo/tree/mybranch`
        - Example for subdirectory - `https://github.com/mnorg/myrepo/tree/mybranch/mysubdirectory`

        Don't have a playbook that you can use? Try out one of our [sample playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook){: external}. 
        {: tip}

    2. Optional: If you want to use a private GitHub repository, enter your personal access token. The personal access token is used to authenticate with your GitHub repository to access your Ansible playbook. For more information, about how to create an access token, see [creating a personal access token for the command-line](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token){: external}. If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-faqs#clone-file-extension) for cloning.
    3. Review the default Ansible version that is used to run your playbook. This version cannot be changed. If you use your own Ansible playbook, make sure that your playbook can be run with this Ansible version. 
    4. Click **Retrieve playbooks** to connect to your repository and retrieve all Ansible playbooks from your Git repository.
    5. Select the playbook that you want to run. A {{site.data.keyword.bpshort}} action can point to one playbook at a time. To run multiple playbooks, you must create a separate action for each playbook.
    6. Select the **Verbosity** level. The verbosity level determines the depth of information that is written to the logs when your Ansible playbook is executed. The supported values are `0 (Normal)`, `1 (verbose)`, `2 (More Verbose)`, `3 (Debug)`, `4 (Connection Debug`). For example, if you want to debug your playbook or want to include a detailed summary for each task that Ansible runs, select a high verbosity level. You can view the logs when your playbook runs. 
    7. Optional: Click the **Advanced options** to define input variables that you want to pass to the playbook. Input variables must be entered in key-value pairs. If the variable contains sensitive information, enable the **Sensitive** option so that the value is hidden for the users after the action is created. If you use one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook){: external}, all input variables can be found in the `readme.md` file. 
    8. Click **Save** to save the action details. 
4. Configure your resource inventory. The resource inventory includes all target hosts where you want to run your Ansible playbook.
    1. In the **IBM Cloud resource inventory** section, click the **Edit icon**. 
    2. If your Ansible playbook requires a bastion host, enter the IP address of your bastion host. The bastion host sits in front of your target hosts and proxies all Ansible SSH connections to the target hosts.
    3. From the resource inventory table, select an existing resource inventory. If you do not have a resource inventory yet, click **Create Inventory** to create one. For more information about resource inventories, see [Creating resource inventories for {{site.data.keyword.bpshort}} actions](/docs/schematics?topic=schematics-inventories-setup). 
    4. In the **IBM Cloud resource inventory SSH key** field, enter the private SSH key that you want to use to connect to your target hosts. All hosts must be configured with the matching public SSH key so that {{site.data.keyword.bpshort}} can connect to your hosts and run your playbook. To use a different SSH key to connect to your bastion host, deselect the **Use the same key for IBM Cloud resource inventory and Bastion host** option and enter your SSH key in the **Bastion host SSH key** field.
    5. Click **Save** to save your resource inventory details.
5. Click **Check action** to verify your action details. The **Jobs** page opens automatically and you can view the results in the logs. This check verifies that your playbook can be executed in {{site.data.keyword.bpshort}}. If you need to change your action settings, return to the action's **Settings** page and click the edit icon. 
6. Click **Run action** to run the action. You can monitor the progress of an action by reviewing the logs on the **Jobs** page. Every `30 seconds` the job logs are automatically refreshed. 

    You cannot delete or stop a running job for your {{site.data.keyword.bpshort}} action. To make changes to your action, wait for the job to complete, then change your settings, and click **Check action** or **Run action** again. 
    {: note}


## Deleting a {{site.data.keyword.bpshort}} action
{: #delete-ansible-actions}

If you no longer need your {{site.data.keyword.bpshort}} action, you can delete it. Deleting an action removes the Ansible playbook and your action data. However, any configurations on your target hosts that you made by using the action are not removed. 
{: shortdesc}

1. From the [{{site.data.keyword.bpshort}} actions dashboard](https://cloud.ibm.com/schematics/actions), find the action that you want to delete.
2. From the actions menu, click **Delete**. 



## Reviewing the {{site.data.keyword.bpshort}} action job details
{: #action-jobs}

Use the {{site.data.keyword.bpshort}} action job details to find a history of all {{site.data.keyword.bpshort}}-internal activities, such as downloading your Ansible playbook or verifying your playbook, and to see the Ansible logs for the playbook that you ran on your target hosts. 
{: shortdesc}

Jobs are classified into the following categories:
- **System jobs**: These jobs represent all {{site.data.keyword.bpshort}}-internal activities and checks, such as downloading your Ansible playbook from GitHub or verifying your playbook. You can find these jobs in the **All** tab on the action's **Jobs** page. 
- **User jobs**: These jobs are created when you check or run an action. You can find a summary of all user-initiated jobs when you click on the **User** tab on the action's **Jobs** page. 

Review the following status that can be assigned to a job: 

|Status|Description|
|-----|---------|
|`ok` |The total number of target hosts where the Ansible playbook was successfully executed.  |
|`changed` | The total number of target hosts that were accessed and changed. |
|`failed` |The total number of target hosts where the Ansible playbook could not be successfully executed.  |
|`skipped` |The total number of target host that were accessed but could not be updated because changes have already applied to these hosts.|
|`unreachable` |The total number of target hosts that could not be found or reached. |
{: caption="Job status" caption-side="top"}





