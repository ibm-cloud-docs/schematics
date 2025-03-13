---

copyright:
  years: 2017, 2025
lastupdated: "2025-03-13"

keywords: schematics workspaces, workspaces, schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Running a workspace plan
{: #sch-plan-wks}

A workspace plan, performs a Terraform plan to determine the {{site.data.keyword.cloud}} resources that are created, modified or deleted on any subsequent workspace apply operation. Run the {{site.data.keyword.bpshort}} plan job against your workspace. You can use the plan summary logs to verify any resource changes before the template is applied.
{: shortdesc}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

{{site.data.keyword.bplong_notm}} deprecates creation of workspace using the {{site.data.keyword.terraform-provider_full_notm}} v1.2, v1.3 template from 2nd week of April 2024.
{: important}

## Before you begin
{: #plan-prerequisites}

- [Create a Terraform configuration](/docs/schematics?topic=schematics-create-tf-config), and store the configuration in a `GitHub`, `GitLab`, or `Bitbucket` repository. You can also upload a copy of the repo as a `tar` (tape archive file) from your local workstation to provide your template to {{site.data.keyword.bplong_notm}}. For more information, see the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command and see the [upload a `tar` file to your workspace](/apidocs/schematics/schematics#template-repo-upload) API.
- Make sure that you have the [permissions](/docs/schematics?topic=schematics-access) to create a workspace.

Ensure the `location` and the `url` endpoint are pointing to the same region when you list the {{site.data.keyword.bpshort}} workspaces and actions. For more information about location and endpoint, see [Where is your information stored](/docs/schematics?topic=schematics-secure-data#pi-location)?

- Run a {{site.data.keyword.bpshort}} plan job against your workspace. An plan job calculates which resources are provisioned, modified, or removed. This process might take a few minutes.

During workspace plan execution, you cannot edit your workspace.
{: note}

## Generate a workspace plan using the UI 
{: #plan-wks-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![hamburger icon](../images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > [**Terraform**](https://cloud.ibm.com/automation/schematics/terraform){: external}.
3. Click your workspace name.
4. Click **Generate Plan** to create a plan for the workspace.

### Verifying workspace plan 
{: #verify-wks-plan-ui}

1. Click on your workspace that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/automation/schematics/terraform){: external} 
2. Click **Jobs** to see the job execution results. It are listed under the heading `Generate Plan`
3. On a successful plan, the cost for the proposed changes is reviewed by clicking on the `Cost Estimate` button. For more information, see [Infrastructure cost estimation](/docs/schematics?topic=schematics-cost-estimation).

## Generate a workspace plan using the CLI
{: #plan-wks-cli}
{: cli}

1. Create a JSON file on your system and plan your workspace configuration. For more information about configuration options, see [`ibmcloud schematics workspace new`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) command.

	Example

    ```json
		{
		"name": "testwspace31jan",
		"type": [
			"terraform_v1.4"
		],
		"description": "terraform workspace",
		"location": "us-east",
		"tags": [
			"department:HR",
			"application:compensation",
			"environment:production"
		],
		"template_repo": {
			"url": "https://github.com/Anil-CM/newrepo"
		},
		"workspace_status": {
			"frozen": true
		},
		"template_data": [{
			"folder": ".",
			"type": "terraform_v1.4",
			"variablestore": [{
					"name": "sample_var",
					"secure": true,
					"value": "THIS IS IBM CLOUD TERRAFORM CLI DEMO",
					"description": "Description of sample_var"
				},
				{
					"name": "sleepy_time",
					"value": "15"
				}
			]
		}]
	}
	```
	{: codeblock}

	Syntax

    ```sh
	ibmcloud schematics plan --id WORKSPACE_ID [--output OUTPUT] [--var-file PATH_TO_VARIABLES_FILE] [--var-file PATH_TO_VARIABLES_FILE] []
	```
	{: pre}

	For more information about syntax and arguments flags, see [`ibmcloud schematics plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) command.
	{: note}

2. Verify that your workspace plan is applied. Make sure that your workspace is in an **Inactive** state.

    ```sh
    ibmcloud schematics workspace list
    ```
    {: pre}

3. Refer to, [Managing Cloud resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to view job logs.

### Verifying workspace plan execution
{: #verify-wks-plan-cli}

Execute CLI command to check the status of the workspace plan is success.

    ```sh
    ibmcloud schematics workspace list
    ```
    {: pre}

	```text
	Retrieving workspaces...
	Name                                          ID                                                                       Description                                   Version             Status           Frozen
	testwspace31jan                               us-east.workspace.testwspace31jan.a31438c6                               terraform workspace                           Terraform v1.0.11   INACTIVE         True

	OK
	```
	{: screen}

On successful plan returns the update details of an existing workspace.

For more information about FAQ, see [troubleshooting section](/docs/schematics?topic=schematics-wks-create-api&interface=cli).


## Generate a workspace plan using the API
{: #plan-wks-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.

2. Generate plan for the existing workspace. 

	Example

	```json

	POST /v1/workspaces/{w_id}/plan HTTP/1.1
	Host: schematics.cloud.ibm.com
	Content-Type: application/json
	Authorization: <auth-token>
	Cache-Control: no-cache
	Postman-Token: bdc869fd-dc7c-06a6-5e99-4de6e3aa7dd9

	{
		"name": "testwspace31jan",
		"type": [
			"terraform_v1.4"
		],
		"description": "terraform workspace",
		"location": "us-east",
		"tags": [
			"department:HR",
			"application:compensation",
			"environment:production"
		],
		"template_repo": {
			"url": "<gitrepo-url>"
		},
		"workspace_status": {
			"frozen": true
		},
		"template_data": [{
			"folder": ".",
			"type": "terraform_v1.4",
			"variablestore": [{
					"name": "sample_var",
					"secure": true,
					"value": "THIS IS IBM CLOUD TERRAFORM CLI DEMO",
					"description": "Description of sample_var"
				},
				{
					"name": "sleepy_time",
					"value": "15"
				}
			]
		}]
	}

	```
	{: codeblock}

3. See [Managing Cloud resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to create, update, or delete Cloud resources with Terraform.

### Verifying workspace plan execution
{: #verify-wks-plan-api} 

Verify that the workspace plan is successfully listed with the list of workspace jobs that were created.
{: shortdesc}

```sh
curl -X GET https://schematics.cloud.ibm.com/v1/workspaces -H "Authorization: <iam_access_token>"
```
{: pre}

Output

```text
{
    "activityid": "3815ef757cd34030bc43191f7d7c6744"
}
```
{: screen}

On successful, returns the list of the workspace.

For more information, see [FAQ](/docs/schematics?topic=schematics-workspaces-faq), and [troubleshooting section](/docs/schematics?topic=schematics-wks-create-api).

## Generating a workspace plan using Terraform
{: #plan-wks-terraform}
{: terraform}

1. Follow the steps in [Setting up Terraform for {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-terraform-setup) to create your workspace with Terraform.

2. See [Managing Cloud resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start creating, updating, or deleting Cloud resources with Terraform.

## Next steps
{: #sch-plan-wks-nextsteps}

The next stage of working with workspace is [deploying workspaces](/docs/schematics?topic=schematics-sch-deploy-wks).

For more information, see [FAQ](/docs/schematics?topic=schematics-workspaces-faq), and [troubleshooting section](/docs/schematics?topic=schematics-wks-create-api).
