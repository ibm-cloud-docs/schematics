---

copyright:
  years: 2017, 2024
lastupdated: "2024-11-13"

keywords: schematics workspaces, workspaces, schematics, destroy workspace

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Destroying workspace resources
{: #sch-destroy-wks}

Remove the {{site.data.keyword.cloud}} resources that you provisioned with your {{site.data.keyword.bpshort}} workspace, even if these resources are active.
{: shortdesc}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

{{site.data.keyword.bplong_notm}} deprecates creation of workspace using the {{site.data.keyword.terraform-provider_full_notm}} v1.2, v1.3 template from 2nd week of April 2024.
{: important}

## Before you begin
{: #prerequisites-destroy}

- [Create a Terraform configuration](/docs/schematics?topic=schematics-create-tf-config), and store the configuration in a `GitHub`, `GitLab`, or `Bitbucket` repository. You can also upload a tape archive file (`.tar`) from your local workstation to provide your template to {{site.data.keyword.bplong_notm}}. For more information, see the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command and see the [upload a `tar` file to your workspace](/apidocs/schematics/schematics#template-repo-upload) API.
- Make sure that you have the [permissions](/docs/schematics?topic=schematics-access) to create a workspace.

Ensure the `location` and the `url` endpoint are pointing to the same region when you create or update the {{site.data.keyword.bpshort}} workspace or action. For more information about location and endpoint, see [Where is your information stored](/docs/schematics?topic=schematics-secure-data#pi-location)?
{: note}

## Destroying workspace resources using the UI
{: #destroy-wks-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![hamburger icon](../images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > [**Terraform**](https://cloud.ibm.com/automation/schematics/terraform){: external}.
    - In **workspace List** section:
        - Click required **Workspace** to destroy a required workspace. If you do not see the required workspace in the list, check your navigation page.
        - Click `Next`.
    - In **Workspace list** section. Click workspace name and click **Actions** dropdown. Click `Destroy resources` and enter workspace name for confirmation before delete. Enter the name while creation and click destroy.
    - Click `Destroy`. Your resource of workspace are destroyed.

### Verifying a workspace destroy operation 
{: #verify-wks-destroy-ui}

1. Click your workspace that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/automation/schematics/terraform){: external} to view the results of the destroy operation.
2. Click **Jobs** tab to see the workspace logs.
3. Click **Jobs history** tab view the result of the destroy job and operations that were run by the automation modules.

## Destroying workspace resources using the CLI
{: #destroy-wks-cli}
{: cli}

1. Destroy workspace configuration. For more information about destroy the resource in workspace, see the [`ibmcloud schematics destroy workspace` command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-destroy).

    ```sh
    ibmcloud schematics destroy workspace --id <WORKSPACE_ID>
    ```
    {: pre}

    ```sh
    ibmcloud schematics destroy --id WORKSPACE_ID [--target RESOURCE1] [--target RESOURCE2] [--force] [--output OUTPUT]
    ```
    { pre}

2. Verify that your source created through workspace is destroyed.

    ```sh
    ibmcloud schematics workspace list
    ```
    {: pre}

3. Refer to, [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start Deleting, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.

### Verifying a workspace destroy operation
{: #verify-workspace-destroy-cli}

Verify that the workspace was created successfully. When you destroy the resource by using the CLI, the command removes the resources of the workspace.

```text
   ibmcloud schematics destroy --id us-east.workspace.testwspace03jan.811182d2 --target vpc_name --target vpc_tags
   Do you really want to perform this action? [y/N]> y
                     
   Activity ID   c10fc92ddfd2d9ec645fc5dbece5e341   
                     
   OK
```
{: screen}

On successful destroying, it returns the unique Activity ID of the workspace.

For more information, see [workspace FAQs](/docs/schematics?topic=schematics-workspaces-faq#clusterdeletion-warn-faq).

## Destroying workspace resources using the  API
{: #destroy-wks-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.

2. Destroy the workspace. 

   Example:

   ```json
      PUT /v1/workspaces/{w_id}/destroy HTTP/1.1
      Host: schematics.cloud.ibm.com
      Content-Type: application/json
      Authorization: <auth_token>

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

3. See [Managing {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle) to start Deleting, updating, or deleting {{site.data.keyword.cloud_notm}} resources with Terraform.

### Verifying a workspace destroy operation
{: #verify-workspace-destroy-api}

Verify that the resource is destroyed successfully.

Verify that the workspace resource is destroyed successfully as shown in the output.
{: shortdesc}

Output

```text
{
    "activityid": "6e5c84b58100472395b53a056cc27edc"
}
```
{: screen}

On successful, destroy the unique activity ID of the workspace is returned.

For more information, see [workspace FAQs](/docs/schematics?topic=schematics-workspaces-faq#clusterdeletion-warn-faq).

## destroying workspace resources with Terraform
{: #destroy-wks-terraform}
{: terraform}

1. Follow the steps in [Deleting {{site.data.keyword.bpshort}} data](/docs/schematics?topic=schematics-delete-schematics-data-intro&interface=ui) to destroy your workspace with Terraform.

2. See [Managing {{site.data.keyword.cloud_notm}} resources](/docs/schematics?topic=schematics-manage-lifecycle) with {{site.data.keyword.bpshort}} to create, update, or delete the {{site.data.keyword.cloud_notm}} resources.

## Next steps
{: #sch-destroy-wks-nextsteps}

- Looking for additional workspace samples to work with? Check out the [sample workspaces](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-template#sample). Check the `Readme` files of the examples for further customization and usage.
