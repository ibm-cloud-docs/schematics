---

copyright:
  years: 2017, 2025
lastupdated: "2025-11-03"

keywords: schematics, schematics action delete, delete schematics actions, delete ansible playbooks, delete action,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deleting {{site.data.keyword.bpshort}} action
{: #delete-ansible-actions}

If you no longer need your {{site.data.keyword.bpshort}} action, you can delete it. Deleting an action removes the Ansible playbook and your action data. However, any configurations applied to your target host by using the action are not removed.
{: shortdesc}

## Deleting an action by using Console
{: #create-action-working-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Navigate to **Menu** icon ![Hamburger icon](images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > [**Ansible**](https://cloud.ibm.com/automation/schematics/ansible){: external}
3. Click the three vertical dots to view the **Delete** option against the Ansible action that you want to delete.
4. From the **Delete ansible** popup window, type and confirm the action that you want to delete.
5. Click **Delete**

## Deleting an action by using CLI
{: #delete-action-working-cli}
{: cli}

1. From your [local command line interface](/docs/schematics?topic=schematics-setup-cli){: external} setup your CLI and {{site.data.keyword.bpshort}} plug-in.
2. Delete an action by using the [ibmcloud schematics action delete](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-delete-action) command.
3. Delete an inventory by using the [ibmcloud schematics inventory delete](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-delete-inventory) command.
4. Check the logs to verify that the creation was successful.

## Deleting an action by using API
{: #delete-action-settings-api}
{: api}

1. Retrieve your [IAM access token and authenticate](/docs/schematics?topic=schematics-setup-api#cs_api) with {{site.data.keyword.bpshort}} by using the API.
2. Delete an action by sending a [DELETE](/apidocs/schematics/schematics#delete-action) request.
3. Delete an inventory by sending a [DELETE](/apidocs/schematics/schematics#delete-inventory) request.
3. Check the response status to verify that the deletion was successful.

## Next steps
{: #action-working-update-nextsteps}

After your {{site.data.keyword.bpshort}} action or inventory delete, you can further enhance your automation workflow with the following options:

- [Creating an auto deploy button](/docs/schematics?topic=schematics-auto-deploy-url). The feature allows you to trigger your {{site.data.keyword.bpshort}} action automatically, streamlining the deployment process and reducing manual intervention.
