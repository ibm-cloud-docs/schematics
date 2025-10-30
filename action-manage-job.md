---

copyright:
  years: 2017, 2025
lastupdated: "2025-10-30"

keywords: schematics, schematics action state,  schematics actions state, ansible state,  schematics action,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Reviewing actions job
{: #action-jobs}

Use the {{site.data.keyword.bpshort}} actions job details to find the history of all {{site.data.keyword.bpshort}} internal activities. The activities include downloading your Ansible playbook or verifying your playbook, and to see the Ansible logs for the playbook that you ran on your target hosts.
{: shortdesc}

Jobs are classified into the following categories:

- **System jobs**: These jobs represent all {{site.data.keyword.bpshort}} internal activities and checks. For example, downloading your Ansible playbook from GitHub or verifying your playbook. You can find these jobs in the **All** tab on the action's **Jobs** page.
- **User jobs**: These jobs are created when you check or run an action. You can find a summary of all user-initiated jobs when you click the **User** tab on the action's **Jobs** page.

Review the following status that can be assigned to a job:

|Status|Description|
|-----|---------|
|`ok` |The total number of target hosts where the Ansible playbook ran. |
|`changed` | The total number of target hosts that were accessed and changed. |
|`failed` |The total number of target hosts where the Ansible playbook might not have run successfully. |
|`skipped` |The total number of target hosts that were accessed but might not be updated as changes are applied to the hosts.|
|`unreachable` |The total number of target hosts that might not be found or reached. |
{: caption="Job status" caption-side="top"}

When you encounter a `DEPRECATION WARNING` message in job execution, set the input variable as `ansible_python_interpreter = auto` to supress the warning.
