---

copyright:
  years: 2017, 2024
lastupdated: "2024-03-12"

keywords: schematics workspaces, workspaces, migrate terraform tofu, tofu, 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Migrating workspace using Terraform to Tofu
{: #sch-migrate-tfwks-tofuwks}

A customer uses {{site.data.keyword.bpshort}} workspace that supports `Terraform v1.x.x` since two years. Now the customer's organization wants their Infrastructure as Code (IaC) to migrate to `Tofu v1.6`. To meet an organization's requirement customer wants to learn how to use Tofu?, migrate the Terraform to Tofu, and rollback from Tofu to Terraform by using {{site.data.keyword.bpshort}}.
{: shortdesc}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

{{site.data.keyword.bplong_notm}} deprecates creation of workspace using the {{site.data.keyword.terraform-provider_full_notm}} v1.2, v1.3, v1.4 template from 2nd week of April 2024.
{: important}

## Before you begin using CLI
{: #prerequisites-create-cli}
{: cli}

1. You must have {{site.data.keyword.bpshort}} [Workspace created](/docs/schematics?topic=schematics-sch-create-wks&interface=api#prerequisites-create) by using Terraform `v1.x.x`.
2. Make sure that you apply all the changes by using [`ibm schematics apply`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-apply). 
3. The [`ibm schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) must result as `no infrastructure changes`. If [`ibm schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) results as `pending changes`, you might experience unexpected issues when migrating your workspace using the Terraform state file to Tofu.
4. Backup your existing Terraform state file
    a. Take a backup of your Terraform state file and save as a file by using [`ibmcloud schematics workspace get`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-get) command to fetch the `TEMPLATE_ID` from the workspace response.

    ```sh
    ibmcloud schematics workspace get --id WORKSPACE_ID
    ```
    {: pre}

    b. Run [`ibmcloud schematics state pull`](/docs/schematics?topic=schematics-schematics-cli-reference#state-pull) command to view the details of the state file.

    ```sh
    ibmcloud schematics state pull --id WORKSPACE_ID --template TEMPLATE_ID
    ```
    {: pre}

## Migrate using {{site.data.keyword.bpshort}} CLI
{: #migrate-wks-tofu-cli}
{: cli}

Follow the steps to migrate the {{site.data.keyword.bpshort}} workspace using Terraform to Tofu.
{: shortdesc}

1. Create a JSON `migratetotofu.json` file to include `type` field in the `template_data` block as shown in the example.
    
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

2. Run the [Update workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update) command.

    ```sh
    ibmcloud schematics workspace update --id WORKSPACE_ID --file <PATH OF THE STATE_FILE_NAME>
    ```
    {: pre}

3. Validate the workspace by using the [`ibm schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan).

3. Run [`ibm schematics apply`](/docs/schematics?topic=schematics-sch-deploy-wks&interface=cli) to provision your resource. For more information, see [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle).

## Rollback to Terraform using CLI
{: #rollback-wks-tf-cli}
{: cli}

1. Create a `rollback-terraform.json` as shown in the example.

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

2. Run the [`ibmcloud schematics workspace new`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) by using `rollback-terraform.json` file and state file.

    ```sh
    ibmcloud schematics workspace new  --file FILE_NAME  --state STATE_FILE_PATH 
    ```
    {: pre}

3. Validate the workspace by using the [`ibm schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan).

4. Run [`ibm schematics apply`](/docs/schematics?topic=schematics-sch-deploy-wks&interface=cli) to provision your resource. For more information, see [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle).

5. Optional, You can [Delete the {{site.data.keyword.bpshort}} workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-delete) that contains Tofu engine.


## Before you begin using API
{: #prerequisites-create-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.
2. You must have {{site.data.keyword.bpshort}} [Workspace created](/apidocs/schematics/schematics#create-workspace) by using `Terraform v1.x.x`.
3. Make sure that you apply all the changes by using [`{{site.data.keyword.bpshort}} apply job`](/apidocs/schematics/schematics#apply-workspace-command).
4. [`{{site.data.keyword.bpshort}} plan job`](/apidocs/schematics/schematics#plan-workspace-command) must result as `no infrastructure changes`. If [`{{site.data.keyword.bpshort}} plan job`](/apidocs/schematics/schematics#apply-workspace-command) results as `pending changes`, you might experience unexpected issues when migrating your workspace using the Terraform state file to Tofu.
5. Backup your existing Terraform state file
   a. Take a backup of your Terraform state file and save as a file by using [`ibmcloud schematics workspace get`](/apidocs/schematics/schematics#get-workspace).
   
    ```sh
    GET  http://<endpoint>/v2/jobs/{job_id}/files?file_type=state_file
    ```
    {: pre}

   To get the `job_id` for the `Apply` and `Plan` job, run the [Get workspace actions API](https://<endpoint>/v1/workspaces/{workspace-id}/actions).
   {: note}

## Migrate using {{site.data.keyword.bpshort}} API
{: #migrate-wks-tofu-api}
{: api}

1. Include `type` field in the `template_data` block in your request as shown in the example.

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

2. Run the [Update workspace](/apidocs/schematics/schematics#replace-workspace).

    ```sh
    PUT https://<endpoint>/v1/workspaces/{id}
    ```
    {: pre}

3. Validate the workspace by using the [Get workspace details](/apidocs/schematics/schematics#get-workspace).

4. Run the [`ibm schematics plan`](/apidocs/schematics/schematics#plan-workspace-command).

5. Run the [`ibm schematics apply`](/apidocs/schematics/schematics#apply-workspace-command) to provision your resource. For more information, see [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle).

## Rollback to Terraform using API
{: #rollback-wks-tf-api}
{: api}

1. Include `type` field in the `template_data` block, and run the [Create a workspace](/apidocs/schematics/schematics#create-workspace) in your request as shown in the example.

    ```curl
    curl --request POST --url https://<endpoint>/v1/workspaces -H "Authorization: <iam_access_token>" -d '{
    "name": "<workspace_name>",
    "type": [
        "terraform_v1.x"
    ],
    "location": "<location>",
    "resource_group": "<resource_group>",
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

3. Run the [`ibm schematics plan`](/apidocs/schematics/schematics#plan-workspace-command).

4. Run the [`ibm schematics apply`](/apidocs/schematics/schematics#apply-workspace-command) to provision your resource. For more information, see [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle).

5. Optional, you can [Delete the workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-delete) that contains Terraform engine.
