---

copyright:
  years: 2017, 2024
lastupdated: "2024-10-17"

keywords: schematics workspaces, workspaces, schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Updating workspaces
{: #sch-update-wks}

Update the details for an existing workspace, such as the workspace name, variables, or source control URL. To provision or modify IBM Cloud, see the ibmcloud schematics plan command.
{: shortdesc}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

{{site.data.keyword.bplong_notm}} deprecates creation of workspace using the {{site.data.keyword.terraform-provider_full_notm}} v1.2, v1.3 template from 2nd week of April 2024.
{: important}

## Before you begin
{: #update-prerequisites}

Ensure the `location` and the `url` endpoint are pointing to the same region when you create or update the {{site.data.keyword.bpshort}} workspace or action. For more information about location and endpoint, see [Where is your information stored](/docs/schematics?topic=schematics-secure-data#pi-location)?
{: note}

## Updating a workspace using the UI
{: #update-wks-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Access **Schematics** > **Workspaces** > [**Update workspace**](https://cloud.ibm.com/schematics/workspaces/create){: external}.
    - In **Specify Template** section:
        - **GitHub, GitLab, or `Bitbucket` repository URL** - `<provide your Terraform template Git repository URL`.
        - **Personal access token** - `<leave it blank>`. You can click the `Open reference picker` to select a your Secret Manager key reference. For more information, see [creating a Secret Manager instance](/docs/secrets-manager?topic=secrets-manager-create-instance).
        - Terraform Version - `terraform_v1.5`. You need to select Terraform version 1.5 or greater version. For example, if your Terraform templates are created by using Terraform v1.5, select the `Terraform version` parameter as **terraform_v1.5**. 
          You can select `Terraform_v1.4` to use Terraform version 1.4, `terraform_v1.6` to use Terraform version 1.6. When you specify `terraform_v1.5` means that users can have template that is of Terraform `v1.5.0`, `v1.5.1`, or `v1.5.7`, so on. {{site.data.keyword.bpshort}} supports `Terraform_v1.x` and also plans to make releases available after `30  to 45 days` of HashiCorp Configuration Language (HCL) release.
          {: note}

          {{site.data.keyword.bpshort}} supports the current release of `Terraform v1.4`, through `terraform_v1.6`. The Terraform template must use the version constraint, such as `>` or `>=` or `~>` for the `required_version` of Terraform, to automatically pick the current version.

          ```terraform
          terraform {
          required_version = "~> 1.5"
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
    - Click `Update`. Your workspace is Updated with a **Draft** state and the workspace **Settings** page opens.

### Verifying workspace update 
{: #verify-wks-update-ui}

1. Click your workspace that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/workspaces){: external} to view the results of the workspace details. 
2. Click **Jobs** tab to see the workspace logs. 
3. Click **Jobs history** tab view the result of the update job operation that were run by the automation modules.
4. Click **Settings** tab to view the summary of the configuration.

## Updating a workspace using the CLI
{: #update-wks-cli}
{: cli}

1. Update a JSON file on your local workstation and add your workspace configuration. For more configuration options when updating the workspace, see the [`ibmcloud schematics workspace update` command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update).

    ```json
    {
        "name":"testwspace03jan",
        "type":[
            "terraform_v1.4"
        ],
        "description":"terraform workspace",
        "location":"us-east",
        "tags":[
            "department:HR",
            "application:compensation",
            "environment:production"
        ],
        "template_repo":{
            "url":"https://github.com/Anil-CM/newrepo"
        },
        "workspace_status":{
            "frozen":true
        },
        "template_data":[
            {
            "folder":".",
            "type":"terraform_v1.4",
            "variablestore":[
            {
                "name":"sample_var",
                "secure":true,
                "value":"THIS IS IBM CLOUD TERRAFORM CLI DEMO",
                "description":"Description of sample_var"
            },
            {
                "name":"sleepy_time",
                "value":"15"
                }
            ]
            }
        ]
    }
    ```
    {: codeblock}

    Syntax

    ```sh
    ibmcloud schematics workspace update --id WORKSPACE_ID [--file FILE_NAME] [--github-token GITHUB_TOKEN] [--pull-latest]  [--output OUTPUT]
    ```
    {: pre}

2. Update the workspace details for an existing workspace, such as the workspace name, variables, or source control URL. 

    ```sh
    ibmcloud schematics workspace update --id <WORKSPACE_ID>
    ```
    {: pre}

3. Verify that your workspace is updated. Make sure that your workspace is in an **Inactive** state.

    ```sh
    ibmcloud schematics workspace list
    ```
    {: pre}

4. Refer to, [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start creating, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.

### Verifying workspace update 
{: #verify-wks-update-cli}

Confirm the details using the CLI command update where the parameters of your workspace were updated successfully that has been created earlier.

```text
ibmcloud schematics workspace update --id us-east.workspace.testwspace03jan.811182d2 --target vpc_name --target vpc_tags
Do you really want to perform this action? [y/N]> y
                
Activity ID   c10fc92ddfd2d9ec645fc5dbece5e341   
                
OK
```
{: screen}

On successful update, it returns the updated details of an existing workspace.

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-wks-create-api&interface=cli).

## Updating a workspace using the API
{: #update-wks-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.

2. Updating the details for an existing workspace, such as the workspace name, variables, or source control URL.

    Example

    ```json
    PUT /v1/workspaces/{w_id} HTTP/1.1
    Host: schematics.cloud.ibm.com
    Content-Type: application/json
    Authorization: <auth_token>
    {
    "name":"testwspace03jan",
    "type":[
        "terraform_v1.4"
    ],
    "description":"terraform workspace updated",
    "location":"us-east",
    "tags":[
        "department:HR",
        "application:compensation",
        "environment:production"
    ],
    "template_repo":{
        "url":"https://github.com/Anil-CM/newrepo"
    },
    "workspace_status":{
        "frozen":true
    },
    "template_data":[
        {
            "folder":".",
            "type":"terraform_v1.4",
        "variablestore":[
            {
                "name":"sample_var",
                "secure":true,
                "value":"THIS IS IBM CLOUD TERRAFORM CLI DEMO",
                "description":"Description of sample_var"
            },
            {
                "name":"sleepy_time",
                "value":"15"
                }
            ]
        }
    ]
    }
    ```
    {: codeblock}

3. Verify that the workspace is successfully updated.

    ```sh
    curl -X GET https://schematics.cloud.ibm.com/v1/workspaces -H "Authorization: <iam_access_token>"
    ```
    {: pre}

4. see [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start creating, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.

### Verifying workspace update
{: #verify-wks-update-api}

Verify that the workspace update is successfully as shown in the output.
{: shortdesc}

Output

```text
{
    "id": "us-east.workspace.testwspace03jan.cf74cc48",
    "name": "testwspace03jan",
    "crn": "crn:v1:bluemix:public:schematics:us-south:a/1f7277194bb748cdb1d35fd8fb85a7cb:9ae7be42-0d59-415c-a6ce-0b662f520a4d:workspace:us-east.workspace.testwspace03jan.cf74cc48",
    "type": [
        "terraform_v1.4"
    ],
    "description": "terraform workspace updated successfully",
    "resource_group": "Default",
    "location": "us-east",
    "tags": [
        "department:HR",
        "application:compensation",
        "environment:production"
    ],
    "created_at": "2023-01-03T07:02:38.369965717Z",
    "created_by": "test@in.ibm.com",
    "status": "DRAFT",
    "failure_reason": "",
    "workspace_status_msg": {
        "status_code": "200",
        "status_msg": ""
    },
    "workspace_status": {
        "frozen": true,
        "frozen_by": "test@in.ibm.com",
        "frozen_at": "2023-01-05T13:44:32.400019282Z",
        "locked": false
    },
    "template_repo": {
        "url": "https://github.com/Anil-CM/newrepo",
        "commit_id": "3dc60ea7fb30f236dbadfa817cf4beb5d337808d",
        "full_url": "https://github.com/Anil-CM/newrepo",
        "has_uploadedgitrepotar": false
    },
    "template_data": [
        {
            "id": "b44c147b-81fb-4e",
            "folder": ".",
            "compact": false,
            "type": "terraform_v1.4",
            "values_url": "https://us.schematics.cloud.ibm.com/v1/workspaces/us-east.workspace.testwspace03jan.cf74cc48/template_data/b44c147b-81fb-4e/values",
            "values": "",
            "values_metadata": [
                {
                    "default": "testvpcone",
                    "description": "",
                    "name": "vpc_name",
                    "type": "string"
                },
                {
                    "default": "[\"tag:test1\", \"tag:test2\"]",
                    "description": "",
                    "name": "vpc_tags",
                    "type": "list(string)"
                }
            ],
            "variablestore": [
                {
                    "name": "sample_var",
                    "secure": true,
                    "value": "THIS IS IBM CLOUD TERRAFORM CLI DEMO",
                    "type": "",
                    "description": "Description of sample_var"
                },
                {
                    "name": "sleepy_time",
                    "secure": false,
                    "value": "15",
                    "type": "",
                    "description": ""
                }
            ],
            "has_githubtoken": false
        }
    ],
    "runtime_data": [
        {
            "id": "b44c147b-81fb-4e",
            "engine_name": "terraform",
            "engine_version": "v1.0.11",
            "state_store_url": "https://us.schematics.cloud.ibm.com/v1/workspaces/us-east.workspace.testwspace03jan.cf74cc48/runtime_data/b44c147b-81fb-4e/state_store",
            "log_store_url": "https://us.schematics.cloud.ibm.com/v1/workspaces/us-east.workspace.testwspace03jan.cf74cc48/runtime_data/b44c147b-81fb-4e/log_store"
        }
    ],
    "shared_data": {
        "resource_group_id": ""
    },
    "applied_shareddata_ids": null,
    "updated_by": "test@in.ibm.com",
    "updated_at": "2023-01-05T13:44:32.483034358Z",
    "last_health_check_at": "0001-01-01T00:00:00Z",
    "cart_id": "",
    "last_action_name": "WORKSPACE_UPDATE",
    "last_activity_id": "fe1591c02c66167b41fefac1ab5445cb",
    "last_job": {
        "job_id": "fe1591c02c66167b41fefac1ab5445cb",
        "job_name": "WORKSPACE_UPDATE",
        "job_status": ""
    }
}
```
{: screen}

On successful workspace update, it returns the details for an updated configuration, such as the workspace name, variables, or source control URL of the workspace.

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-wks-create-api&interface=cli).

## Updating a workspace with Terraform
{: #update-wks-terraform}
{: terraform}

1. Follow the steps in [Setting up Terraform for {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-terraform-setup) to create your workspace with Terraform.

2. See [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to create, update, or delete {{site.data.keyword.cloud_notm}} resources with Terraform.

## Next steps
{: #sch-update-wks-nextsteps}

The next stage of working with workspace is [deploying workspaces](/docs/schematics?topic=schematics-sch-deploy-wks).
