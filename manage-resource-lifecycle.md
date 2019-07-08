---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-08"

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


### With the CLI
{: #cli}

You can programmatically update resources in your environments with the {{site.data.keyword.bpshort}} CLI.
{:shortdesc}

After you publish code changes to your Terraform configuration in your source control repository, complete the following steps to deploy updates to your environment:

1. Run the `plan` command. The {{site.data.keyword.bpshort}} CLI pulls the updated environment configuration from source control and outputs a preview of how your resources differ against your current deployment.

  ```
  bx schematics action plan --id ENVIRONMENT_ID
  ```
  {: codeblock}

  You can run `bx schematics environment list` if you need to retrieve your environment ID.

2. View the plan output in the activity log.

  ```
  bx schematics activity log --id ACTIVITY_ID
  ```
  {: codeblock}

3. Deploy your plan by running the `apply` command.

  ```
  bx schematics action apply --id ENVIRONMENT_ID
  ```
  {: codeblock}


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

### With the CLI
{: #auditing-cli}

1. Retrieve your environment ID.

  ```
  bx schematics environment list
  ```
  {: codeblock}

2. Retrieve the activity IDs for your environment. Activity IDs are assigned to plan, apply, destroy, and delete actions. The following command lists all of the activities that ran against your environment.

  ```
  bx schematics activity list --id ENVIRONMENT_ID
  ```
  {: codeblock}

3. Retrieve data about a specific activity, such as who changed an environment and when.

  ```
  bx schematics activity show --id ACTIVITY_ID
  ```
  {: codeblock}

4. Optional: To view a detailed log, such as plan or apply output, run the `log` command.

  ```
  bx schematics activity log --id ACTIVITY_ID
  ```
  {: codeblock}

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


### With the CLI
{: #cli}

You can use the {{site.data.keyword.bpshort}} CLI to spin up and tear down temporary environments.
{:shortdesc}

1. Run the `destroy` command to tear down your environment and resources.

  ```
  bx schematics action destroy --id ENVIRONMENT_ID
  ```
  {: codeblock}

  You can run `bx schematics environment list` if you need to retrieve your environment ID.

2. Optional: Run the `delete` command if you want to remove environment metadata from the service.

  ```
  bx schematics environment delete --id ENVIRONMENT_ID
  ```
  {: codeblock}


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
