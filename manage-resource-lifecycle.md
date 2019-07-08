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

## Deploying your resources
{: #deploy-resources}

With {{site.data.keyword.bplong}}, you can deploy your latest code changes directly from your source code. When you deploy your environment, you are deploying the resources the are defined by templates or by your own Terraform configuration. You can then manage the provisioning, orchestrations, and lifecycle of the environment from within {{site.data.keyword.bpshort}}.
{:shortdesc}

You can create an environment from a template or bring your own Terraform configuration.

### With the console
{: #gui}

If you prefer a graphical view to deploy resources, you can use the {{site.data.keyword.bpshort}} dashboard.
{:shortdesc}


### Prerequisite
{: #gui-prereq}

* If you are bringing your own Terraform configuration, store the Terraform files in source control.

In the {{site.data.keyword.bpshort}} dashboard:

1. Create an environment from a template or from your own Terraform configuration.
  * If you are using a template, go to the template library and click **Create** on the template that you want to use as your source.
  * If you are using your own Terraform configuration, select **Environments** and click **Create Environment**.

2. Add the source control repository where your Terraform configuration is stored. If you are creating an environment from a private repository, or from a GitHub or GitLab Enterprise repository, you will be prompted to add a personal access token to allow {{site.data.keyword.bpshort}} to gain read-access to the repository. The token is added as a variable named `SCHEMATICSGITTOKEN` for the environment.

3. Optional: Add values or override default values that are assigned to variables.

  * For variables with a `string` type, you can pass the string in plain text in the **Values** field.

  For example, if you have the following variable block in your Terraform file, you can add a value like `dal10` in the GUI.

  ```
  variable "datacenter" {
    type        = "string"
    description = "The data center that you want to deploy your Kubernetes cluster in."
  }
  ```
  {:screen}

  * For variables with a `list` type, you can pass the value with the following syntax.

  ```
  [
   "list_element_1", "list_element_2" list_element_3"
  ]
  ```
  {:codeblock}

  * For variables with a `map` type, you can pass the value with the following syntax.

  ```
  {
   key_1 = "value_1",
   key_2 = "value_2"
  }
  ```
  {:codeblock}

4. Click **Plan** to preview what resources are provisioned when you deploy your environment. When you run plan, {{site.data.keyword.bpshort}} extracts the variables in your environment details and the latest version of your Terraform configuration from source control. The plan output displays how the configuration compares to what is deployed according to your Terraform state file. Your environment is locked from more changes until the plan is applied to your environment or canceled.

5. View the log in the **Recent Activity** section to inspect the plan output. The plan output shows if errors exist and which resources the service is planning to create, change, or destroy. For more information about how to read the output, see [deployment logs and annotations](schematics_logs.html).

6. When you are ready to implement your plan, click **Apply**. You can monitor the activity log to ensure that the plan is successfully applied in the latest run.


### With the CLI
{: #cli}

You can use the {{site.data.keyword.bpshort}} plug-in for the {{site.data.keyword.cloud_notm}} CLI to deploy updates to your environment.
{:shortdesc}

Check out the [CLI reference](schematics_reference.html) to see all available commands for the {{site.data.keyword.bpshort}} plug-in.

### Prerequisites
{: #cli-prereq}

* Store a Terraform configuration in source control.
* If you haven't already, install the {{site.data.keyword.cloud_notm}} CLI for your operating system from the [CLI repository](https://clis.ng.bluemix.net/ui/home.html).

After you log in to the {{site.data.keyword.cloud_notm}} CLI:

1. Install the {{site.data.keyword.bpshort}} CLI plug-in by running the following command.

  ```
  bx plugin install schematics -r Bluemix
  ```
  {: codeblock}

  The {{site.data.keyword.bpshort}} plug-in appears under `bx plugin list` if the installation is successful.

2. Create an environment from your configuration. When you create your environment, you are pointing the service to your source control so that it can extract the latest code.

  a. Create a JSON file to pass metadata that describes your environment, including the source control repository where your Terraform files are stored. For Terraform files stored in a private repository, or in GitHub or GitLab Enterprise, you can grant read-access to {{site.data.keyword.bpshort}} by adding a variable with the name `SCHEMATICSGITTOKEN` and a Git personal access token as the value. Set the variable to `"secure": true` so that it is hidden from view for other users in your {{site.data.keyword.cloud_notm}} account.

  The following JSON sample shows all available parameters.

  ```
  {
    "description": "Optional description of the environment",
    "frozen": false,
    "name": "Name of the environment",
    "sourceurl": "The URL for the source control repository where your Terraform configuration is stored",
    "tags": [
      "optional_tag_1",
      "optional_tag_2"
    ],
    "variablestore": [
      {
        "name": "visible_string_variable",
        "secure": false,
        "value": "Visible value"
      },
      {
        "name": "secret_string_variable",
        "secure": true,
        "value": "Secured value"
      },
      {
        "name": "visible_map_variable",
        "secure": false,
        "type": "map",
        "value": "{ \"key_1\" = \"value_1\", \"key_2\" = \"value_2\", \"key_3\" = \"value_3\" }"
      },
      {
        "name": "visible_list_variable",
        "secure": false,
        "type": "list",
        "value": "[ \"value_1\", \"value_2\", \"value_3\" ]"
      }
    ]
  }
  ```
  {: codeblock}

  b. Run the `create` command to add the environment to {{site.data.keyword.bpshort}}.

  ```
  bx schematics environment create --file FILE_NAME
  ```
  {: codeblock}

  <table summary="Description of the create command.">
  <caption>Table 1. Description of the create command.
  </caption>
  <thead>
  <th colspan="1">Parameter</th>
  <th colspan="1">Description</th>
  </thead>
  <tbody>
  <tr>
  <td>--file FILE_NAME</td>
  <td>The JSON file that you created in the previous step to describe your environment.
  </td>
  </tbody></table>

  Note the `ID` value that is returned. The `ID` is a unique identifier that is assigned to your environment and is used in subsequent commands.

3. Run the `plan` command to preview how your environment would change. The plan command shows which resources would change and by what quantity compared to what is deployed, but the changes do not take effect until you run the `apply` command.

  ```
  bx schematics action plan --id ENVIRONMENT_ID
  ```
  {: codeblock}

  <table summary="Description of the plan command.">
  <caption>Table 2. Description of the plan command.
  </caption>
  <thead>
  <th colspan="1">Parameter</th>
  <th colspan="1">Description</th>
  </thead>
  <tbody>
  <tr>
  <td>--id ENVIRONMENT_ID</td>
  <td>The specific environment that you want to run an execution plan for. You can run <code>bx schematics list</code> if you need to retrieve the value for environment ID.
  </td>
  </tbody></table>

  An `activity_id` is assigned to the plan and recorded in the activity log.

4. Retrieve the activity log to view the Terraform plan output. For more information about how to read the output, see [deployment logs and annotations](schematics_logs.html).

  ```
  bx schematics activity log --id ACTIVITY_ID
  ```
  {: codeblock}

  <table summary="Description of the log command.">
  <caption>Table 3. Description of the log command.
  </caption>
  <thead>
  <th colspan="1">Parameter</th>
  <th colspan="1">Description</th>
  </thead>
  <tbody>
  <tr>
  <td>--id ACTIVITY_ID</td>
  <td>The specific activity that you want to view the log for. You can run <code>bx schematics activity list --id ENVIRONMENT_ID</code> if you need to retrieve the value for activity ID.
  </td>
  </tbody></table>

5. Run the `apply` command to deploy resources to your environment.

  ```
  bx schematics action apply --id ENVIRONMENT_ID
  ```
  {: codeblock}

6. Monitor the apply output to ensure that the deployment is successful.

  ```
  bx schematics activity log --id ACTIVITY_ID
  ```
  {: codeblock}

  A successful deployment returns the following line towards the end of the output.

  ```
  Apply complete! Resources: N added, N changed, N destroyed.
  ```
  {: screen}

### With the API
{: #api}

You can programmatically deploy your environment with the {{site.data.keyword.bpshort}} API.
{:shortdesc}

Calls to the <a href="https://us-south.schematics.bluemix.net/swagger-api/">{{site.data.keyword.bpshort}} API <img src="../../icons/launch-glyph.svg" alt="External link icon"></a> point to the following base endpoint:

```
https://us-south.schematics.bluemix.net
```
{: codeblock}

### Prerequisite
{: #api-prereq}

* Store a Terraform configuration in source control.

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

2. Create an environment by running a `POST v1/environments` call.

  ```
  curl -X POST \
    https://us-south.schematics.bluemix.net/v1/environments \
    -H 'accept: text/plain' \
    -H 'authorization: bearer <OAuth_token>' \
    -H 'content-type: application/json' \
    -d '{
    "description": "(Optional) Description of the environment",
    "frozen": false,
    "name": "Name of the environment",
    "sourceurl": "The URL for the source control repository where your Terraform configuration is stored",
    "tags": [
      "(Optional) metadata_tag_1",
      "(Optional) metadata_tag_2"
    ],
    "variablestore": [{
        "name": "(Optional) variable_1",
        "secure": false,
        "value": "Visible value"
    },
    {
        "name": "(Optional) variable_2_secret",
        "secure": true,
        "value": "Secured value"
    }]
  }'
  ```
  {: codeblock}

  If your Terraform files are located in a private repository or GitHub and GitLab Enterprise, add `SCHEMATICSGITTOKEN` to your variable store with a Git personal access token as the value. Set the variable to `"secure": true` so that it is hidden from view.

  A successful response returns the following output.
  ```
  {
    "id": "Environment ID",
    "name": "Environment name",
    "description": "Description of the environment",
    "account": "{{site.data.keyword.cloud_notm}} account ID",
    "owner": "The IBMid of the user who created the environment",
    "creationtime": "YYYY-MM-DDTHH:MM:SSZ",
    "status": "CREATED",
    "frozen": false,
    "sourceurl": "The URL for the source control repository where your Terraform configuration is stored",
    "statefile": "The reference to the Terraform state file",
    "tags": [
      "metadata_tag_1",
      "metadata_tag_2"
    ],
    "variablestore": [
      {
        "name": "(Optional) variable_1",
        "value": "Visible value"
      }
    ]
  }
  ```
  {: screen}

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
