---

copyright:
  years: 2017, 2025
lastupdated: "2025-10-30"

keywords: schematics, schematics action update, update schematics actions, update ansible playbooks, update action,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Updating a {{site.data.keyword.bpshort}} action
{: #action-working-update}

Updating {{site.data.keyword.bpshort}} actions involves modifying the action definition to incorporate new configurations or automation logic. After making changes, validate and reapply the updated action to ensure the revised workflow functions correctly across targeted resources, enabling the deployment of complex multitiered applications to your cloud infrastructure.
{: shortdesc}

## Before you begin
{: #action-working-prereq}

Before initiating the updation of a {{site.data.keyword.bpshort}} action for Ansible playbooks, confirm that you meet the following prerequisites:

- Create an Ansible playbook and store it in a GitHub or GitLab repository. Alternatively, use one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook){: external}.
- Verify that you possess the required [permissions](/docs/schematics?topic=schematics-access) to create a {{site.data.keyword.bpshort}} action.

Make sure that the `location` and the `url` endpoint are consistent when creating or updating {{site.data.keyword.bpshort}} workspaces and actions, as they must point to the same region. This helps ensure [proper data storage](/docs/schematics?topic=schematics-secure-data#pi-location) and access for your automation tasks.
{: note}

## Updating an action by using Console
{: #action-settings}
{: ui}

Update a {{site.data.keyword.bpshort}} action and specify the Ansible playbook that you want to run against your Cloud resources.
{: shortdesc}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![Hamburger icon](images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > [**Ansible**](https://cloud.ibm.com/automation/schematics/ansible){: external} > `click your Ansible Action name`.
3. From the **Overview** > **Details** section, click `Edit` icon.
4. Modify the action details such as **Description**, **GitHub, GitLab or Bitbucket repository URL**, **Branch (optional)**, **Folder (optional)**, **Personal access token (Only necessary for private repository)**, **Playbook**, and **Verbosity**.
5. In **Update inventory** page, Optionally, edit **Inventory name**, **Description**, **Connection type** (`SSH` or `WinRM`), **Host groups**, define the host group upon which your playbook operates by using [.ini file format](/docs/schematics?topic=schematics-inventories-setup#inv-file-format), **GitHub, GitLab or Bitbucket repository URL**, **Branch (optional)**, **Folder (optional)**, **Personal access token (Only necessary for private repository)**, **Playbook**, and **Verbosity**.
6. Click your **Inventory** and then click [...] to `Manage credentials`, `Manage variables`, or `Delete` inventories.
7. Click **Check action** to verify the tasks you are trying to run, and help ensure that your resources are properly linked to your playbook. You can monitor the **Action history** section to view the results in the logs. This check verifies the tasks that you are trying to run, and ensure that your resources are properly linked to your playbook.

      You cannot delete or stop a running job of your {{site.data.keyword.bpshort}} action. To change your action, wait for the job to complete, then change your settings, and click **Check action** or **Run action** again.
      {: note}

8. Finally, click **Run action** to run the action. You can monitor the progress of an action by reviewing the logs on the **Jobs** page. Every `30 seconds` the job logs are automatically refreshed.

    In the console, there is no limit to the number of job logs displayed. The logs are automatically refreshed every `30 seconds`. For a complete view of your action's job logs, use the [`ibmcloud schematics job list`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-list-job) command.
    {: note}

## Next steps
{: #action-working-update-nextsteps}

After setting up and updating your {{site.data.keyword.bpshort}} action, you can further enhance your automation workflow with the following options:

- [Creating an auto deploy button](/docs/schematics?topic=schematics-auto-deploy-url): This feature allows you to trigger your {{site.data.keyword.bpshort}} action automatically, streamlining the deployment process and reducing manual intervention.

- [Deleting {{site.data.keyword.bpshort}} action](/docs/schematics?topic=schematics-delete-ansible-actions): If you no longer require a specific action, you can delete it to maintain a clean and organized automation environment.
