---

copyright:
  years: 2017, 2025
lastupdated: "2025-11-03"

keywords: schematics workspaces, workspaces, schematics, delete workspace

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deleting a workspace
{: #sch-delete-wks}

Deletes a workspace from the {{site.data.keyword.bplong_notm}}. Deleting a workspace does not automatically remove the Cloud resources that the workspace manages. Use workspace destroy to remove all resources that are associated with the workspace.
{: shortdesc}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

{{site.data.keyword.bplong_notm}} deprecates creation of workspace using the {{site.data.keyword.terraform-provider_full_notm}} v1.2, v1.3 template from 2nd week of April 2024.
{: important}

## Before you begin
{: #prerequisites-delete}

- [Create a Terraform configuration](/docs/schematics?topic=schematics-create-tf-config), and store the configuration in a `GitHub`, `GitLab`, or `Bitbucket` repository. You can also upload a tape archive file (`.tar`) from your local workstation to provide your template to {{site.data.keyword.bplong_notm}}. For more information, see the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command and see the [upload a `tar` file to your workspace](/apidocs/schematics/schematics#template-repo-upload) API.
- Make sure that you have the [permissions](/docs/schematics?topic=schematics-access) to create a workspace.

Ensure the `location` and the `url` endpoint are pointing to the same region when you create or update the {{site.data.keyword.bpshort}} workspaces and actions. For more information about location and endpoint, see [Where is your information stored](/docs/schematics?topic=schematics-secure-data#pi-location)?
{: note}

## Deleting the workspace through UI
{: #delete-wks-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![hamburger icon](../images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > **Terraform**.
    - In **Terraform List**:
        - Click required **Workspace** to delete.
        - Select **Actions** > **Delete workspace**.
        - Click `Next`.
    - In **Delete workspace** dialog box.  Enter a name of your `workspace name` for confirmation before delete.
    - Click `Delete`. Your workspace can be deleted with a **Draft**, **Inactive**, **Active** state.

### Verifying workspace delete
{: #verify-wks-delete-ui}

1. Click your workspace that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/automation/schematics/terraform){: external} to view the results of the destroyed operation.
2. Click **Workspaces** tab to see the workspace list.
3. Type **Workspace** name in the search box to get confirmation about your workspace is deleted.

## Deleting the workspace through CLI
{: #delete-wks-cli}
{: cli}

1. Delete workspace configuration. For more about deleting the workspace, see the [`ibmcloud schematics workspace delete` command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-delete).

    ```sh
    ibmcloud schematics workspace delete --id <WORKSPACE_ID>
    ```
    {: pre}

2. Verify that your workspace is deleted.

    ```sh
    ibmcloud schematics workspace list
    ```
    {: pre}

3. Refer to, [Managing Cloud resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start Deleting, updating, or deleting Cloud resources with Terraform.

### Verifying workspace delete
{: #verify-wks-delete-cli}

Verify that the workspace are created successfully. When you destroy the resource by using the CLI, the command deletes the workspace completely.

```sh
ibmcloud schematics workspace delete  --id <WORKSPACE_ID>
```
{: pre}

```text
  Do you really want to delete the workspace us-east.workspace.testwspace03jan.811182d2? [y/N]> y
  Successfully deleted the workspace.
  OK
```
{: screen}

On successful deleting, it returns the Successfully deleted the workspace confirmation.

For more information, see [troubleshooting section](/docs/schematics?topic=schematics-wks-create-api&interface=cli).


## Deleting the workspace through API
{: #delete-wks-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.

2. Delete the workspace.

   Example

   ```json
   DELETE /v1/workspaces/us-east.workspace.testwspace03jan.4abe4795 HTTP/1.1
   Host: schematics.cloud.ibm.com
   Content-Type: application/json
   Authorization: <auth_token>
   Cache-Control: no-cache
   Postman-Token: 420968d1-2874-2b40-d278-edc59b526eb0

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

3. See [Managing Cloud resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to deploy, update, or delete Cloud resources with Terraform.

## Deleting the workspace with Terraform
{: #delete-wks-terraform}
{: terraform}

1. Follow the steps in [Deleting {{site.data.keyword.bpshort}} data](/docs/schematics?topic=schematics-delete-schematics-data-intro&interface=ui) to delete your workspace with Terraform.

2. See [Managing Cloud resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to create, update, and delete Cloud resources with Terraform.

## Next steps
{: #sch-delete-wks-nextsteps}

- Looking for more workspace samples to work with? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-template#sample){: external}. Check the `Readme` files of the examples for further customization, and usage for each sample.
