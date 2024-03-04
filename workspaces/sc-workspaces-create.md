---

copyright:
  years: 2017, 2024
lastupdated: "2024-02-29"

keywords: schematics workspaces, workspaces, schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Creating workspaces and importing your Terraform template
{: #sch-create-wks}

Use a {{site.data.keyword.bpshort}} to manage your {{site.data.keyword.bplong_notm}} resources using Terraform. Workspace settings define the Terraform template hosted in a Git repository to be used, along with any input variables to customize the template. 
{: shortdesc} 

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

{{site.data.keyword.bplong_notm}} deprecates creation of workspace using the {{site.data.keyword.terraform-provider_full_notm}} v1.2, v1.3, v1.4 template from 2nd week of April 2024.
{: important}

## Before you begin
{: #prerequisites-create}

- [Create a Terraform configuration](/docs/schematics?topic=schematics-create-tf-config), and store the configuration in a `GitHub`, `GitLab`, or `Bitbucket` repository. You can also upload a tape archive file (`.tar`) from your local workstation to provide your template to {{site.data.keyword.bplong_notm}}. For more information, see the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command and see the [upload a `tar` file to your workspace](/apidocs/schematics/schematics#template-repo-upload) API. 
- Make sure that you have the [permissions](/docs/schematics?topic=schematics-access) to create a workspace. 

Ensure the `location` and the `url` endpoint are pointing to the same region when you create or update the {{site.data.keyword.bpshort}} workspaces and actions. For more information about location and endpoint, see [Where is your information stored](/docs/schematics?topic=schematics-secure-data#pi-location)?
{: note}

## Creating a workspace using the UI
{: #create-wks-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Access **Schematics** > **Workspaces** > [**Create workspace**](https://cloud.ibm.com/schematics/workspaces/create){: external}.
    - In **Specify Template** section:
        - **GitHub, GitLab, or `Bitbucket` repository URL** - `<provide your Terraform template Git repository URL`.
        - **Personal access token** - `<leave it blank>`.
        - Terraform Version - `terraform_v1.0`. You need to select Terraform version 1.0 or greater version. For example, if your Terraform templates are created by using Terraform v1.0, select the `Terraform version` parameter as **terraform_v1.0**. 
          You can select `Terraform_v1.1` to use Terraform version 1.1, `terraform_v1.0` to use Terraform version 1.0. When you specify `terraform_v1.1`means that users can have template that is of Terraform `v1.1.0`, `v1.1.1`, or `v1.1.2`, so on. {{site.data.keyword.bpshort}} supports `Terraform_v1.x` and also plans to make releases available after `30  to 45 days` of HashiCorp Configuration Language (HCL) release.
          {: note}

          {{site.data.keyword.bpshort}} supports the current release of `Terraform v1.1`, through `terraform_v1.0`. The Terraform template must use the version constraint, such as `>` or `>=` or `~>` for the `required_version` of Terraform, to automatically pick the current version.

          ```terraform
          terraform {
          required_version = "~> 1.1"
          }
          ```
          {: pre}

        - Click `Next`.
    - In **Workspace details** section. Enter a name for your `workspace name`. The name can be up to 128 characters long and can include alphanumeric characters, spaces, dashes, and underscores.
        - **Workspace name** as `schematics-agent-service`.
        - **Tags** as `my-tags`. Optional: Enter tags for your workspace. You can use the tags later to find your workspace faster.
        - **Resource group** as `default` or other resource group for this workspace. 
        - **Location** as `North America` or other [region](/docs/schematics?topic=schematics-multi-region-deployment) for this workspace. Decide where you want to create your workspace? The location determines where your {{site.data.keyword.bpshort}} jobs run?, and where your workspace data is stored? You can choose between a location, such as North America, or a metro city, such as Frankfurt or London. If you select a location, {{site.data.keyword.bpshort}} determines the location based on availability. If you select a metro city, your workspace is created in this location. For more information about where your data is stored, see [Where is your information stored](/docs/schematics?topic=schematics-secure-data#pi-location)? The location that you choose is independent from the region or regions where you want to provision your {{site.data.keyword.cloud_notm}} resources. The console does not support all available locations. To create the workspace in a different location, use the [CLI](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) or [API](/apidocs/schematics/schematics#create-workspace) instead.
        - Optional, enter a descriptive name for your workspace.
        - Click `Next`.
    - Click `Create`. Your workspace is created with a **Draft** state and the workspace **Settings** page opens.

### Importing your Terraform template
{: #import-template}
{: ui}

If you want to upload a tape archive file (`.tar`) instead of importing your workspace to a Git repository, you must use the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command and see the [upload a tar file to your workspace](/apidocs/schematics/schematics#template-repo-upload) API. 
{: tip}

1. On the workspace **Settings** page, enter the edit icon to edit your `Repository URL`. The link can point to the `master` branch, any other branch, or a subdirectory. 
    - Example for `master` branch: `https://github.com/myorg/myrepo`
    - Example for other branches: `https://github.com/myorg/myrepo/tree/mybranch`
    - Example for subdirectory: `https://github.com/mnorg/myrepo/tree/mybranch/mysubdirectory`      
2. If you want to use a private Git repository, enter your personal access token. The personal access token is used to authenticate with your Git repository to access your Terraform template. For more information, see [Creating a personal access token for the command-line](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens). If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-general-faq#clone-file-extension) for cloning.
3. Select the `Terraform version` that your Terraform configuration files are written in.
4. Click the checkbox `I understand the changes that could happen if I edit this URL and I agree to these happening` option.
5. Click **Save**. {{site.data.keyword.bplong_notm}} automatically downloads the Terraform configuration files from your repository, scans them for syntax errors, and retrieves all input variables that you declared in your configuration files. When all configuration files are downloaded successfully and no syntax errors are found, the workspace state changes to **Inactive**.

    After your Terraform configuration files are scanned, you can view the results on the workspace **Activity** page. The total number of files that were scanned in the source repository is displayed as `scanned`. The total number of files that are vulnerable, such as unsupported file extensions, is displayed as `discarded`. Click **Jobs** to find the details of the files that were scanned and discarded. For more information about viewing logs, see [Reviewing the {{site.data.keyword.bpshort}} job details](/docs/schematics?topic=schematics-interrupt-job#sch-job-logs).
    {: tip}

6. Review the default input variable values for your Terraform template. To change an input variable value, click **Edit** from the Workspace actions menu. Depending on the data type that your variable uses, you must enter the value in a specific format. see the following table to find example values for each supported data type. 

    | Type | Example |
    | --- | -- |
    | `number` | 4.56 |
    | `string` | example value |
    | `bool` | false |
    | `map(string)` | {key1 = "value1", key2 = "value2"} |
    | `set(string)` | ["hello", "he"] |
    | `map(number)` | {internal = 8080, external = 2020} |
    | `list(string)` | ["us-south", "eu-gb"] |
    | `list` | ["value", 30] |
    | `list(list(string))` | :[{internal = 8300 external = 8300 protocol = `"tcp"`},{internal = 8301 external = 8301 protocol = `"ldp"` } ] : list(object({internal = number external = number protocol = string})) : [{internal = 8300 external = 8300 protocol = `"tcp"`} {internal = 8301 external = 8301 protocol = `"ldp"`}]|
    {: caption="Input variables and its sample values" caption-side="bottom"}

### Using Terraform templates in {{site.data.keyword.cloud_notm}}
{: #run-template}
{: ui}

You can [Manage {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start creating, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.

## Creating a workspace using the CLI
{: #create-wks-cli}
{: cli}

1. Create a JSON file on your local workstation and add your workspace configuration. For more configuration options when creating the workspace, see the [`ibmcloud schematics workspace new` command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new).

    To create a workspace with Agent, refer to [ibmcloud schematics workspace new with Agent](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) command.
    {: note}

    ```json
    {
    "name": "<workspace_name>",
    "type": [
        "<terraform_version>"
    ],
    "location": "<location>",
    "description": "<workspace_description>",
    "tags": [],
    "template_repo": {
        "url": "<github_source_repo_url>"
        "branch": "master"
    },
    "template_data": [
        {
        "folder": ".",
        "type": "<terraform_version>",
        "variablestore": [
          {
            "name": "<variable_name1>",
            "value": "<variable_value1>",
            "type": "string",
            "secure": true
          },
          {
            "name": "<variable_name2>",
            "value": "<variable_value2>",
            "type": "bool",
            "secure": false
          }
        ]
        }
    ]
    }
    ```
    {: codeblock}

    | Parameter | Description |
    | --- |  --- |
    | `workspace_name` | Enter a name for your workspace. The maximum length of character limit is set to 1 MB. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspaces-plan#structure-workspace). |
    | `terraform_version` | The Terraform version that you want to use to run your Terraform code. To use Terraform `version 0.12`, enter `terraform_v0.12`, and similarly `terraform_v0.13`, and `terraform_v0.14`. Make sure that your Terraform config files are compatible with the Terraform version that you specify. If the Terraform variable version is not specified. by default, {{site.data.keyword.bpshort}} selects the version from your template. |
    | `location` | Enter the location where you want to create your workspace. The location determines where your {{site.data.keyword.bpshort}} actions run and where your workspace data is stored. The location is independent from the region where you want to create your {{site.data.keyword.cloud_notm}} services. |
    | `description` | Enter a description for your workspace. |
    | `github_source_repo_url` | Enter the URL to the GitHub or GitLab repository where your Terraform configuration files are stored. If you choose to create your workspace without a GitHub repository, your workspace is created with a **draft** state. To connect your workspace to a GitHub repository later, you must use the `ibmcloud schematics workspace update` command. |
    | `variable_name` | Optional, enter the name for the input variable that you declared in your Terraform configuration files. |
    | `variable_type` | Optional, enter the data type of your input variable. For supported data types, see the [`ibmcloud schematics workspace new` command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new). |
    {: caption="JSON file component description" caption-side="bottom"}

2. Create the workspace. 
    ```sh
    ibmcloud schematics workspace new --file workspace.json
    ```
    {: pre}

3. Verify that your workspace is created. Make sure that your workspace is in an **Inactive** state.
    ```sh
    ibmcloud schematics workspace list
    ```
    {: pre}

4. Refer to, [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start creating, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.

## Creating a workspace using the API
{: #create-wks-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.

2. Create the workspace by using Terraform template.
    ```sh
    curl --request POST --url https://schematics.cloud.ibm.com/v1/workspaces -H "Authorization: <iam_access_token>" -d '{"name": "<workspace_name>","type": ["<terraform_version>"],"location": "<location>","description": "<description>","template_repo": {"url": "<github_source_repo_url>"},"template_data": [{"folder": ".","type": "<terraform_version>","variablestore": [{"value": "<variable_value>","name": "<variable_name>","type": "<variable_type>","secure": true}]}]}'
    ```
    {: pre}

    | Parameter | Description |
    | ----- | ----- |
    | `iam_access_token` | Enter the IAM access token that you retrieved in step 1. |
    | `workspace_name` | Enter a name for your workspace. The maximum length of character limit is set to 1 MB. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspaces-plan#structure-workspace). |
    | `terraform_version` | The Terraform version that you want to use to run your Terraform code. Enter `terraform_v0.12` to use Terraform `version 0.12`, and similarly `terraform_v0.13`, and `terraform_v0.14`. Make sure that your Terraform config files are compatible with the Terraform version that you specify. If the Terraform variable version is not specified, by default, {{site.data.keyword.bpshort}} selects the version from your template.|
    | `location` | Enter the location where you want to create your workspace. The location determines where your {{site.data.keyword.bpshort}} actions run and where your workspace data is stored. The location is independent from the region where you want to create your {{site.data.keyword.cloud_notm}} services. |
    | `description` | Enter a description for your workspace. |
    | `github_source_repo_url` | Enter the URL to the GitHub or GitLab repository where your Terraform configuration files are store |
    | `variable_name` | Optional, enter the name for the input variable that you declared in your Terraform configuration files. |
    | `variable_value` | Optional, enter the value for your input variable. |
    | `variable_type` | Optional, enter the data type of your input variable. For supported data types, see the [`ibmcloud schematics workspace new` command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new). |
    {: caption="JSON file component description" caption-side="bottom"}

3. Verify that the workspace is created successfully. 
    ```sh
    curl -X GET https://schematics.cloud.ibm.com/v1/workspaces -H "Authorization: <iam_access_token>"
    ```
    {: pre}

4. See [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to create, update, or delete {{site.data.keyword.cloud_notm}} resources with Terraform.



## Creating a workspace using a Terraform template 
{: #create-wks-terraform}
{: terraform}

1. Follow the steps in [Setting up Terraform for {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-terraform-setup) to create your workspace with Terraform.

2. See [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to create, update, or delete {{site.data.keyword.cloud_notm}} resources with Terraform.

## Next steps
{: #sch-create-wks-nextsteps}

The next stage of working with workspaces is [deploying workspaces](/docs/schematics?topic=schematics-sch-deploy-wks). 
