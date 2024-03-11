---

copyright:
  years: 2017, 2024
lastupdated: "2024-03-11"

keywords: schematics workspaces, workspaces, migrate terraform tofu, tofu, 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Migrating Terraform template to support Tofu
{: #sch-migrate-tfwks-tofuwks}

A customer is using Terraform v1.4.x template since 3 years. Now, the customer says that the organization is moving to Tofu v1.6.x and the customer wants to learn how to use Tofu and  migrate Terraform template by using {{site.data.keyword.bpshort}}. The following steps allow the customer to migrate the Terraform template to Tofu by using {{site.data.keyword.bpshort}} CLI, and API.
{: shortdesc}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

{{site.data.keyword.bplong_notm}} deprecates creation of workspace using the {{site.data.keyword.terraform-provider_full_notm}} v1.2, v1.3, v1.4 template from 2nd week of April 2024.
{: important}

## Before you begin using CLI
{: #prerequisites-create-cli}
{: cli}

- You must have {{site.data.keyword.bpshort}} [workspaces created](/docs/schematics?topic=schematics-sch-create-wks&interface=api#prerequisites-create) by using Terraform v1.4.x template.
- Make sure that you apply all changes by using [ibm schematics plan](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan). The [ibm schematics apply](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-apply) must result as `no planned changes`.
{: shortdesc}

If [`ibm schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) results as `pending changes`, you cannot migrate the Terraform template to Tofu.
{: note}

- Backup your existing Terraform state file

    Take a backup of your Terraform state file and save it as a file by using [ibmcloud schematics workspace get](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-get) command to fetch the `TEMPLATE_ID` from the workspace response. And run [ibmcloud schematics state pull](/docs/schematics?topic=schematics-schematics-cli-reference#state-pull) command to view the details of the state file.

    Example

    ```sh
    ibmcloud schematics workspace get --id WORKSPACE_ID [--output OUTPUT][--json]
    ```
    {: pre}

    ```sh
    ibmcloud schematics state pull --id WORKSPACE_ID --template TEMPLATE_ID
    ```
    {: pre}

## Migrate using {{site.data.keyword.bpshort}} CLI
{: #migrate-wks-tofu-cli}
{: cli}

Follow the steps to migrate the Terraform template to Tofu by using {{site.data.keyword.bpshort}}.
{: shortdesc}

1. Create a JSON `migratetotofu.json` file to include `type` field in the `template_data` block as shown in the example. And run the [update workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update) command.

    Example
    
    ```json
    {
    "template_data": [
            {
                "type": "tofu_v1.6",
            }
    ]
    }
    ```
    {: codeblock}

    ```sh
    ibmcloud schematics workspace update --id WORKSPACE_ID --file FILE_NAME
    ```
    {: pre}

2. Validate the workspace by using the [ibm schematics plan](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan).

3. [Delete the workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-delete) that has the Terraform template. (Optional)

## Rollback to Terraform using CLI
{: #rollback-wks-tf-cli}
{: cli}

Follow the steps to rollback from Tofu to Terraform template by using {{site.data.keyword.bpshort}}.
{: shortdesc}

1. Create a `rollback-terraform.json` as shown in the example.

    Example 

    ```json
    {
    "template_data": [
            {
                "type": "terraform_v1.x",
            }
    ]
    }
    ```
    {: codeblock}

2. Run the [ibmcloud schematics workspace new](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) by using `rollback-terraform.json` as shown in the example.

    ```sh
    ibmcloud schematics workspace new  --file FILE_NAME  --state STATE_FILE_PATH 
    ```
    {: pre}

3. Validate the workspace by using the [ibm schematics plan](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan).

4. [Delete the workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-delete) that has the Tofu template.


## Before you begin using API
{: #prerequisites-create-api}
{: api}

- - You must have {{site.data.keyword.bpshort}} [workspaces created](/apidocs/schematics/schematics#create-workspace) by using Terraform v1.4.x template.
- Make sure that you apply all changes by using [{{site.data.keyword.bpshort}} plan job](/apidocs/schematics/schematics#plan-workspace-command). The [{{site.data.keyword.bpshort}} apply job](/apidocs/schematics/schematics#apply-workspace-command) must result as `no planned changes`.
{: shortdesc}

If [{{site.data.keyword.bpshort}} plan job](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) results pending changes, you cannot migrate the Terraform template to Tofu.
{: note}

- Take a backup of your existing Terraform state file

    Take a backup of your Terraform state file and save it as a file by using [Get workspace details](/apidocs/schematics/schematics#get-workspace) to fetch the `TEMPLATE_ID` from the workspace response. And run [ibmcloud schematics state pull](/docs/schematics?topic=schematics-schematics-cli-reference#state-pull) command.

    Example

    ```sh
    GET  http://schematics.cloud.ibm.com/v2/jobs/{job_id}/files?file_type=state_file
    ```
    {: pre}

    To get the `job_id` for the `Apply` and `Plan` job, run the `GET` workspace API by using the [endpoint](/apidocs/schematics/schematics#api-endpoints).

## Migrate using {{site.data.keyword.bpshort}} API
{: #migrate-wks-tofu-api}
{: api}

Follow the steps to migrate the Terraform template to Tofu by using {{site.data.keyword.bpshort}}.
{: shortdesc}

1. Include `type` field in the `template_data` block as shown in the example in your request. And run the [Update workspace](/apidocs/schematics/schematics#replace-workspace).

    Example 

    ```json
    {
    "template_data": [
            {
                "type": "tofu_v1.6",
            }
    ]
    }
    ```
    {: pre}


    ```sh
    PUT https://schematics.cloud.ibm.com/v1/workspaces/{id}
    ```
    {: pre}

2. Validate the workspace by using the [Get workspace details](/apidocs/schematics/schematics#get-workspace). 

3. [Delete a workspace](/apidocs/schematics/schematics#delete-workspace) that has the Terraform template.

## Rollback to Terraform using API
{: #rollback-wks-tf-api}
{: api}

Follow the steps to rollback from Tofu to Terraform template by using {{site.data.keyword.bpshort}}.
{: shortdesc}

1. Include `type` field in the `template_data` block as shown in the example in your request. And run the [Update workspace](/apidocs/schematics/schematics#replace-workspace). Run the [Create a workspace](/apidocs/schematics/schematics#create-workspace) in your request as shown in the example.

    Example

    ```curl
    curl --request POST --url https://us.schematics.test.cloud.ibm.com/v1/workspaces -H "Authorization: <iam_access_token>" -d '{
    "name": "<workspace_name>",
    "type": [
        "NOT_SET"
    ],
    "location": "<location>",
    "resource_group": "Default",
    "description": "<description>",
    "template_repo": {
        "url": "<github_source_repo_url>",
        "branch": "<branch_name>"
    },
    "template_data": [
        {
            "folder": ".",
            "type": "terraform_v1.x"
        }
        ]
    }'
    ```
    {: codeblock}

2. Validate the workspace by using the [Get workspace details](/apidocs/schematics/schematics#get-workspace).

3. [Delete a workspace](/apidocs/schematics/schematics#delete-workspace) that has the Tofu template.
