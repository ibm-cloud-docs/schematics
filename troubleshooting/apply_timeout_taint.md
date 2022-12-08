---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-02"

keywords: schematics, schematics timeout, terraform timeout, tainted resources, untaint, taint

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do timeout failures result in tainted {{site.data.keyword.cloud_notm}} resources?
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

4. Retrieve the [Terraform state file](/docs/schematics?topic=schematics-schematics-cli-reference#state-list) for your workspace and note the name of the resource that is tainted.
    ```sh
    ibmcloud schematics state pull --id <workspace_ID> --template <template_ID>
    ```
    {: pre}

5. Verify that the [tainted resource](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-taint) is successfully provisioned and in a working correctly state by using the {{site.data.keyword.cloud_notm}} console, CLI, or API. For example, if you tried to provision an {{site.data.keyword.containerlong_notm}} cluster, check that the cluster is in a `Normal` state and that you can successfully connect to the cluster. 

6. `Untaint` the resource. Enter the name of the tainted resource that you retrieved from the state file in the `--address` parameter. For example, a cluster resource name from a state file might look like this: `ibm_container_vpc_cluster.mycluster`. 
    ```sh
    ibmcloud schematics workspace untaint --id <workspace_ID> --address <resource_name>
    ```
    {: pre}

7. Retrieve the Terraform state file for your workspace again and verify that your resource is marked as [`untainted`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-untaint). 
    ```sh
    ibmcloud schematics state pull --id <workspace_ID> --template <template_ID>
    ```
    {: pre}
