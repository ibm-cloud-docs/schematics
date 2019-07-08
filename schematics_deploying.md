---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-08"

keywords: schematics, automation, terraform

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

# Deploying resources to your environments
{: #deploying}

<div class="p"><div class="note attention"><span class="attentiontitle">Attention: This service is deprecated.</span> All instances of this service are deprecated. Existing instances can be used until they are no longer supported on 01 May 2018. For more information, see the [deprecation announcement blog](https://www.ibm.com/blogs/bluemix/2018/03/retirement-ibm-cloud-schematics/). For migration information, see [Migrating to the {{site.data.keyword.Bluemix_notm}} Terraform Provider](index.html#migrating).</div>
</div>

{:deprecated}

With {{site.data.keyword.bplong}}, you can deploy your latest code changes directly from your source code. When you deploy your environment, you are deploying the resources the are defined by templates or by your own Terraform configuration. You can then manage the provisioning, orchestrations, and lifecycle of the environment from within {{site.data.keyword.bpshort}}.
{:shortdesc}

You can create an environment from a template or bring your own Terraform configuration.

## Deploying to your environments with the GUI
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


## Deploying to your environments with the CLI
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

## Deploying to your environments with the API
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
  
  
  
  Updating and managing
The following recommendations can help you use Schematics to safely manage and update your resources. For more information about how to update resources, see Managing resources in your environment.

Use only Schematics to change a deployed environment
When you deploy an environment with Schematics, always use the Schematics GUI, CLI, or API to change the environment. Schematics manages the statefiles that are used to compare previous configurations to new configurations when you change your resources. If you do not use Schematics, changes you make can lead to mismatches within the Terraform files. These mismatches cause errors in your deployments. For more information about state, see the Terraform state documentation External link icon.

Run plan to check for errors and preview changes to your environment before you run apply
When you run plan, Schematics extracts the variables in your environment details and the latest version of your Terraform configuration from source control. The plan output displays how the configuration compares to what is deployed according to your Terraform state file. The output allows you to see how your changes might affect resources already running before you commit those changes.

Running plan becomes especially important when you work with configurations that teams contribute to collaboratively under version control. The plan helps you see whether the configuration file you are using might have changed since your last plan.

Use API calls to audit changes to environments
Version control systems provide you with a way to audit changes to configurations. However, if you want to audit changes that are made to an environment, you can use the Schematics REST API External link icon. The call GET /v1/environments/{id}/activities retrieves a list of all activities that occurred in a specified environment, and the call GET /v1/environments/{id}/activities/{activity_id} lists details for a specific activity that ran against an environment.

