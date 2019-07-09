---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-09"

keywords: Schematics, automation, Terraform

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
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}

# Managing the lifecycle of your resources
{: #manage-lifecycle}

## Deploying your resources
{: #deploy-resources}

With {{site.data.keyword.bplong}}, you can deploy your latest code changes directly from your source code. When you deploy your environment, you are deploying the resources the are defined by templates or by your own Terraform configuration. You can then manage the provisioning, orchestrations, and lifecycle of the environment from within {{site.data.keyword.bpshort}}.
{:shortdesc}

You can create an environment from a template or bring your own Terraform configuration.

Use caution when managing critical resources
Terraform is idempotent, which means that it erases and replaces the resource when you apply changes. As such, use caution when you create resources in Schematics that contain irreplaceable data, like databases. If you do choose to put these kinds of resources into Schematics, be sure to always use plan to confirm changes. Running plans show you how resources might be replaced or deleted. Inspect plan logs thoroughly before you apply the changes. For more information about plan and apply logs, see Deployment logs and annotations.

Verify limits for services before you deploy resources
Before you deploy a resource, check whether you can create the resource without reaching the service limit for your IBM account. If the service limit is reached, the resource will not be deployed until you increase your service quota or destroy other resources from that service. Run bx resource quotas to see all resource quota definitions for your account.

### With the console
{: #deploy-resources-console}

Run your infrastructure code to provision, or modify your {{site.data.keyword.cloud_notm}} resources by using the {{site.data.keyword.cloud_notm}} Schematics console.  
{:shortdesc}

Before you begin, make sure that you [created a workspace from your GitHub repository](/docs/schematics?topic=schematics-workspace-setup#create-workspace) that hosts your Terraform configuration files. 

1. From the workspace details page, click **Run new plan** to create a Terraform execution plan. This plan equals the output of the `terraform plan` command. You can review the status of your plan in the **Recent activtity** section of your workspace details page.
2. Review the log files of your execution plan. This plan includes a summary of {{site.data.keyword.cloud_notm}} resources that must be created, modified, or deleted to achieve the state that you described in your Terraform configuration files. If you have syntax errors in your configuration files, you can review the error message in the log file.
3. Optional: Open the **Variables** tab from the workspace details page to review the variables that you set for your workspace. The values of your variables are used everytime you reference the variable in your Terraform configuration file. 
4. When you are ready, apply your Terraform configuration by clicking **Apply plan** from the **Details** tab of the workspace details page. {{site.data.keyword.cloud_notm}} Schematics starts provisioning, modifying, or deleting your {{site.data.keyword.cloud_notm}} resources based on what actions were identified in the execution plan. Depending on the type and number of resources that you want to provision or modify, this process might take a few minutes, or even up to hours to complete. During this time, you cannot make changes to your workspace. After all updates are applied, the state of your {{site.data.keyword.cloud_notm}} resources is stored in a Terraform state file that {{site.data.keyword.cloud_notm}} Schematics uses to determine what resources exist in your {{site.data.keyword.cloud_notm}} account. 
5. Review the log file to ensure that no errors occured during the provisioning, modification, or deletion process. 
6. From the workspace details page, select the **Resources** tab to find a summary of {{site.data.keyword.cloud_notm}} resources that are available in your {{site.data.keyword.cloud_notm}} account.


### With the API
{: #deploy-resources-api}

Run your infrastructure code to provision, or modify your {{site.data.keyword.cloud_notm}} resources by using the {{site.data.keyword.cloud_notm}} Schematics API.  
{:shortdesc}

Calls to the <a href="https://us-south.schematics.bluemix.net/swagger-api/">{{site.data.keyword.bpshort}} API <img src="../../icons/launch-glyph.svg" alt="External link icon"></a> point to the following base endpoint:

```
https://us-south.schematics.bluemix.net
```
{: codeblock}

Complete the following steps to deploy resources to your environment:

1. Generate an OAuth token to use in your authentication header by running a `POST` call. For more information about authenticating with the platform and services, see the <a href="https://console.bluemix.net/docs/iam/apikeys.html#manapikey" target="_blank">managing identity and access docs. <img src="../../icons/launch-glyph.svg" alt="External link icon"></a>

  ```
  curl -X POST \
    -s -u "bx:bx" \
    -H "Content-Type: application/x-www-form-urlencoded" \
    -H "Accept: application/json" \
    --data-urlencode "grant_type=password" \
    --data-urlencode "username=<IBMid>" \
    --data-urlencode "password=<IBMid password>" \
    --data-urlencode "response_type=cloud_iam" \
    --data-urlencode "bss_account=<{{site.data.keyword.cloud_notm}} account ID>" \
    http://iam.ng.bluemix.net/oidc/token | jq -r .access_token
  ```
  {: codeblock}

  If you generated an API key for your {{site.data.keyword.cloud_notm}} account ID, the following call also generates an OAuth token.

  ```
  curl -X POST \
  -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&response_type=cloud_iam&apikey=<API key>" \
  https://iam.bluemix.net/oidc/token | jq -r .access_token
  ```
  {: codeblock}




  Note the `id` value that is returned. The `id` is a unique identifier that is assigned to your environment and is used in subsequent calls.

3. Run the `POST v1/environments/<environment_ID>/plan` call to preview which resources will be deployed to your environment when you apply your configuration. Plans extract the code in the master branch of your repository.

  ```
  curl -X POST \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/plan \
    -H 'accept: application/json' \
    -H 'authorization: bearer <OAuth_token>' \
    -H 'content-type: application/json'
  ```
  {: codeblock}

  A successful response returns an `activityid`. The activity ID value is needed to view logs, such as the plan and apply output from Terraform.
  ```
  {
    "activityid": "<activity_ID>"
  }
  ```
  {: screen}

4. Run the `GET v1/environments/<environment_ID>/activities/<activity_ID>/log` call to view the plan output. For more information about how to read the output, see [deployment logs and annotations](schematics_logs.html).

  ```
  curl -X GET \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/activities/<activity_ID>/log \
    -H 'accept: text/html' \
    -H 'authorization: bearer <OAuth_token>'
  ```
  {: codeblock}

5. Deploy your environment by running the `PUT v1/environments/<environment_ID>/apply/` call.

  ```
  curl -X PUT \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/apply \
    -H 'accept: application/json' \
    -H 'authorization: bearer <OAuth_token>' \
    -H 'content-type: application/json'
  ```
  {: codeblock}

6. Monitor the deployment by running the `GET v1/environments/<environment_ID>/activities/<activity_ID>/log` call to view the apply output.

  ```
  curl -X GET \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/activities/<activity_ID>/log \
    -H 'accept: text/html' \
    -H 'authorization: bearer <OAuth_token>'
  ```
  {: codeblock}

  A successful deployment returns the following line towards the end of the output.

  ```
  Apply complete! Resources: N added, N changed, N destroyed.
  ```
  {: screen}



## Updating your resources

Deploying changes to your environment is a lightweight process. To change which resources are allocated, you code changes to your Terraform configuration in declarative syntax, meaning you state only the outcome you want. With {{site.data.keyword.bpshort}}, you can preview your changes before deployment.


### With the console
{: #gui}

If you prefer a graphical view to manage the resources in your environments, you can use the {{site.data.keyword.bpshort}} dashboard.
{:shortdesc}

After you publish code changes to your Terraform configuration in your source control repository, or modify variables in the GUI, complete the following steps to deploy updates to your environment:

1. In the **{{site.data.keyword.bpshort}}** dashboard, select **Environments**.

2. Click the row of the specific environment that you want to update.

3. Click **Plan** and inspect your plan log for errors.

4. Click **Apply** to deploy updates.


### With the API
{: #api}

You can programmatically manage resources in your environments with the {{site.data.keyword.bpshort}} API.
{:shortdesc}

After you publish code changes to your Terraform configuration in your source control repository, complete the following steps to deploy updates to your environment:

1. Run the `POST v1/environments/<environment_ID>/plan` call. The {{site.data.keyword.bpshort}} API pulls the updated environment configuration from source control and compares it with the Terraform state file to show how your resources differ against your current deployment.

  ```
  curl -X POST \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/plan \
    -H 'accept: application/json' \
    -H 'authorization: bearer <OAuth_token>' \
    -H 'content-type: application/json'
  ```
  {: codeblock}

  Example response:
  ```
  {
    "activityid": "<activity_ID>"
  }
  ```
  {: screen}

2. View the plan output in the activity log.

  ```
  curl -X GET \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/activities/<activity_ID>/log \
    -H 'accept: text/html' \
    -H 'authorization: bearer <OAuth_token>'
  ```
  {: codeblock}

3. Deploy your plan by running the `PUT /v1/environments/<environment_ID>/apply` call.

  ```
  curl -X PUT \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/apply \
    -H 'accept: application/json' \
    -H 'authorization: bearer <OAuth_token>' \
    -H 'content-type: application/json' \
  ```
  {: codeblock}
  
## Reviewing workspace changes
{: #review-logs}

You can view the change history of Terraform configurations in your source control repository, like you would with any other codebase. To monitor resource deployments to your environments, or to view logs of previous deployments, you can view audit logs with the {{site.data.keyword.bpshort}} GUI, CLI, and API.
{:shortdesc}

### With the console
{: #auditing-gui}

You can look at the environment status indicators for a quick glance at which environments are actively running resources.

To inspect resource or deployment details:

1. In the <a href="https://console.bluemix.net/schematics" target="_blank">{{site.data.keyword.bpshort}} dashboard</a>, select **Environments** from the left navigation menu.

2. Select the row of the environment to access the environment details page.

3. You can inspect the following:
  * Historical logs of previous plans and deployments can be found in the **Recent Activity** section. For more information about logs, see [deployment logs and annotations](schematics_logs.html).
  * The **Resources** tab shows which resources are actively running in your environment.

### With the API
{: #auditing-api}

1. Retrieve your environment ID.

  ```
  curl -X GET \
    https://us-south.schematics.bluemix.net/v1/environments \
    -H 'accept: application/json' \
    -H 'authorization: bearer <OAuth_token>'
  ```
  {: codeblock}

  The response body contains all environments in your {{site.data.keyword.cloud_notm}} account. Locate the `id` value of the specific environment that you want to audit.

2. Retrieve the activity IDs for the actions run against your environment.

  ```
  curl -X GET \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/activities \
    -H 'accept: application/json' \
    -H 'authorization: bearer <OAuth_token>'
  ```
  {: codeblock}

3. Retrieve a specific activity for an environment.

  ```
  curl -X GET \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/activities/<activity_ID> \
    -H 'accept: application/json' \
    -H 'authorization: bearer <OAuth_token>'
  ```
  {: codeblock}

4. Inspect a detailed log of the Terraform output.

  ```
  curl -X GET \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/activities/<activity_ID>/log \
    -H 'accept: text/html' \
    -H 'authorization: bearer <OAuth_token>'
  ```
  {: codeblock}
  
  
## Removing your resources
{: #destroy-resources}

You can use {{site.data.keyword.bplong}} to destroy your resources. When you destroy your resources, you are tearing down your environment and all associated resources.  
{:shortdesc}

**Warning**: When you destroy resources, the action cannot be reversed. Destroying resources isn't recommended for production environments, but might be useful for temporary environments such as testing or QA.

Before you destroy your resources, consider the following guidelines:
* Ensure that traffic is not directed to your resources.
* Ensure that any data you might need is backed up.


### With the console
{: #gui}

You can use the {{site.data.keyword.bpshort}} dashboard to spin up and tear down temporary environments.
{:shortdesc}

1. In the **{{site.data.keyword.bpshort}}** dashboard, select the **Environments** tab.

2. Click the row of the specific environment that you want to remove.

3. In the options menu, select **Destroy resources** or **Delete environment**. Both the delete and destroy actions are not reversible. To determine which option to select, consider whether you have active resources.
  * Select **Destroy resources** if you want to stop and remove your active resources. It is recommended that you inspect the resources list before you run a destructive action.
  * Select **Delete environment** if you want to remove the environment from the {{site.data.keyword.bpshort}} service. You can typically delete the environment if it is not deployed and does not have active resources. If you delete an environment with active resources, you can no longer use the {{site.data.keyword.bpshort}} service to track or deploy changes to your environment. The active resources need to be managed individually from their service dashboards.

  Follow the on-screen prompts to confirm your choice.

### With the API
{: #api}

You can use the {{site.data.keyword.bpshort}} API to spin up and tear down temporary environments.
{:shortdesc}

1. Run the `PUT v1/environments/<environment_ID>/destroy` call to destroy your resources.

  ```
  curl -X PUT \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/destroy \
    -H 'accept: application/json' \
    -H 'authorization: bearer <OAuth_token>' \
    -H 'content-type: application/json'
  ```
  {: codeblock}

2. Optional: Run the `DELETE v1/environments/<environment_ID>` call if you want to remove environment metadata from the {{site.data.keyword.bpshort}} service.

  ```
  curl -X DELETE \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID> \
    -H 'accept: text/plain' \
    -H 'authorization: bearer <OAuth_token>'
  ```
  {: codeblock}
