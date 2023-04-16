---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-16"

keywords: schematics workspaces, workspaces, schematics, deploy workspace

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deploying workspace
{: #sch-deploy-wks}

The {{site.data.keyword.bpshort}} workspace helps deploying resources in {{site.data.keyword.cloud}} account.

{{site.data.keyword.bplong_notm}} enable you to deploy resources in any {{site.data.keyword.cloud_notm}} location or region globally. The region where you save and run your {{site.data.keyword.bpshort}} workspaces and actions is independent of the region where your {{site.data.keyword.cloud_notm}} resources are deployed or configured.
{: shortdesc}

{{site.data.keyword.bplong_notm}} runs your jobs from the selected {{site.data.keyword.bpshort}} region and remotely access the services to provision resources in the target regions determined by your Terraform templates. It is unaffected by network latency between regions.

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version).
{: deprecated}

## Before you begin
{: #display-prerequisites}

- [Create a Terraform configuration](/docs/schematics?topic=schematics-create-tf-config), and store the configuration in a `GitHub`, `GitLab`, or `Bitbucket` repository. You can also upload a tape archive file (`.tar`) from your local workstation to provide your template to {{site.data.keyword.bplong_notm}}. For more information, see the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command and see the [upload a `tar` file to your workspace](/apidocs/schematics/schematics#upload-template-tar) API. 
- Make sure that you have the [permissions](/docs/schematics?topic=schematics-access) to create a workspace. 

Ensure the `location` and the `url` endpoint are pointing to the same region when you list the {{site.data.keyword.bpshort}} Workspaces and actions. For more information about location and endpoint, see [Where is your information stored](/docs/schematics?topic=schematics-secure-data#pi-location)?

- Run a {{site.data.keyword.bpshort}} apply job against your workspace. An apply job provisions, modifies, or removes the {{site.data.keyword.cloud_notm}} resources that you describe in the Terraform template that your workspace points. Depending on the type and number of resources that you want to provision or modify, this process might take a few minutes, or hours to complete. During this time, you cannot edit your workspace. After all updates are applied, the state of the files persists to determine what resources exist in your {{site.data.keyword.cloud_notm}} account.
{: note}

## Generate the workspace deployment through UI 
{: #deploy-wks-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Access **Schematics** > **Workspaces** > [**Workspace**](https://cloud.ibm.com/schematics/workspaces){: external}.
3. Search your workspace in specific location.
4. Click **Workspaces** > [**workspace**](https://cloud.ibm.com/schematics/workspaces){: external}.
5. Click **Apply plan** to provision the configured resources.

### Verifying workspace deployment 
{: #verify-wks-list-ui}

1. Click your workspace that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/workspaces){: external} to view the results of the workspace details.

## Generate the workspace deployment through CLI
{: #deploy-wks-cli}
{: cli}

1. Create a JSON file on your system and plan your workspace configuration. For more information about configuration options, see [`ibmcloud schematics workspace new`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) command.

   Example

	```json
	{
		"name": "testwspace31jan",
		"type": [
			"terraform_v1.0"
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
			"type": "terraform_v1.0",
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

2. Apply the workspace. For more information about apply argument flag, see [`ibmcloud schematics apply`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-apply) command.

    ```sh
    ibmcloud schematics apply --id <workspace_id>
    ```
    {: pre}

3. Verify that your workspace deployment is applied. Make sure that your workspace is in an **Inactive** state.

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

## Generate the workspace deploy through API
{: #deploy-wks-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.

2. Generate deploy for the existing workspace. 

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
			"terraform_v0.13"
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
			"type": "terraform_v0.13",
		
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

On successful workspace deployment, it returns the job activity id performed on the workspace. For more information, see [troubleshooting section](/docs/schematics?topic=schematics-wks-create-api&interface=cli).

## Generate the workspace deploy with Terraform
{: #deploy-wks-terraform}
{: terraform}

1. Follow the steps in [Setting up Terraform for {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-terraform-setup) to create your workspace with Terraform.

2. See [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to create, update, or delete {{site.data.keyword.cloud_notm}} resources with Terraform.

## Next steps
{: #sch-deploy-wks-nextsteps}

To manage your workspace refer to, [deploying workspace](/docs/schematics?topic=schematics-sch-deploy-wks), [destroying workspace](/docs/schematics?topic=schematics-sch-destroy-wks&interface=ui), and [deleting workspace](/docs/schematics?topic=schematics-sch-delete-wks&interface=ui).
