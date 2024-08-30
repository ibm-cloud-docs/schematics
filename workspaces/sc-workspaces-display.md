---

copyright:
  years: 2017, 2024
lastupdated: "2024-08-30"

keywords: schematics workspaces, workspaces, schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Displaying workspaces
{: #sch-display-wks}

List the workspaces in your {{site.data.keyword.cloud}} account and optionally, show the details for your workspace.
{: shortdesc} 

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

{{site.data.keyword.bplong_notm}} deprecates creation of workspace using the {{site.data.keyword.terraform-provider_full_notm}} v1.2, v1.3 template from 2nd week of April 2024.
{: important}

## Before you begin
{: #display-prerequisites}

Ensure the `location` and the `url` endpoint are pointing to the same region when you list the {{site.data.keyword.bpshort}} workspaces and actions. For more information about location and endpoint, see [Where is your information stored](/docs/schematics?topic=schematics-secure-data#pi-location)?
{: note}

## Displaying the workspace through UI
{: #list-wks-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Access **Schematics** > **Workspaces** > [**workspace**](https://cloud.ibm.com/schematics/workspaces){: external}.
3. Search with name for required workspace with specific to location selecting North America or Europe.

### Verifying workspace display 
{: #verify-wks-list-ui}

1. Click your workspace that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/workspaces){: external} to view the results of the workspace details.


## Displaying the workspace through CLI
{: #list-wks-cli}
{: cli}

1. List the workspaces in your {{site.data.keyword.cloud_notm}} account and optionally, show the details for your workspace. For more configuration options when listing the workspace, see the [`ibmcloud schematics workspace list` command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-list).

    Syntax

    ```sh
    ibmcloud schematics workspace list [--limit LIMIT] [--offset OFFSET] [--output] [--region]
    ```
    {: pre}

2. Verify that your workspace is updated. Make sure that your workspace is in an **Inactive** state.

    ```sh
    ibmcloud schematics workspace list
    ```
    {: pre}   

3. Refer to, [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start creating, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.

### Verifying workspace list 
{: #verify-wks-list-cli}

Confirm the details using the CLI command update where the parameters of your workspace were updated successfully that has been created earlier.

    ```text
    ibmcloud schematics workspace list  
    Retrieving workspaces...
    Name                                          ID                                                                       Description                                   Version             Status           Frozen   
    workspacecos-module                           us-south.workspace.workspacecos-module.f39f5247                          test-cos-module-workspace                 Terraform v1.0.11   TEMPLATE_ERROR   False   
    testwspace03jan                               us-east.workspace.testwspace03jan.cf74cc48                               terraform workspace updated successfully      Terraform v1.0.11   INACTIVE         True   
    teststagews-sleepy12                          us-east.workspace.teststagews-sleepy12.a92d1471                          terraform workspace stage test                Terraform v1.0.11   INACTIVE         False   
    newname                                       us-east.workspace.test.fb3cc39b                                          terraformworkspacetest                        Terraform v1.2.6    ACTIVE           False   
    test-remote                                   us-east.workspace.test-remote.455ecb7a                                   terraform workspace                           Terraform v0.13.7   INACTIVE         True   
    test-remote-msk                               us-east.workspace.test-remote-msk.bcbff09f                               terraform workspace                           Terraform v0.13.7   CONNECTING       False   
    test-remote-msk-may18                         us-east.workspace.test-remote-msk-may18.102cc13d                         terraform workspace                           Terraform v0.13.7   CONNECTING       False   
    test-hpcs                                     us-east.workspace.test-hpcs.fd2f331d                                                                                   Terraform v1.2.6    INACTIVE         False   
    terraform_module1                             us-east.workspace.terraform_module1.e7cb92a3                             myblueprint                                   Terraform v1.0.11   DRAFT            False   
    terraform_module1                             us-east.workspace.terraform_module1.a11f46b9                             complex-delete                                Terraform v1.0.11   DRAFT            False   
    terraform_module1                             us-east.workspace.terraform_module1.647e4d4f                             myblueprint3                                  Terraform v1.0.11   DRAFT            False   
    smulampa-wsgroup                              us-east.workspace.smulampa-wsgroup.95a4d82d                              Sample workspace testing                      Terraform v1.0.11   FAILED           False   
    smulampa-schematics-agent-service-workspace   us-east.workspace.smulampa-schematics-agent-service-workspace.6936524b   schematics agents service workspace           Terraform v1.0.11   DRAFT            False   
    smulampa-cos-module-workspace                 us-east.workspace.smulampa-cos-module-workspace.b77841ac                 smulampa-cos-module-workspace                 Terraform v1.0.11   INACTIVE         False   
                                                
    OK
    ```
    {: screen}

On successful update, it returns the updated details of an existing workspace.

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-wks-create-api&interface=cli).

## Displaying the workspace list through API
{: #list-wks-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.

2. Displaying the details of list of all existing workspace.

    Example:

    ```json
    GET /v1/workspaces HTTP/1.1
    Host: schematics.cloud.ibm.com
    Content-Type: application/json
    Authorization: <auth_token>
    Cache-Control: no-cache
    Postman-Token: b30e81a0-ae99-978c-c7e3-664371ddd1ea
    ```
    {: codeblock}

3. Verify that the workspace is successfully listed with list of all workspace that were created.

    ```sh
    curl -X GET https://schematics.cloud.ibm.com/v1/workspaces -H "Authorization: <iam_access_token>"
    ```
    {: pre}

4. see [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start creating, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.

### verifying workspace update:
{: #verify-wks-list-api}

Verify the workspace update.

Verify that the workspace update is successfully as shown in the output.
{: shortdesc}

Output

```text
{
    "offset": 0,
    "limit": 100,
    "count": 2,
    "workspaces": [
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
            "status": "INACTIVE",
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
                            "value": "$SCHEMATICSSECRET$04$.KMS04&crn:v1:bluemix:public:kms:us-south:a/c19ef85117044059a3be5e45d6dc1cf6:0de40d7c-085d-4373-9083-bbe598d7c3a9:key:4e6e436a-e53f-423d-848d-a7af60335c1f&crn:v1:bluemix:public:kms:us-east:a/c19ef85117044059a3be5e45d6dc1cf6:4d336da6-914a-4d17-8dc6-dfd9da51a422:key:1987a798-d933-446f-af49-3df7de755269&eyJjaXBoZXJ0ZXh0IjoiMitWZXYveFdRc3VKTi9obnAvYWtBT0dDL0NDK0pTOGJYWk5VWGg3RHRVU2JXbFc0Rkx6SjlGRzVWN1U9IiwiaXYiOiJMMjFzRXNSSXFSYXl1VWh0IiwidmVyc2lvbiI6IjQuMC4wIiwiaGFuZGxlIjoiMWQ0ZWE1ODItY2ZmOS00ZDQ3LWE3OGQtYWJjZmQ5NDM0MjA1In0=.15c9468cde760d8bfbacaeed.e0f233aa99d32be8a146dfe0d07db4721c021df5d8cda989d4064996abae4fe6d3d5f3692c754d53ba438548bbecef6c42810b4a",
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
            "last_activity_id": "a7aaa1ddb52edee56377d22b67c7478d",
            "last_job": {
                "job_id": "a7aaa1ddb52edee56377d22b67c7478d",
                "job_name": "WORKSPACE_UPDATE",
                "job_status": ""
            }
        },
        {
            "id": "us-east.workspace.teststagews-sleepy12.a92d1471",
            "name": "teststagews-sleepy12",
            "crn": "crn:v1:bluemix:public:schematics:us-south:a/1f7277194bb748cdb1d35fd8fb85a7cb:9ae7be42-0d59-415c-a6ce-0b662f520a4d:workspace:us-east.workspace.teststagews-sleepy12.a92d1471",
            "type": [
                "terraform_v1.4"
            ],
            "description": "terraform workspace stage test",
            "resource_group": "Default",
            "location": "us-east",
            "tags": [
                "environment:staging"
            ],
            "created_at": "2022-10-11T04:07:20.309871638Z",
            "created_by": "Nishu.Bharti1@ibm.com",
            "status": "INACTIVE",
            "failure_reason": "",
            "workspace_status_msg": {
                "status_code": "200",
                "status_msg": ""
            },
            "workspace_status": {
                "frozen": false,
                "locked": false
            },
            "template_repo": {
                "url": "https://github.com/KshamaG/tf_cloudless_sleepy",
                "branch": "v0.12",
                "commit_id": "62b45f3a3501f6afa5d4c705abc04e474ff23fab",
                "full_url": "https://github.com/KshamaG/tf_cloudless_sleepy/tree/v0.12",
                "has_uploadedgitrepotar": false
            },
            "template_data": [
                {
                    "id": "ff819de7-e084-4c",
                    "folder": "",
                    "compact": false,
                    "type": "terraform_v1.4",
                    "values_url": "https://us-east.schematics.cloud.ibm.com/v1/workspaces/us-east.workspace.teststagews-sleepy12.a92d1471/template_data/ff819de7-e084-4c/values",
                    "values": "",
                    "values_metadata": [
                        {
                            "default": "hello",
                            "description": "A sample_var to pass to the template.",
                            "name": "sample_var",
                            "type": "string"
                        },
                        {
                            "default": "0",
                            "description": "How long our local-exec takes a nap.",
                            "name": "sleepy_time",
                            "type": "string"
                        }
                    ],
                    "variablestore": [
                        {
                            "name": "sample_var",
                            "secure": false,
                            "value": "THIS IS IBM CLOUD TERRAFORM CLI DEMO",
                            "type": "",
                            "description": "Description of sample_var"
                        },
                        {
                            "name": "sleepy_time",
                            "secure": false,
                            "value": "40",
                            "type": "string",
                            "description": ""
                        }
                    ],
                    "has_githubtoken": false
                }
            ],
            "runtime_data": [
                {
                    "id": "ff819de7-e084-4c",
                    "engine_name": "terraform",
                    "engine_version": "v1.0.11",
                    "state_store_url": "https://us-east.schematics.cloud.ibm.com/v1/workspaces/us-east.workspace.teststagews-sleepy12.a92d1471/runtime_data/ff819de7-e084-4c/state_store",
                    "log_store_url": "https://us-east.schematics.cloud.ibm.com/v1/workspaces/us-east.workspace.teststagews-sleepy12.a92d1471/runtime_data/ff819de7-e084-4c/log_store"
                }
            ],
            "shared_data": {
                "resource_group_id": ""
            },
            "applied_shareddata_ids": null,
            "updated_by": "Nishu.Bharti1@ibm.com",
            "updated_at": "2022-10-11T04:24:24.168476128Z",
            "last_health_check_at": "0001-01-01T00:00:00Z",
            "cart_id": "",
            "last_action_name": "WORKSPACE_CREATE",
            "last_activity_id": "e27b2c73cad6571d4abee1092065076d",
            "last_job": {
                "job_id": "e27b2c73cad6571d4abee1092065076d",
                "job_name": "WORKSPACE_CREATE",
                "job_status": ""
            }
        },
    ]
}
```
{: screen}

On successful workspace list, it returns the list of the workspace.

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-wks-create-api&interface=cli).

## Displaying the workspace list with Terraform
{: #list-wks-terraform}
{: terraform}

1. Follow the steps in [Setting up Terraform for {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-terraform-setup) to create your workspace with Terraform.

2. See [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to create, update, or delete {{site.data.keyword.cloud_notm}} resources with Terraform.

## Next steps
{: #sch-list-wks-nextsteps}

The next stage of working with workspace is [deploying workspaces](/docs/schematics?topic=schematics-sch-deploy-wks).
