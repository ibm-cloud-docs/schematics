---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-05"

keywords: schematics, schematics action, create schematics actions, run ansible playbooks, delete schematics action, 

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Debugging {{site.data.keyword.bpshort}} apply errors 
{: #apply-errors}

Review potential errors that might occur when you run {{site.data.keyword.bpshort}} apply actions against your workspaces. 
{: shortdesc}

## Timeout failures result in tainted {{site.data.keyword.cloud_notm}} resources
{: #tainted-resources}

You attempted to create an {{site.data.keyword.cloud_notm}} resource that takes a long time to fully provision, such as {{site.data.keyword.messagehub}} or {{site.data.keyword.databases-for}}. When you run the {{site.data.keyword.bpshort}} apply action, the action fails due to timeouts results in a tainted resource.
{: tsSymptoms}

The {{site.data.keyword.terraform-provider_full_notm}} sets certain timeouts when the provisioning, update, or deletion of a resource must be completed before it is considered failed. Because some resources such as {{site.data.keyword.messagehub}} or {{site.data.keyword.databases-for}} take longer to fully provision, they might exceed these timeouts. If the provisioning cannot be completed before the timeout is reached, the {{site.data.keyword.cloud_notm}} Provider plug-in marks the provisioning process as failed and taints the resource. However, the actual provisioning of the resource is not canceled and continues in the background, which can result in a successfully provisioned resource after all. Because the resource is tainted, the resource is automatically deleted and re-created when you run the next {{site.data.keyword.bpshort}} apply action.
{: tsCauses}

To avoid that a successfully provisioned resource is deleted and re-created, you must `untaint` the resource.
{: tsResolve}

1. List the workspaces in your account and note the ID of the workspace that includes the failed resource. 
    ```sh
    ibmcloud schematics workspace list
    ```
    {: pre}

2. Refresh your workspace. A refresh action validates the {{site.data.keyword.cloud_notm}} resources in your account against the state that is stored in the Terraform state file of your workspace. This process might take a few minutes to complete.
    ```sh
    ibmcloud schematics refresh --id <workspace_ID>
    ```
    {: pre}

3. Retrieve the template ID of your workspace. To template ID is shown as a string after the `Template Variables for: <template_ID>` section of your CLI output. 
    ```sh
    ibmcloud schematics workspace get --id <workspace_ID>
    ```
    {: pre}

4. Retrieve the Terraform state file for your workspace and note the name of the resource that is [tainted](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-taint).
    ```sh
    ibmcloud schematics state pull --id <workspace_ID> --template <template_ID>
    ```
    {: pre}

5. Verify that the [tainted resource](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-taint) is successfully provisioned and in a working correctly state by using the {{site.data.keyword.cloud_notm}} console, CLI, or API. For example, if you tried to provision an {{site.data.keyword.containerlong_notm}} cluster, check that the cluster is in a `Normal` state and that you can successfully connect to the cluster. 

6. [`Untaint`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-untaint) the resource. Enter the name of the tainted resource that you retrieved from the state file in the `--address` parameter. For example, a cluster resource name from a state file might look like this: `ibm_container_vpc_cluster.mycluster`. 
    ```sh
    ibmcloud schematics workspace untaint --id <workspace_ID> --address <resource_name>
    ```
    {: pre}

7. Retrieve the Terraform state file for your workspace again and verify that your resource is marked as `untainted`. 
    ```sh
    ibmcloud schematics state pull --id <workspace_ID> --template <template_ID>
    ```
    {: pre}


## Resource group does not exist
{: #rg-not-found}

When you run an {{site.data.keyword.bplong_notm}} plan or apply action, the resource group that you try to retrieve by using the `ibm_resource_group` data source cannot be found. Following error message are received.
{: tsSymptoms}

```text
Error retrieving resource group <resource-group>: ResourceGroupDoesnotExist: Given resource Group : "<resource-group>" doesn't exist
```
{: screen}

You do not have the wanted permissions in {{site.data.keyword.iamshort}} to view the resource group. If you specified an API key in your Terraform template or in {{site.data.keyword.bpshort}}, this API key does not have the wanted permissions in IAM to view the resource group.
{: tsCauses}

Make sure that the `Viewer` permission on the resource group is assigned to you or the API key that you use. For more information, see [Adding resources to a resource group](/docs/account?topic=account-rgs#add_to_rgs).
{: tsResolve}

</br>

## 5xx errors
{: #server-errors}

When you run a {{site.data.keyword.bpshort}} apply action, the action fails with 5xx HTTP errors such as in the following example: 
{: tsSymptoms}

```text
Error: Request failed with status code: 500, ServerErrorResponse: {"incidentID":"11aa11aaaa11-IAD","code":"A0002","description":"Could not connect to a backend service. Try again later.","type":"Authentication"}
```
{: screen}

5xx HTTP errors indicate an issue with the {{site.data.keyword.cloud_notm}} service that you try to create, update, or delete, and usually cannot be resolved by the user. These issues can include networking errors, timeouts, or the service are temporarily disabled. 
{: tsCauses}

Because this error does not originate within {{site.data.keyword.bpshort}}, wait a few minutes, rerun the {{site.data.keyword.bpshort}} apply action again. If the apply action continues to fail, note the incident ID and find more detailed logs for this incident ID in your {{site.data.keyword.loganalysislong_notm}} service instance. If you cannot resolve this issue, contact support by opening a support case for the service that you want to work with. Make sure to include the incident ID. For more information, see [Using the Support Center](/docs/get-support?topic=get-support-using-avatar). 
{: tsResolve}

</br>



