---

copyright:
  years: 2017, 2025
lastupdated: "2025-10-31"

keywords: schematics, schematics action, create schematics actions, run ansible playbooks, delete schematics action,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Creating {{site.data.keyword.bpshort}} actions
{: #action-working}

{{site.data.keyword.bpshort}} actions provide Ansible-as-a-Service capabilities, enables to automate the configuration and management of your {{site.data.keyword.cloud_notm}} environment. This service allows to deploy complex multitiered applications to your cloud infrastructure efficiently.
{: shortdesc}

For a comprehensive understanding of {{site.data.keyword.bpshort}} actions and their integration with Red Hat Ansible, see the dedicated section on [{{site.data.keyword.bpshort}} actions](/docs/schematics?topic=schematics-sc-actions). This section describes the fundamentals and key aspects of using {{site.data.keyword.bpshort}} actions for Ansible automation.

## Before you begin
{: #action-working-prereq}

Before you begin, ensure that the following requirements are set to proceed with creating the {{site.data.keyword.bpshort}} action.

- Create an Ansible playbook and store it in a GitHub or GitLab repository. Alternatively, use one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook){: external}.
- Confirm that you have the [permissions](/docs/schematics?topic=schematics-access) to create the {{site.data.keyword.bpshort}} action.

Make sure the `location` and the `url` endpoint refer to the same region when you create or update the {{site.data.keyword.bpshort}} workspaces and actions. This information is crucial for proper data storage and access. For more information about location and endpoint, see [Where is the information stored?](/docs/schematics?topic=schematics-secure-data#pi-location)
{: note}

## Creating an action by using Console
{: #create-action-working-ui}
{: ui}

Create a {{site.data.keyword.bpshort}} action and specify the Ansible playbook that you want to run against your Cloud resources.
{: shortdesc}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![Hamburger icon](images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > [**Ansible**](https://cloud.ibm.com/automation/schematics/ansible){: external} and navigate to **Create playbook**.
   - Create a playbook.
      1. Enter a playbook **Name** and a **Description** for your action. Note that the name can be up to 128 characters long and can include alphanumeric characters, spaces, dashes, and underscores.
      2. Enter the **GitHub or GitLab repository URL** where your Ansible playbook is stored. The URL can point to the master branch, any other branch, or a subdirectory. If your repository stores multiple playbooks, you must select the playbook that you want to run. A {{site.data.keyword.bpshort}} action can point to one playbook at a time. To run multiple playbooks, you must create a separate action for each playbook. The following are examples:
          - Master branch - `https://github.com/myorg/myrepo`
          - Branches - `https://github.com/myorg/myrepo/tree/mybranch`
          - Subdirectory - `https://github.com/mnorg/myrepo/tree/mybranch/mysubdirectory`

        If you don't have a playbook, try out one of your [sample playbooks](https://github.com/Cloud-Schematics?q=topic%3Aansible-playbook){: external}.
        {: tip}

      3. Optional: enter the **Branch (optional)** and **Folder (optional)**.
      4. Optional: if you want to use a private GitHub repository, enter your personal access token. The personal access token is used to authenticate private GitHub repository to access your Ansible playbook. For more information about how to create an access token, see [creating a personal access token for the CLI](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens){: external}.

      For more information, see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-general-faq#clone-file-extension) for cloning from the Git repository. To securely validate and clone the template, you can click the `Open reference picker` to select your {{site.data.keyword.secrets-manager_short}} key reference. For more information, see [creating a {{site.data.keyword.secrets-manager_short}} instance](/docs/secrets-manager?topic=secrets-manager-create-instance).
      {: note}

   - Additional information
      1. Optional: enter the **Tags** that you want to add to your action. Tags help in the quick search operation of your action.
      2. Select the **Resource group** where you want to create the action.
      3. Select the **Location** where you want to create the action. The location determines where your action runs and action data are stored. You can choose a geography, such as `Dallas`, or `Frankfurt`. If you select a geography, {{site.data.keyword.bpshort}} decides on a location within this geography based on availability. Be sure that you can store your action data in this location as you cannot change the location after the action is created. For more information, see [where is the information stored](/docs/schematics?topic=schematics-secure-data#pi-location)? Make sure that the location of your action is independent from the location of your Cloud resource where you want to run your Ansible playbook.

   - Choose an inventory
      You can define the host groups on which you want to run your Ansible playbook.

      1. Click **Create Inventory**.
      2. Enter a name for your inventory.
      3. Choose the **Connection type** as `SSH` or `WinRM` depending on your remote host requirements. The default is `SSH`.

            Connection type supports both `SSH` and `WinRM` for connecting to remote hosts. For `SSH` ensure that Bastion host access is configured. `WinRM` is used to connect with Windows remote hosts. Currently, `WinRM` supports only Windows system with the public `IPs` and does not support Bastion host access.
            {: note}

      4. Verify your `location` and select the `Resource group` where you want to create the inventory.
      5. Click **Create host group**.
      6. Enter a name for your host group and select the {{site.data.keyword.bpshort}} workspace that provisioned the target hosts that you want to add to your host group.
      7. Optional: Click **Add query** to add another condition to your query and limit the number of target hosts that are added to your host group. You can choose to identify your hosts by using the resource name or a tag. For more information, see [supported resource queries](/docs/schematics?topic=schematics-inventories-setup#supported-queries).
      8. Click **Add another condition** and repeat steps 6-8 to add more host groups.
      9. Optional: Click **Define manually** tab to define the hosts and group of hosts on which your playbook operates.
      10. Enter the host details by using [.ini file format](/docs/schematics?topic=schematics-inventories-setup#inv-file-format) in **File** option.
      11. From the table list, select the host groups that you want to include in your resource inventory.
      12. Click **Create inventory** to define an inventory file that defines the hosts and group of hosts upon which your playbook operates.
4. From the resource inventory table, select an existing resource inventory. Click the **Edit icon** against the inventory.
        - Click **Manage Credentials** to configure the inventory credentials. For more information, see [Managing credentials in Schematics Actions](/docs/schematics?topic=schematics-sch-multihost-setup#sch-multihost-credentials).
        - Click **Manage Variables** to configure the inventory credentials. For more information, see [Managing variables in Schematics Actions](/docs/schematics?topic=schematics-sch-multihost-setup#sch-multihost-variable).
        - Click **Delete** to remove the inventory.
5. Click **Create**. Your action is created with a `Normal` state, and you are directed to the **Details** section.
6. Click **Check action** to verify the tasks you are trying to run, and ensure that your resources are linked to your playbook. You can monitor the **Action history** section to view the results in the logs. This check verifies the tasks that you are trying to run, and ensure that your resources are properly linked to your playbook.

      You cannot delete or stop a running job of your {{site.data.keyword.bpshort}} action. To change your action, wait for the job to complete, and change your settings, and then click **Check action** or **Run action** again.
      {: note}

7. Click **Run action** to run the action. You can monitor the progress of an action by reviewing the logs on the **Jobs** page. Every `30 seconds` the job logs are automatically refreshed.

## Creating an action by using CLI
{: #create-action-working-cli}
{: cli}

1. From your [local command line interface](/docs/schematics?topic=schematics-setup-cli){: external} setup your CLI and {{site.data.keyword.bpshort}} plug-in.
2. Create an action by using the [ibmcloud schematics action create](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-create-action) command.
3. Create an inventory by using the [ibmcloud schematics inventory create](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-create-inv) command.
4. Check the logs to verify that the creation was successful.


## Creating an action by using API
{: #create-action-working-api}
{: api}

1. Retrieve your [IAM access token and authenticate](/docs/schematics?topic=schematics-setup-api#cs_api) with {{site.data.keyword.bpshort}} using the API.
2. Create an action by sending a [POST request](/apidocs/schematics/schematics#create-action).
3. Create an inventory by sending a [POST request](/apidocs/schematics/schematics#create-inventory).
4. Check the response status to verify that the creation was successful.

## Next steps
{: #create-action-working-nextsteps}

After setting up your {{site.data.keyword.bpshort}} Ansible action, the next step is to [update its settings](/docs/schematics?topic=schematics-action-working-update&interface=ui) as required. This might involve adjusting the playbook, inventory, credentials, or variables to accommodate changes in your environment or needs. Focus on updating these components to ensure that your action remains aligned with your current requirements.
