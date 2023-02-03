---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-03"

keywords: schematics objects, delete schematics objects,  schematics object backup

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deleting {{site.data.keyword.bpshort}} data
{: #delete-schematics-data-intro}

{{site.data.keyword.bplong}} stores your data in a highly available and secure environment. All your data such as automation code, input configuration data, input credentials, and the runtime data are stored in {{site.data.keyword.cos_short}}. This data-at-rest is encrypted using AES GCM 256 with an envelope encryption technique. As the {{site.data.keyword.cloud_notm}} account owner, you can control access to the {{site.data.keyword.bplong}} objects in your account. You can choose to delete your data, by using the {{site.data.keyword.bpshort}} API, UI, or CLI as described. 

When you delete these {{site.data.keyword.bpshort}} objects, the corresponding data in {{site.data.keyword.cos_short}} is marked for deletion. The soft delete option enable you to recover the {{site.data.keyword.bpshort}} data by raising a [support ticket](/docs/schematics?topic=schematics-schematics-help). Further, your data will automatically be purged after 7 days of soft delete.  

Also, {{site.data.keyword.bpshort}} service maintains a backup copy of your data in a separate {{site.data.keyword.cos_short}} bucket. This backup copy is automatically overwritten, every 30 days. Hence, all backup copy of your data will be purged in 30 days.



## Scope
{: #delete-schematics-data-scope}

The scope of deletion is limited to data stored in {{site.data.keyword.bpshort}} service and its backup.

Following points does not include in the scope of deletion of {{site.data.keyword.bpshort}} objects.
- The deletion or removal of the {{site.data.keyword.cloud_notm}} resources that were provisioned by using {{site.data.keyword.bpshort}}.
- The data stored by the {{site.data.keyword.cloud_notm}} resources provisioned by the {{site.data.keyword.bpshort}}.  
- At the time of {{site.data.keyword.bpshort}} object deletion, you will be provided with the option to destroy the Cloud resources. However, it is suggested to independently confirm and destroy the Cloud resources from the Cloud Services. You can explore the list of Cloud resources, from the [{{site.data.keyword.cloud_notm}} resource list](https://cloud.ibm.com/resources){: external} page.

## Deleting {{site.data.keyword.bpshort}} objects from UI
{: #delete-schematics-data-ui}
{: ui}

You can delete {{site.data.keyword.bpshort}} objects by using {{site.data.keyword.cloud_notm}} console. Following {{site.data.keyword.bpshort}} objects are used to store your data and helps to delete the {{site.data.keyword.bplong}} objects.

You must have [Manager role](/docs/schematics?topic=schematics-access#access-roles) to delete from these {{site.data.keyword.bplong}} objects.
{: important}

### Workspaces
{: #delete-schematics-data-wkscategory}

You can follow these steps to delete the {{site.data.keyword.bpshort}} objects by using {{site.data.keyword.cloud_notm}} console.

1. From the [{{site.data.keyword.bpshort}} Workspaces dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that you want to delete.
2. Click **actions** tab and select **Delete workspace** option.
3. Type your workspace name in **Type `workspace_name` to confirm** text box.
4. Click **Delete** button.

### Actions
{: #delete-schematics-data-actionscategory}

You can follow these steps to delete the {{site.data.keyword.bpshort}} objects by using {{site.data.keyword.cloud_notm}} Console.

1. From the [{{site.data.keyword.bpshort}} Workspaces dashboard](https://cloud.ibm.com/schematics/actions){: external}, select the actions that you want to delete.
2. Click Select the **Actions** drop down list and select **Delete** option.
3. Type your actions name in **Type `action_name` to confirm** text box.
4. Click **Delete** button.

### Inventories
{: #delete-schematics-data-invcategory}

You can follow these steps to delete the {{site.data.keyword.bpshort}} objects by using {{site.data.keyword.cloud_notm}} Console.

1. From the [{{site.data.keyword.bpshort}} Workspaces dashboard](https://cloud.ibm.com/schematics/inventories){: external}, select the inventories that you want to delete.
2. Click `...` dots against the inventory you want to delete and click **Delete**.
3. Type your inventory name in **Type `Inventory_name` to confirm** text box.
4. Click **Delete** button.

## Deleting {{site.data.keyword.bpshort}} objects from CLI
{: #delete-schematics-data-cli}
{: cli}

You can delete {{site.data.keyword.bpshort}} objects by using command-line. Following {{site.data.keyword.bpshort}} objects are used to store your data and helps to delete the {{site.data.keyword.bplong}} objects.

You must have [Manager role](/docs/schematics?topic=schematics-access#access-roles) to delete from these {{site.data.keyword.bplong}} objects.
{: important}

### Workspaces
{: #delete-schematics-data-cliwks}

You can follow these steps to delete the {{site.data.keyword.bpshort}} objects.

1. Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli) and install the [{{site.data.keyword.bplong}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin)
2. Run `ibmcloud schematics workspace list [--limit LIMIT] [--offset OFFSET] [--output] [--region] [--json]` to list and select the workspace ID that you want to delete. For more information about listing the workspace, see [{{site.data.keyword.bpshort}} Workspaces list](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-list) command.
3. Run `ibmcloud schematics workspace delete --id WORKSPACE_ID [--force]` to delete the workspace. For more information about workspace delete, see [{{site.data.keyword.bpshort}} Workspaces delete](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-delete) command.

### Actions
{: #delete-schematics-data-cliactions}

You can follow these steps to delete the {{site.data.keyword.bpshort}} objects.

1. Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli) and install the [{{site.data.keyword.bplong}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin)
2. Run `ibmcloud schematics action list` to list and select an action ID that you want to delete. For more information about listing the actions, see [{{site.data.keyword.bpshort}} Actions list](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-list-action) command.
3. Run `ibmcloud schematics action delete --id ACTION_ID [--force]` to delete an action. For more information about {{site.data.keyword.bpshort}} Actions delete, see [{{site.data.keyword.bpshort}} Actions delete](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-delete-action) command.


### Inventories
{: #delete-schematics-data-cliinvcategory}

You can follow these steps to delete the {{site.data.keyword.bpshort}} objects.

1. Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli) and install the [{{site.data.keyword.bplong}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin)
2. Run `ibmcloud schematics inventory list [--limit LIMIT] [--offset OFFSET] [--output OUTPUT]` to list and select the inventory ID that you want to delete. For more information about listing the inventories, see [{{site.data.keyword.bpshort}} inventory list](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-list-inv) command.
3. Run `ibmcloud schematics inventory delete --id ACTION_ID` to delete an inventory. For more information about {{site.data.keyword.bpshort}} inventory delete, see [{{site.data.keyword.bpshort}} inventory delete](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-delete-inventory) command.

## Deleting {{site.data.keyword.bpshort}} objects from API
{: #delete-schematics-data-api}
{: api}

You can delete {{site.data.keyword.bpshort}} objects by using API. Following {{site.data.keyword.bpshort}} objects are used to store your data and helps to delete the {{site.data.keyword.bplong}} objects.

You must have [Manager role](/docs/schematics?topic=schematics-access#access-roles) to delete from these {{site.data.keyword.bplong}} objects. And select the right {{site.data.keyword.bpshort}} endpoint to call the delete API.
{: important}

### Workspaces
{: #delete-schematics-data-apiwks}

You can follow these steps to delete the {{site.data.keyword.bpshort}} objects.

1. [Set up your REST client](/docs/schematics?topic=schematics-setup-api&interface=api#cs_api) to execute {{site.data.keyword.bpshort}} API.
2. Run `curl -X GET https://schematics.cloud.ibm.com/v1/workspaces -H "Authorization: <iam_token>"` to list and select the workspace ID that you want to delete. For more information about listing the workspace, see [{{site.data.keyword.bpshort}} Workspaces list](/apidocs/schematics/schematics#list-workspaces) API.
3. Run `curl -X DELETE https://schematics.cloud.ibm.com/v1/workspaces/{id} -H "Authorization: <iam_token>"` to delete the workspace. For more information about workspace delete, see [{{site.data.keyword.bpshort}} Workspaces delete](/apidocs/schematics/schematics#delete-workspace) API.

### Actions
{: #delete-schematics-data-apiactions}

You can follow these steps to delete the {{site.data.keyword.bpshort}} objects.

1. [Set up your REST client](/docs/schematics?topic=schematics-setup-api&interface=api#cs_api) to execute {{site.data.keyword.bpshort}} API.
2. Run `curl --location --request GET https://schematics.cloud.ibm.com/v2/actions/actions --header "Authorization:  <access_token>"` to list and select an action ID that you want to delete. For more information about listing the actions, see [{{site.data.keyword.bpshort}} Actions list](/apidocs/schematics/schematics#list-actions) API.
3. Run `curl --location --request DELETE https://schematics.cloud.ibm.com/v2/actions/{action_id} --header "Authorization:  <access_token> "` to delete an action. For more information about {{site.data.keyword.bpshort}} Actions delete, see [{{site.data.keyword.bpshort}} Actions delete](/apidocs/schematics/schematics#delete-action) API.


### Inventories
{: #delete-schematics-data-apicategory}

You can follow these steps to delete the {{site.data.keyword.bpshort}} objects.

1. [Set up your REST client](/docs/schematics?topic=schematics-setup-api&interface=api#cs_api) to execute {{site.data.keyword.bpshort}} API.
2. Run `curl --location --request GET https://schematics.cloud.ibm.com/v2/inventories --header "Content-Type: application/json" --header "Authorization: <access_token> " --data-raw "{"name": "dev-inventory538","description": "My dev env inventory","location": "us-east","resource_group": "Default",,"inventories_ini": "[windows] \n 158.177.7.181"}` to list and select the inventory ID that you want to delete. For more information about listing the inventories, see [{{site.data.keyword.bpshort}} inventory list](/apidocs/schematics/schematics#list-inventories) API.
3. Run `curl --location --request DELETE https://schematics.cloud.ibm.com/v2/inventories/us-east.INVENTORY.dev-inventory523.244223cf/  --header "Content-Type: application/json" --header "Authorization: <access_token> " --data-raw "{"name": "dev-inventory538","description": "My dev env inventory","location": "us-east","resource_group": "Default","resource_queries": ["default.RESOURCEQUERY.string.dxxx8a47"]}` to delete an inventory. For more information about {{site.data.keyword.bpshort}} inventory delete, see [{{site.data.keyword.bpshort}} inventory delete](/apidocs/schematics/schematics#delete-inventory) API.
