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

A customer uses {{site.data.keyword.bpshort}} workspace through `Terraform v1.x.x` since two years. Now the customer's organization requires their Infrastructure as Code (IaC) to migrate to `Tofu v1.6`. To meet an organization's requirement customer need to learn how to use Tofu?, migrate the Terraform to Tofu, and rollback from Tofu to Terraform by using {{site.data.keyword.bpshort}}.
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
3. Generate [`ibm schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) must display `no infrastructure changes`. If generate [`ibm schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) displays `pending changes`, you might experience unexpected issues when migrating your workspace using the Terraform to Tofu.
4. Backup your existing Terraform state file:

    a. Run [`ibmcloud schematics workspace get`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-get) command to fetch the `TEMPLATE_ID`.

    ```sh
    ibmcloud schematics workspace get --id WORKSPACE_ID
    ```
    {: pre}

    b. Run [`ibmcloud schematics state pull`](/docs/schematics?topic=schematics-schematics-cli-reference#state-pull) command to view and save the details of the state file.

    ```sh
    ibmcloud schematics state pull --id WORKSPACE_ID --template TEMPLATE_ID 
    ```
    {: pre}

## Migrate using {{site.data.keyword.bpshort}} CLI
{: #migrate-wks-tofu-cli}
{: cli}

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

3. Validate the workspace by running the [`ibmcloud schematics workspace get`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-get) command.

4. Verify your changes are applied by using the [`ibm schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) command.

5. Run the [`ibm schematics apply`](/docs/schematics?topic=schematics-sch-deploy-wks&interface=cli) to provision your resource. For more information, see [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle).

## Rollback your workspace using Tofu to Terraform
{: #rollback-wks-tf-tfrm}

You can achieve rollback only by using CLI commands. In case of any issues during rollback. You cannot rollback on the same workspace. You need to create a new workspace by using your existing state file.
{: important}

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

    Use the state file path that you have saved during backup.
    {: important}

    ```sh
    ibmcloud schematics workspace new  --file FILE_NAME  --state STATE_FILE_PATH 
    ```
    {: pre}

3. Validate the workspace by using the [`ibmcloud schematics workspace get`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-get) command.

4. Verify your changes are applied by using the [`ibm schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) command.

5. Run [`ibm schematics apply`](/docs/schematics?topic=schematics-sch-deploy-wks&interface=cli) to provision your resource. For more information, see [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle).

6. You may [Delete the {{site.data.keyword.bpshort}} workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-delete) that contains the Tofu.


## Before you begin using API
{: #prerequisites-create-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.
2. You must have {{site.data.keyword.bpshort}} [Workspace created](/apidocs/schematics/schematics#create-workspace) by using `Terraform v1.x.x`.
3. Make sure that you apply all the changes by using [`{{site.data.keyword.bpshort}} apply job`](/apidocs/schematics/schematics#apply-workspace-command).
4. Generate [`{{site.data.keyword.bpshort}} plan job`](/apidocs/schematics/schematics#plan-workspace-command) should display `no infrastructure changes`. If generate [`{{site.data.keyword.bpshort}} plan job`](/apidocs/schematics/schematics#apply-workspace-command) displays `pending changes`, you might experience unexpected issues when migrating your workspace using the Terraform to Tofu.
5. Backup your existing Terraform state file:

   a. Run [`ibmcloud schematics workspace get`](/apidocs/schematics/schematics#get-workspace) API to fetch and save the state file.
   
    ```sh
    GET  http://<endpoint>/v2/jobs/{job_id}/files?file_type=state_file
    ```
    {: pre}

   b. To get the `job_id` for the `Apply Plan` job, run the [Get workspace actions](https://<endpoint>/v1/workspaces/{workspace-id}/actions) API.

    ```sh
    GET https://<endpoint>/v1/workspaces/{workspace-id}/actions
    ```
    {: pre}

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

3. Validate the workspace by using the [Get workspace details](/apidocs/schematics/schematics#get-workspace) API.

4. Verify your changes by using the [`ibm schematics plan`](/apidocs/schematics/schematics#plan-workspace-command) API.

5. Run the [`ibm schematics apply`](/apidocs/schematics/schematics#apply-workspace-command) to provision your resource. For more information, see [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle).
