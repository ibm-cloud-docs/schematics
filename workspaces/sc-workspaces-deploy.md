---

copyright:
  years: 2017, 2024
lastupdated: "2024-11-13"

keywords: schematics workspaces, workspaces, schematics, deploy workspace

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Running a workspace apply
{: #sch-deploy-wks}

{{site.data.keyword.bpshort}} uses workspaces to deploy and manage resources in {{site.data.keyword.cloud}} accounts.

{{site.data.keyword.bplong_notm}} enables you to deploy and manage resources in any {{site.data.keyword.cloud_notm}} location or region globally. The region where you create and work with your {{site.data.keyword.bpshort}} workspaces and actions is independent of the region where your {{site.data.keyword.cloud_notm}} resources are deployed or configured.
{: shortdesc}

{{site.data.keyword.bplong_notm}} runs your jobs from the {{site.data.keyword.bpshort}} region hosting the workspace and remotely accesses services to provision resources in the target regions determined by your Terraform templates. Workspace operations in remote regions are unaffected by network latency between the management and target regions.

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

{{site.data.keyword.bplong_notm}} deprecates creation of workspace using the {{site.data.keyword.terraform-provider_full_notm}} v1.2, v1.3 template from 2nd week of April 2024.
{: important}

## Before you begin
{: #deploy-prerequisites}

- [Create a Terraform configuration](/docs/schematics?topic=schematics-create-tf-config), and store the configuration in a `GitHub`, `GitLab`, or `Bitbucket` repository. You can also upload a tape archive file (`.tar`) from your local workstation to provide your template to {{site.data.keyword.bplong_notm}}. For more information, see the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command and see the [upload a `tar` file to your workspace](/apidocs/schematics/schematics#template-repo-upload) API.
- Make sure that you have the [permissions](/docs/schematics?topic=schematics-access) to create a workspace.

Ensure the `location` and the `url` endpoint are pointing to the same region when you list the {{site.data.keyword.bpshort}} workspaces and actions. For more information about location and endpoint, see [Where is your information stored](/docs/schematics?topic=schematics-secure-data#pi-location)?

- Run a {{site.data.keyword.bpshort}} apply job against your workspace. An apply job provisions, modifies, or removes the {{site.data.keyword.cloud_notm}} resources that you describe in the Terraform template that your workspace points. Depending on the type and number of resources that you want to provision or modify, this process might take a few minutes, or hours to complete. During this time, you cannot edit your workspace. After all updates are applied, the state of the files persists to determine what resources exist in your {{site.data.keyword.cloud_notm}} account.
{: note}

## Perform a workspace apply using the UI 
{: #deploy-wks-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![hamburger icon](../images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > [**Terraform**](https://cloud.ibm.com/automation/schematics/terraform){: external}.
3. Search your workspace in specific location and click your workspace name.
4. Click **Apply plan** to provision the configured resources.

### Verifying workspace apply 
{: #verify-wks-deploy-ui}

1. Click your workspace that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/automation/schematics/terraform){: external} to view the results of the workspace apply job.

## Perform a workspace apply using the CLI
{: #deploy-wks-cli}
{: cli}

1. Create a JSON file on your system to pass the parameters to {{site.data.keyword.bpshort}} to run the workspace apply. For more information about configuration options, see [`ibmcloud schematics workspace new`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) command.

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
	ibmcloud schematics apply --id <Provide your workspace ID>
	```
	{: pre}

2. Apply the workspace. For more information about the apply argument flag, see [`ibmcloud schematics apply`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-apply) command.

    ```sh
    ibmcloud schematics apply --id <workspace_id>
    ```
    {: pre}

3. Verify that your workspace apply has been executed successfully. Make sure that your workspace is in an **Inactive** state.

	```sh
    ibmcloud schematics workspace list
    ```
    {: pre}

4. Refer to, [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to view job logs .

### Verifying workspace deploy 
{: #verify-wks-deploy-cli}

Execute CLI command to check the status of the workspace deploy is success.

	```text
	ibmcloud schematics apply --id us-east.workspace.testwspace01feb.b5e8fdaa
	Do you really want to perform this action? [y/N]> Y

	Activity ID   df51c0e61a020592d3403a05d08692d7   

	OK
	```
	{: screen}

On successful plan it returns the update details of an existing workspace.

For more information about FAQ, see [troubleshooting section](/docs/schematics?topic=schematics-wks-create-api&interface=cli).

## Perform the workspace deploy using the API
{: #deploy-wks-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.

2. Perform deploy for the existing workspace.

	Example

	```json

	PUT /v1/workspaces/{w_id}/apply HTTP/1.1
	Host: schematics.cloud.ibm.com
	Content-Type: application/json
	Authorization: <auth-token>
	Cache-Control: no-cache
	Postman-Token: 77ef7f05-7290-0b60-781c-f8c147c78ed2

	{
		"name": "testworkspacejanATTest",
		"type": [
			"terraform_v1.4"
		],
		"description": "terraform workspace",
		"tags": [
			"test:bbbranch"
		],
		"template_repo": {
			"url": "https://github.com/srikar-git/tf_cloudless_sleepy/tree/v0.13"
		},
		"template_data": [{
			"folder": ".",
			"type": "terraform_v1.4",
		
		"variablestore": [
		{
		"value": "12",
		"name": "sleepy_time",
		"type": "string"
		}
	]
		}]
	}
	```
	{: codeblock}

3. Verify that the workspace deploy list all the workspace jobs that are created.

    ```sh
    curl -X GET https://schematics.cloud.ibm.com/v1/workspaces -H "Authorization: <iam_access_token>"
    ```
    {: pre}

4. see [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start creating, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.


### Verifying workspace deploy
{: #verify-wks-deploy-api}

Verify that the workspace are deployed successfully as shown in the output.
{: shortdesc}

	Output

	```text
	{
		"activityid": "a75d99109c80291faead70225b5409ee"
	}
	```
	{: screen}

On successful workspace apply, it returns the job activity id performed on the workspace. For more information, see [troubleshooting section](/docs/schematics?topic=schematics-wks-create-api&interface=cli).

## Perform the workspace deploy with Terraform
{: #deploy-wks-terraform}
{: terraform}

1. Follow the steps in [Setting up Terraform for {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-terraform-setup) to create your workspace with Terraform.

2. See [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to create, update, or delete {{site.data.keyword.cloud_notm}} resources with Terraform.

## Next steps
{: #sch-deploy-wks-nextsteps}

To manage your workspace refer to, [deploying workspace](/docs/schematics?topic=schematics-sch-deploy-wks), [destroying workspace](/docs/schematics?topic=schematics-sch-destroy-wks&interface=ui), and [deleting workspace](/docs/schematics?topic=schematics-sch-delete-wks&interface=ui).
