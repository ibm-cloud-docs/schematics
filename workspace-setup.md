---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-22"

keywords: schematics workspaces, schematics workspace vs github repo, schematics workspace access, schematics freeze workspace

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Setting up workspaces
{: #workspace-setup}

With {{site.data.keyword.bplong}} Workspaces, you can organize your Terraform templates and control the access to run infrastructure code in your {{site.data.keyword.cloud}} account. Before you create a workspace, make sure that you [design the organizational structure of your Git repository and workspaces](/docs/schematics?topic=schematics-workspace-setup#workspaces-plan) so that you can replicate and manage your configurations across multiple environments. 
{: shortdesc} 

If you plan to store your Terraform templates on your local machine and upload them as a tape archive file (`.tar`) to {{site.data.keyword.bplong_notm}}, make sure that the file structure on your local machine matches the suggested Git repository structure. 
{: note}

## Creating workspaces and importing your Terraform template
{: #create-workspace}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version).
{: deprecated}

Create a workspace for your Terraform template by using the {{site.data.keyword.bplong_notm}} console. The workspace settings can be configured to use the Terraform template that are hosted and managed in a Git repository. Your workspace is used to manage the state of the cloud resources, provisioned using the Terraform template.
{: shortdesc} 

### Before you begin
{: #prerequisites}

- [Create a Terraform configuration](/docs/schematics?topic=schematics-create-tf-config), and store the configuration in a `GitHub`, `GitLab`, or `Bitbucket` repository. You can also upload a tape archive file (`.tar`) from your local machine to provide your template to {{site.data.keyword.bplong_notm}}. For more information, see the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command and see the [upload a tar file to your workspace](/apidocs/schematics/schematics#upload-template-tar) API. 
- Make sure that you have the [required permissions](/docs/schematics?topic=schematics-access) to create a workspace. 

Ensure the `location` and the `url` endpoint are pointing to the same region when you create or update the {{site.data.keyword.bpshort}} Workspaces and actions. For more information about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
{: note}

### Creating the workspace from the console
{: #create-workspace_ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Navigate to **Schematics** > **Workspaces** > [**Create workspace**](https://cloud.ibm.com/schematics/workspaces/create){: external}.
    - In **Specify Template** section:
        - **GitHub, GitLab or `Bitbucket` repository URL** - `<provide your Terraform template Git repository URL`.
        - **Personal access token** - `<leave it blank>`.
        - Terraform Version - `terraform_v1.0`. You need to select Terraform version 1.0 or greater version. For example, if your Terraform templates is created by using Terraform v1.0, select the `Terraform version` parameter as **terraform_v1.0**. 
          You can select `Terraform_v1.1` to use Terraform version 1.1, `terraform_v1.0` to use Terraform version 1.0. When you specify `terraform_v1.1`means users can have template that are of Terraform `v1.1.0`, `v1.1.1`, or `v1.1.2`, so on. {{site.data.keyword.bpshort}} supports `Terraform_v1.x` and also plans to make releases available after `30  to 45 days` of HashiCorp Configuration Language (HCL) release.
          {: note}

          {{site.data.keyword.bpshort}} supports the latest release of `Terraform v1.1`, through `terraform_v1.0`. The Terraform template must use the version constraint, such as `>` or `>=` or `~>` for the `required_version` of Terraform, to automatically pick the latest version.
          ```terraform
          terraform {
          required_version = "~> 1.1"
          }
          ```
          {: pre}

        - Optional: Click **Bind Agent** to bind a new workspace to the Agent. If you do not see the Agents list, check have you [connected the Agents](/docs/schematics?topic=schematics-register-agent)?
        - Click `Next`.
    - In **Workspace details** section. Enter a name for your `workspace name`. The name can be up to 128 characters long and can include alphanumeric characters, spaces, dashes, and underscores.
        - **Workspace name** as `schematics-agent-service`.
        - **Tags** as `my-tags`. Optional: Enter tags for your workspace. You can use the tags later to find your workspace faster.
        - **Resource group** as `default` or other resource group for this workspace. 
        - **Location** as `North America` or other [region](/docs/schematics?topic=schematics-multi-region-deployment) for this workspace. Decide where you want to create your workspace. The location determines where your {{site.data.keyword.bpshort}} jobs run and your workspace data is stored. You can choose between a geography, such as North America, or a metro city, such as Frankfurt or London. If you select a geography, {{site.data.keyword.bpshort}} determines the location based on availability. If you select a metro city, your workspace is created in this location. For more information about where your data is stored, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location). The location that you choose is independent from the region or regions where you want to provision your {{site.data.keyword.cloud_notm}} resources. Note that the console does not support all available locations. To create the workspace in a different location, use the [CLI](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) or [API](/apidocs/schematics/schematics#create-a-workspace) instead.
        - Optional: Enter a descriptive name for your workspace.
        - Click `Next`.
    - Click `Create`. Your workspace is created with a **Draft** state and the workspace **Settings** page opens.

### Creating the workspace through CLI
{: #create-workspace-cli}
{: cli}

1. Create a JSON file on your local machine and add your workspace configuration. For additional configuration options when creating the workspace, see the [`ibmcloud schematics workspace new` command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new).

    To create a workspace with Agent, refer to [ibmcloud schematics workspace new with Agent](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-new) command.
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
    | `workspace_name` | Enter a name for your workspace. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace). |
    | `terraform_version` | The Terraform version that you want to use to run your Terraform code. Enter `terraform_v0.12` to use Terraform version 0.12, and similarly `terraform_v0.13`, and `terraform_v0.14`. Make sure that your Terraform config files are compatible with the Terraform version that you specify. This is a required variable. If the Terraform version is not specified, By default, {{site.data.keyword.bpshort}} selects the version from your template. |
    | `location` | Enter the location where you want to create your workspace. The location determines where your {{site.data.keyword.bpshort}} Actions run and where your workspace data is stored. The location is independent from the region where you want to create your {{site.data.keyword.cloud_notm}} services. |
    | `description` | Enter a description for your workspace. |
    | `github_source_repo_url` | Enter the URL to the GitHub or GitLab repository where your Terraform configuration files are stored. If you choose to create your workspace without a GitHub repository, your workspace is created with a **draft** state. To connect your workspace to a GitHub repository later, you must use the `ibmcloud schematics workspace update` command. |
    | `variable_name` | Optional: Enter the name for the input variable that you declared in your Terraform configuration files. |
    | `variable_type` | Optional: Enter the data type of your input variable. For supported data types, see the [`ibmcloud schematics workspace new` command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new). |
    {: caption="JSON file component description" caption-side="bottom"}

2. Create the workspace. 
    ```sh
    ibmcloud schematics workspace new --file workspace.json
    ```
    {: pre}

3. Verify that your workspace is created. Make sure that you workspace is in an **Inactive** state.
    ```sh
    ibmcloud schematics workspace list
    ```
    {: pre}

4. Refer to, [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start creating, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.

### Creating the workspace through API
{: #create-workspace-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.

2. Create the workspace. 
    ```sh
    curl --request POST --url https://schematics.cloud.ibm.com/v1/workspaces -H "Authorization: <iam_access_token>" -d '{"name": "<workspace_name>","type": ["<terraform_version>"],"location": "<location>","description": "<description>","template_repo": {"url": "<github_source_repo_url>"},"template_data": [{"folder": ".","type": "<terraform_version>","variablestore": [{"value": "<variable_value>","name": "<variable_name>","type": "<variable_type>","secure": true}]}]}'
    ```
    {: pre}

    | Parameter | Description |
    | -- | -- |
    | `iam_access_token` | Enter the IAM access token that you retrieved in step 1. |
    | `workspace_name` | Enter a name for your workspace. For more information, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace). |
    | `terraform_version` | The Terraform version that you want to use to run your Terraform code. Enter `terraform_v0.12` to use Terraform version 0.12, and similarly `terraform_v0.13`, and `terraform_v0.14`. Make sure that your Terraform config files are compatible with the Terraform version that you specify. This is a required variable. If the Terraform version is not specified, By default, {{site.data.keyword.bpshort}} selects the version from your template.|
    | `location` | Enter the location where you want to create your workspace. The location determines where your {{site.data.keyword.bpshort}} Actions run and where your workspace data is stored. The location is independent from the region where you want to create your {{site.data.keyword.cloud_notm}} services. |
    | `description` | Enter a description for your workspace. |
    | `github_source_repo_url` | Enter the URL to the GitHub or GitLab repository where your Terraform configuration files are store |
    | `variable_name` | Optional: Enter the name for the input variable that you declared in your Terraform configuration files. |
    | `variable_value` | Optional: Enter the value for your input variable. |
    | `variable_type` | Optional: Enter the data type of your input variable. For supported data types, see the [`ibmcloud schematics workspace new` command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new). |
    {: caption="JSON file component description" caption-side="bottom"}

3. Verify that the workspace is created successfully. 
    ```sh
    curl -X GET https://schematics.cloud.ibm.com/v1/workspaces -H "Authorization: <iam_access_token>"
    ```
    {: pre}

4. see [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start creating, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.

### Creating the workspace with Terraform
{: #create-workspace-terraform}
{: terraform}

1. Follow the steps in [Setting up Terraform for {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-terraform-setup) to create your workspace with Terraform.

2. see [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start creating, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.

### Importing your Terraform template
{: #import-template}
{: ui}

If you want to upload a tape archive file (`.tar`) instead of importing your workspace to a Git repository, you must use the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command and see the [upload a tar file to your workspace](/apidocs/schematics/schematics#upload-template-tar) API. 
{: tip}

1. On the workspace **Settings** page, enter the edit icon to edit your `Repository URL`. The link can point to the `master` branch, any other branch, or a subdirectory. 
    - Example for `master` branch: `https://github.com/myorg/myrepo`
    - Example for other branches: `https://github.com/myorg/myrepo/tree/mybranch`
    - Example for subdirectory: `https://github.com/mnorg/myrepo/tree/mybranch/mysubdirectory`      
2. If you want to use a private Git repository, enter your personal access token. The personal access token is used to authenticate with your Git repository to access your Terraform template. For more information, see [Creating a personal access token for the command-line](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token). If you want to clone from the Git repository see the [allowed and blocked file extensions](/docs/schematics?topic=schematics-workspaces-faq#clone-file-extension) for cloning.
3. Select the `Terraform version` that your Terraform configuration files are written in.
4. Click the checkbox `I understand the changes that could happen if I edit this URL and I agree to these happening` option.
5. Click **Save**. {{site.data.keyword.bplong_notm}} automatically downloads the Terraform configuration files from your repository, scans them for syntax errors, and retrieves all input variables that you declared in your configuration files. When all configuration files are downloaded successfully and no syntax errors are found, the workspace state changes to **Inactive**.

    After your Terraform configuration files are scanned, you can view the results on the workspace **Activity** page. The total number of files that were scanned in the source repository is displayed as `scanned`. The total number of files that are vulnerable, such as unsupported file extensions, is displayed as `discarded`. Click **Jobs** to find the details of the files that were scanned and discarded. For more information about viewing logs, see [Reviewing the {{site.data.keyword.bpshort}} job details](/docs/schematics?topic=schematics-workspace-setup#job-logs).
    {: tip}

6. Review the default input variable values for your Terraform template. To change an input variable value, click **Edit** from the Actions menu. Depending on the data type that your variable uses, you must enter the value in a specific format. see the following table to find example values for each supported data type. 

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




### Running your Terraform template in {{site.data.keyword.cloud_notm}}
{: #run-template}
{: ui}

refer to, [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start creating, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.

## Freezing and unfreezing workspaces 
{: #lock-workspace}

As the {{site.data.keyword.cloud_notm}} account or an {{site.data.keyword.bplong_notm}} user who is assigned the **Manager** IAM service access role for {{site.data.keyword.bpshort}}, you can disable changes to a workspace (freeze) so that you cannot create a Terraform execution plan or run your infrastructure code to provision or modify your {{site.data.keyword.cloud_notm}} resources. 
{: shortdesc}

Before you begin, make sure that you are assigned the [**Manager** IAM service access role](/docs/schematics?topic=schematics-access) for {{site.data.keyword.bplong_notm}}. 

**To freeze a workspace**: 
1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that you want to freeze. 
2. Select the **Settings** tab. 
3. In the **State** section on the workspace settings page, set the toggle to **Frozen** to disable changes to your workspace. The ID of the user who freezes the workspace and a timestamp are automatically logged. After you freeze a workspace, no user can generate a Terraform execution plan, or apply the plan in {{site.data.keyword.cloud_notm}}. 

**To unfreeze a workspace**: 
1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that you want to unfreeze. 
2. Select the **Settings** tab. 
3. In the **State** section on the workspace settings page, set the toggle to **Unfrozen**. The ID of the user who unfreezes the workspace and a timestamp are automatically logged. After you unfreeze a workspace, you can generate new Terraform execution plans or run your infrastructure code by applying the plan in {{site.data.keyword.cloud_notm}}.

## Deleting a workspace
{: #del-workspace}

You can use the {{site.data.keyword.bplong_notm}} to delete your workspace. While deleting the workspace, you can optionally choose to destroy the cloud resources provisioned by the workspace. 

1. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select the workspace that you want to delete. The table describes the delete workspace and destroy the cloud resources with various job.

    Decide if you want to delete the workspace and destroy the associated cloud resources, or both. This job cannot be undone. If you remove the workspace and keep the  cloud resources, you need to manage the resources with the resource list or command-line.
    {: note}

    | Job | Delete workspace | Destroy the associated cloud resources |
    | -- | -- |  -- |
    | Delete workspace  \n Use this option, if you have already destroyed the cloud resources, or intend to destroy them later by using the command-line or user interface. | True | False |
    | Destroy only resources | False | True |
    | Delete workspace and destroy the resources provisioned by workspace | True | True |
    | Resources destroyed using command-line or resource list, and want to delete workspace | True | False |
    {: caption="Delete workspace and destroy the resources" caption-side="bottom"}

2. Select the workspace that you want to delete.
3. Select the **Actions** drop down list.
4. Select the **Delete workspace** option.
5. Type the workspace name in the **Type workspace name to confirm** text box.
6. Click the **Delete** button.

## Planning your workspace
{: #workspaces-plan}

You can plan and design your workspace by following queries.
- How to map workspace resource with the Git repository structure?
- How many number of workspaces that you need?
- How to reuse the configuration files across environments and workspaces?
- How to control access and manage your workspaces?

### Designing your workspace and Git repository structure
{: #structure-workspace}
{: help}
{: support}

Plan out the organizational structure of your workspace to match the microservice and permission structure of your organization. These workspaces uses Terraform templates from private or public Git repositories such as `GitHub`, `GitLab`, `Bitbucket`, and Azure DevOps. The table provides the format of the repositories source.
{: shortdesc} 

|Git repositories| URL|
|-------------|-----|
|`GitHub`| `https://github.com/<your_user_name>/<repo_name>/tree/<branch_name>/<folder_name>`|
|`GitLab`| `https://gitlab.com/<your_user_name>/<project_name>/tree/<branch_name>/<folder_name>` |
|`Bitbucket`|`https://bitbucket.org/<your_user_name>/<repo_name>/src/<branch_name>/<folder_name>`  \n `https://<username>@bitbucket.org/<workspace_name>/tf_cloudless_sleepy/src/master` |
|`Azure DevOps`|`https://azure.com/<your_user_name>/<repo_name>/src/<branch_name>/<folder_name>`  \n `https://visualstudio.com/<your_user_name>/<repo_name>/src/<branch_name>/<folder_name>`|
{: caption="Git repositories" caption-side="bottom"}

### How many workspaces do I need?
{: #plan-number-of-workspaces}

To find out how many workspaces you need in {{site.data.keyword.bplong_notm}}, look at the microservices that build your app and the environments that you need to develop, test, and publish your microservice. 
{: shortdesc}

As a rule of thumb, consider creating separate workspaces for each of your microservices and the environment that you use. For example, if you have a product app that consists of a search, payment, and review microservice component, consider creating a separateworkspace for each microservice component and development, staging, and production environment. With separate workspaces for each microservice component and environment, you can develop, deploy, and manage the Terraform configuration files and associated {{site.data.keyword.cloud_notm}} resources without affecting the resources of other microservices.

Review the following image to see the number of workspaces in {{site.data.keyword.bplong_notm}} for an app that consists of three microservices.

![Workspace structure for {{site.data.keyword.bplong_notm}}](images/workspace-structure.png){: caption="Figure 1. workspace structure for {{site.data.keyword.bplong_notm}}" caption-side="bottom"}

Do not use one workspace to manage an entire staging or production environment. When you deploy all your {{site.data.keyword.cloud_notm}} resources into a single workspace, it can become difficult for various teams to coordinate updates and manage access for these resources.
{: important}

### How do I structure my Git repository to map my workspaces?
{: #plan-github-structure}

Structure your Git repository so that you have one repository for all your Terraform configuration files that build your microservice, and use input variables in {{site.data.keyword.bpshort}}, or GitHub branches or directories to differentiate between your development, staging, and production environment. 
{: shortdesc}

Review the following table to find a list of options for how to structure your Git repository to map the different workspace environments. 
{: shortdesc}

| Option | Description | 
| ------- | ---------------------------- | 
| One Git repo, use variables to distinguish between environments | Create one Git repository where you store the Terraform configuration files that make up your microservice component. Make your Terraform configuration files as general as possible so that you can reuse the same configuration across your environments. To configure the specifics of your development, staging, and production environment, use [Terraform input variables](/docs/schematics?topic=schematics-create-tf-config#configure-variables) in your configuration files. Input variables are automatically loaded into {{site.data.keyword.bplong_notm}} when you create your workspace. To customize your workspace, you enter the environment-specific values for your variables. This setup is useful if you have one team that manages the lifecycle of the microservice component and where the configuration of your environments does not differ drastically. |
|One Git repo, use branches to distinguish between environments | Create one Git repository for your microservice component, and use different Git branches to store the Terraform configuration files for each of your environments. With this setup, you have a clear distinction between your environments and more control over who can access and change a particular configuration. Make sure to set up how changes in one configuration file are populated across branches to avoid that you have different configurations in each environment. |
| One Git repo, use directories to distinguish between environments | For organizations that prefer short-lived branches, and where configurations differ drastically across environments, consider creating directories that represent the different configurations of your environments. With this setup, all your directories listen for changes that are committed to the `master` branch. Make sure to set up how changes in one configuration file are populated across directories to avoid having different configurations in each environment. |
| Use one Git repo per environment | Use one Git repository for each of your environments. With this setup, you have a 1:1 relationship between your workspace and Git repository and you can apply separate permissions for each of your Git repositories. Make sure that your team can manage multiple Git repositories and keep them in sync. | 
{: caption="Structure of the Git repository" caption-side="bottom"}

### How can I reuse configuration files across environments and workspaces?
{: #plan-reuse}

Try to minimize the number of Terraform configuration files that you need to manage by creating standardized Terraform templates and by using variables to customize the template to your needs. 
{: shortdesc}

Now, you can use Terraform modules from the Terraform module registry for IBM Cloud.
{: important}

With standardized Terraform templates or Terraform modules, you can ensure that development best practices are followed within your organization and that all Terraform configuration files have the same structure. Knowing the structure of a Terraform configuration file makes it easier for your developers to understand a file, declare variables, contribute to the code, and troubleshoot the errors. 

### How do I control access to my workspaces? 
{: #plan-workspace-access}

{{site.data.keyword.bplong_notm}} is fully integrated with {{site.data.keyword.iamlong}}. To control access to a workspace, and who can execute your infrastructure code with {{site.data.keyword.bplong_notm}}, see [Managing user access](/docs/schematics?topic=schematics-access). 

### What do I need to be aware of when I have a repository that I managed with native Terraform?
{: #plan-terraform-migration}

Because {{site.data.keyword.bplong_notm}} delivers Terraform-as-a-Service, you can import your existing Terraform templates into {{site.data.keyword.bpshort}} Workspaces. Depending on how your Terraform templates and Git repositories are structured, you might need to make changes so that you can successfully use {{site.data.keyword.bplong_notm}}. 
{: shortdesc}

- **Provider block declaration**: Because {{site.data.keyword.bplong_notm}} is integrated with {{site.data.keyword.iamlong}}, your {{site.data.keyword.cloud_notm}} API key is automatically retrieved for all IAM-enabled resources and you don't have to provide this information in the `provider` block. However, the API key is not retrieved for classic infrastructure and {{site.data.keyword.ibmcf_notm}} resources. For more information, see [Configuring the `provider` block](/docs/schematics?topic=schematics-create-tf-config#configure-provider). 
- **Terraform command-line and {{site.data.keyword.cloud_notm}} provider plug-in:** To use {{site.data.keyword.bplong_notm}}, you don't need to install the Terraform command-line or the {{site.data.keyword.terraform-provider_full_notm}}. If you want to automate the provisioning of resources, try out the [{{site.data.keyword.bplong_notm}} command-line plug-in](/docs/schematics?topic=schematics-setup-cli) instead.

## Setting up a continuous delivery toolchain for your workspace
{: #continuous-delivery}

Connect your source repository to a continuous delivery pipeline in {{site.data.keyword.cloud_notm}} to automatically generate a Terraform execution plan and run your Terraform code in {{site.data.keyword.cloud_notm}} whenever you update your Terraform configuration files. 
{: shortdesc}

1. If you do not have a {{site.data.keyword.contdelivery_short}} service instance in your account yet, create one.
    1. From the {{site.data.keyword.cloud_notm}} catalog, open the [{{site.data.keyword.contdelivery_short}} service](https://cloud.ibm.com/catalog/services/continuous-delivery).
    2. Select the {{site.data.keyword.cloud_notm}} region where you want to create the service.
    3. Select a pricing plan.
    4. Enter a name for your service instance, select a resource group, and enter any tags that you want to associate with your service instance.
    5. Click **Create** to create the service instance in your account. 
2. From the [workspace dashboard](https://cloud.ibm.com/schematics/workspaces){: external}, select a workspace. 
3. Select the **Settings** tab. 
4. In the **Summary** section, click **Enable continuous delivery**. 
5. Configure your toolchain. 
    1. Enter a name for your toolchain, and select the region and resource group where you want to deploy this toolchain. The region and resource group can be different from the region and resource group that you used for your {{site.data.keyword.bpshort}} Workspace.
    2. Select the type of source repository where your Terraform configuration files are stored. For Example GitHub. 
    3. Review the information for your source repository. For example, if your Terraform files are stored in GitHub, review the GitHub server and the repository for which you want to create a continuous delivery toolchain. These fields are pre-populated based on your workspace configuration.
    4. Optional: Choose if you want to enable Git issues and code change tracking for your toolchain. 
6. Select the **Delivery Pipeline** icon to configure your Delivery Pipeline. 
    1. Verify that the workspace ID that is displayed to you is correct.
    2. Enter an {{site.data.keyword.cloud_notm}} API key. If you do not have an API key, click **New +** to create one. 
7. Click **Create** to finish the setup of your toolchain. You see an overview of tools that were configured for your toolchain. 
8. Open the **Delivery Pipeline**. The Delivery Pipeline includes stages to retrieve updates from your source repository, create a Terraform execution plan, apply this plan, and to run a health check against your workspace.
9. Update the Terraform file in your source repository and review how this change is processed in your Delivery Pipeline. If one of the stages fails, click **View logs and history** to start troubleshooting errors. For more information about viewing logs and history, see [Reviewing the {{site.data.keyword.bpshort}} job details](/docs/schematics?topic=schematics-workspace-setup#job-logs).

## Workspace states
{: #wks-state}

### Workspace state overview
{: #states-overview}

Review the states that a workspace can have in the following table. You might not see all states in the {{site.data.keyword.cloud_notm}} console. Some states are only visible when using the command-line or API.
{: shortdesc}

| State | Description |
| ------- | ---------------------------- |
| `Active` | After you successfully ran your infrastructure code with {{site.data.keyword.bplong_notm}} by applying your Terraform execution plan, the state of your workspace changes to **Active**. |
| `Connecting` | {{site.data.keyword.bpshort}} tries to connect to the template in your source repo. If successfully connected, the template is downloaded and metadata, such as input parameters, is extracted. After the template is downloaded, the state of the workspace changes to **Scanning**. |
| `Draft` | The workspace is created without a reference to a `GitHub`, `GitLab`, or `Bitbucket` repository.  |
| `Failed` | If errors occur during the execution of your infrastructure code in {{site.data.keyword.bplong_notm}}, your workspace state is set to **Failed**. To troubleshoot errors, open the logs on the workspace **Activity** page. |
| `Inactive` | The {{site.data.keyword.bpshort}} template was scanned successfully and the workspace creation is complete. You can now start running {{site.data.keyword.bpshort}} plan and apply job to provision the {{site.data.keyword.cloud_notm}} resources that you specified in your template. If you have an **Active** workspace and decide to remove all your resources, your workspace is set to **Inactive** after all your resources are removed. |
| `Inprogress` | When you instruct {{site.data.keyword.bplong_notm}} to run your infrastructure code by applying your Terraform execution plan, the state of your workspace changes to `Inprogress`. |
| `Scanning` | The download of the {{site.data.keyword.bpshort}} template is complete and vulnerability scanning started. If the scan is successful, the workspace state changes to **Inactive**. If errors in your template are found, the state changes to **Template Error**. |
| `Stopped` | The {{site.data.keyword.bpshort}} plan, apply, or destroy job are stopped manually. |
| `Template_Error` | The {{site.data.keyword.bpshort}} template contains errors and cannot be processed.|
{: caption="Workspace state overview" caption-side="bottom"}

### Workspace state diagram and manipulative job
{: #workspace-state-diagram}

The state of a workspace indicates if you have successfully created a Terraform execution plan and applied to provision your resources in the {{site.data.keyword.cloud_notm}} account. The table represents the state and the workspace job.
{: shortdesc}

| workspace | State diagram | Description |
| -- | -- | --|
| `Create workspace`| ![Create workspace state](images/createworkspace.png) | The workspace is created without a reference to `GitHub`, `GitLab`, or `Bitbucket` to the draft state. From the draft state you can connect to the infrastructure template in your source repository. From connecting state, the template is processed successfully to reach Inactive state (Final state) or template parsing may fail and reach failed state. From inactive state, when you do an apply, and if it results in one resource then, state enters active state and if they destroy, state enters destroy state. you can maintain at least one resource in the state file by apply job, to move the workspace into active state. The {{site.data.keyword.bpshort}} [stores](/docs/schematics?topic=schematics-general-faq#persist-file) the user-defined file for running the subsequent Terraform commands. Then, you can destroy all the resources to make your workspace in an inactive state. |
|`Delete workspace` | ![Delete workspace state](images/deleteworkspace.png) | When you perform delete workspace on an inactive, active or failed state. From these state, the template is parsed successfully to reach an inactive state or template parsed can fail and reach failed state. If you delete at least one resource, the plan and apply job executes to destroy the resource from the active state. |
| `Plan and apply job` | ![Plan and apply action state](images/applyplan.png) | When you perform the plan or apply job on active, inactive, and failed state. Your workspace is in in progress and locked state. And the job is performed, if it is success, your workspace is in active state, if it contains at least one resource, your workspace is in an inactive state, on failure workspace is in failed state. The {{site.data.keyword.bpshort}} [stores](/docs/schematics?topic=schematics-general-faq#persist-file) the user-defined file for running the subsequent Terraform commands. |
| `Destroy job` | ![Destroy action state](images/destroyworkspace.png) | The destroy job performs when your workspace is in an inactive, active or failed state. From these state, the destroy job connects to parse the template from your source repository and workspace gets into in progress unlocked state. From   state if you destroy, resource reaches failed state.|
{: caption="Workspace state diagram" caption-side="bottom"}

## Creating an auto deployment to the {{site.data.keyword.bplong_notm}}
{: #create-deploy-to-schematics}

{{site.data.keyword.bplong_notm}} now supports an efficient way to share your Git repository in a cloned copy of the code in a new Git repository to deploy to {{site.data.keyword.cloud_notm}} without affecting your original code. For more information about deploy to {{site.data.keyword.cloud_notm}}, see [automating the deployment to the {{site.data.keyword.bpshort}}](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-create_deploy_to_schematics).


## Reviewing the {{site.data.keyword.bpshort}} job details
{: #job-logs}

Use the {{site.data.keyword.bpshort}} job page in the console to find the history of all {{site.data.keyword.bpshort}} activities, such as downloading your `template`, `plan`, `apply`, and to see the logs of the jobs. The jobs are created when you run your templates. You can also see the count of the resources that are in `plan`, or `apply` jobs that are in **added**, **modified**, or **destroyed** status. For more information about job queue process, see [Execution process of the {{site.data.keyword.bpshort}} job queue](/docs/schematics?topic=schematics-job-queue-process#about-job-queue).

In the job log you can see a message such as: 

- **Activity triggered. Waiting for the logs**. This means the job is in pending status and yet to be processed. 

- **Your job was submitted and is in queue, at position x out of y**. Here `x` is the position of your job in the pending queue and `y` is a total pending jobs. The available resources in {{site.data.keyword.bpshort}} backend are equally distributed to the pending jobs. In case you are running a huge number of jobs, you can view the position increase along with the total.
