---

copyright:
  years: 2017, 2025
lastupdated: "2025-01-17"

keywords: schematics faqs, infrastructure as code, iac, schematics workspaces faq, workspaces faq

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# Terraform
{: #workspaces-faq}

Answers to common questions about the {{site.data.keyword.bplong_notm}} workspaces are classified into the following section.
{: shortdesc}

## Does {{site.data.keyword.bpfull_notm}} support multiple Terraform provider versions?
{: #provider-versions}
{: faq}
{: support}

Yes, {{site.data.keyword.bpfull_notm}} supports multiple Terraform provider versions. You need to add the Terraform provider block with the provider version. By default the provider run current version `1.21.0`, and previous four versions such as `1.20.1`, `1.20.0`, `1.19.0`, `1.18.0` are supported.

Example for a multiple provider configuration:

```terraform
terraform{
    required_providers{
    ibm = ">= 1.21.0" // Error !! version unavailable.
    ibm = ">= 1.20.0" // Execute against latest version.
    ibm = "== 1.20.1" // Executes version v1.20.1. 
    }
}
```
{: codeblock}

Currently, version 1.21.0 is released. For more information, see [provider version](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-setup_cli#install_provider).
{: note}

## How do I update the Terraform version 
{: #migrate-terraform-v11}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version#deprecate-timeline).
{: deprecated}

{{site.data.keyword.bplong_notm}} deprecates creation of workspace using the {{site.data.keyword.terraform-provider_full_notm}} v1.2, v1.3 template from 2nd week of April 2024.
{: important}

You can follow the topics to upgrade from one Terraform version to another version

- [Upgrading the Terraform template version](/docs/schematics?topic=schematics-migrating-terraform-version)
- [Updating the workspace Terraform 1.x version](/docs/schematics?topic=schematics-migrating-terraform-version#terraform-version-upgrade1x-process)
- [Upgrade Terraform template from `v0.13` and higher to `v1.0`](/docs/schematics?topic=schematics-migrating-terraform-version#upgrade-13-to10)
- [Upgrading a Terraform v0.12 workspace to v0.13](/docs/schematics?topic=schematics-migrating-terraform-version#migrate-steps12)

## How do I `pull latest` code from a Git repo by using the command line? 
{: #latestcode-workspace-commandline}
{: faq}
{: support}

Updating the {{site.data.keyword.bplong}} workspaces through command line need the needed field `name`.

You need to run `ibmcloud schematics workspace update --id <workspace-id>  --file <updatefile.json>`  command. The sample `updatefile.json` contains the name field with the value.

```json
{
    "name":"testworkspace"
}
```
{: codeblock}

## What tools and utilities are used in the runtime?
{: #schematics-tools}
{: faq}
{: support}

{{site.data.keyword.bpshort}} runtime is built by using Universal Base Image (UBI-8) and the runtime `utilities/softwares` that come with the UBI-8 are available for Terraform provisions and Ansible actions. For more information, see the list of [tools and utilities](/docs/schematics?topic=schematics-sch-utilities) that are used in {{site.data.keyword.bpshort}} runtime.

## How can I fix Git token issues when creating workspaces by using the CLI 
{: #create-workspace-cli-tokens}
{: faq}
{: support}

Using `schematics workspace new --file schematic-file.json -g xxxx` command throws an `Access token creation failed status`, as the token is not specified in the command.

You need to check your [authentication](/docs/schematics?topic=schematics-setup-api#cs_api) before performing the operation through command-line. Then, create a workspace by using `schematics workspace new --file schematic-file.json --github-token xxxx` command. For more information, see [`ibmcloud schematics workspace new`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) command.

## How do I fix authorization issues when creating or updating a workspace?
{: #workspace-auth}
{: faq}
{: support}

You see authorization issues when the role and permission access is insufficient while updating the workspace. For more information, see [Managing user access](/docs/schematics?topic=schematics-access).

## How to use {{site.data.keyword.bpshort}} services with a test ID?
{: #global-catalog-faq}
{: faq}
{: support}

The test IDs are considered as a valid `IBM ID` to set the global catalog or resource controller-related API calls. If you are unable to access, do [Contact support service](/docs/schematics?topic=schematics-schematics-help).

## How to limit Git repo folder cloning 
{: #compact-faq}
{: faq}
{: support}

By default when creating a workspace through the UI, {{site.data.keyword.bpshort}} default to cloning the full Git repository and all sub directory. De-select the `Use full repository` flag to limit the folders that are cloned and improve download performance. 

{{site.data.keyword.bpshort}} introduced a `compact` flag in the [create workspace](/apidocs/schematics/schematics#create-workspace) and [update workspace](/apidocs/schematics/schematics#replace-workspace) API to download the `sub directories` in Git repositories. If the compact flag is set to **true** it downloads and save `sub directories` recursively, otherwise, you can continue to download and save the full repository on workspace creation.

You can get the response by starting `get workspace API` to view the compact flag value. The compact flag can be given only if the `template_repo.url` field is passed. On update, if this field is not passed, but the URL is passed, the download is compact.

Compact usage in the payload is `.template_data[0].compact = true/false`. For more information, see [Compact download for {{site.data.keyword.bpshort}} workspaces](/docs/schematics?topic=schematics-compact-download).

## How to delete a workspace when the delete fails?
{: #clusterdeletion-warn-faq}
{: faq}
{: support}

If a resource is deleted outside the {{site.data.keyword.bpshort}}, a workspace delete operation displays that as `resource no longer exists`.

You need to delete the workspace and NOT destroying the resources as if resource is not available. For more information, see [Deleting a workspace](/docs/schematics?topic=schematics-sch-delete-wks).

## What is the best way to deploy a Helm chart by using credentials or secrets?
{: #gherepo-warn-faq}
{: faq}
{: support}

The best way is to use {{site.data.keyword.cloud_notm}} catalog to manage the Helm charts where inside the catalog you can keep the credentials and mark it as secured. For more information, see [List of catalog that is related to Helm](https://cloud.ibm.com/catalog?search=label%3Ahelm).

## How to address job failures that are caused by maintenance activities? 
{: #impact-downtime-workspace}
{: faq}
{: support}

The unexpected impact due to maintenance results in the failure of the running activities in {{site.data.keyword.bpshort}} workspace. Such workspace and the ongoing activity are marked as `Failed`. The user can then re-run the activity. For more information, see [workspace state diagram](/docs/schematics?topic=schematics-wks-state#workspace-state-diagram).

## How do you set the Git release tag? 
{: #releasetag-warn-faq}
{: faq}
{: support}

```text
2021/11/08 12:34:06 -----  New Action  -----
 2021/11/08 12:34:06 Request: RepoURL=https://github.ibm.com/wh-hp-insights/hi-cloud-automation, workspaceSource=Schematics, Branch=2021.10, Release=, Folder=terraform-v2/workspace-hi-qa-automation-app
 2021/11/08 12:34:06 Related Activity: action=UPDATE_WORKSPACE,processedBy=sandbox-6bcf8bffcd-rxbww_2478
 2021/11/08 12:34:06 Getting download command
 2021/11/08 12:34:11 Fatal, could not download repo, Failed to clone git repository, couldn't find remote ref "refs/heads/2021.10" (most likely invalid branch name is passed)
 2021/11/08 12:34:12 Problems found with the Repository. Please Rectify and Retry
```
{: screen}

If the `Release` parameter is empty and the `Branch` was set with release tag.
{: note}

{{site.data.keyword.bpshort}} does not support `release` tag, as it's difficult to identify if it’s a release tag or a branch from the Git repository URL. You need to set the `release` tag through the [{{site.data.keyword.bpshort}} API](/apidocs/schematics/schematics#create-workspace).

## Why do I get a 403 error instead of a 404 error when using an invalid workspace ID?
{: #invalidwspid-warn-faq}
{: faq}
{: support}

```sh
curl -X GET https://schematics.cloud.ibm.com/v1/workspaces/badWOrkspaceId -H "Authorization: $IAM_TOKEN"
{"requestid":"3a3cbffe-e23a-4ccf-b764-042f7379c084","timestamp":"2021-11-11T17:00:07.169953698Z","messageid":"M1078","message":"Error while validating the location in the account. Verify you have permission to the location in the global catalog settings.","statuscode":403}
```
{: pre}

Yes there is a change in the API that checks for the location first and if it doesn’t get the proper location for the workspace it returns a 403 error instead of 404 error.

## How can I enable Terraform debug logging 
{: #terraform-debug-ibmcli}
{: faq}
{: support}

You can set the environment variable for setting the Terraform log debug `TF_LOG=debug` trace in the payload, as shown in the sample payload. For more information, see [{{site.data.keyword.bpshort}} workspaces update](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update).

```json
{
  "name": "sample",
  "type": [
    "terraform_v1.4"
  ],
  "description": "terraform workspace",
  "tags": [
  ],
  "template_repo": {
    "url": "<your repo>"
  },
  "template_data": [
    {
      "folder": ".",
      "type": "terraform_v1.4",
      "env_values":[
      {
        "TF_LOG":"debug"
      }
    ]
    }
  ]
}
```
{: codeblock}

## How can I import Cloud resources into a workspace?
{: #workspace-import-ibmcli}
{: faq}
{: support}

Use the `ibmcloud schematics workspace import --options value, -o value : Optional` command and the sample syntax to import from command line. For more information, see [{{site.data.keyword.bpshort}} workspace import](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-import).

``` sh

ibmcloud schematics workspaces import --id <workspace_id> --address <my terraform resource address> --resourceID <the CRN of the item to import> --options "-var IC_API_KEY=XXXXXXXX"

or 

ibmcloud schematics workspaces import --id <workspace_id> --address <my terraform resource address> --resourceID <the CRN of the item to import> --options "--var-file=<path-to-var-file>"
```
{: pre}

## How can I download Job files?
{: #download-jobfile}
{: faq}
{: support}

Yes, you can download the {{site.data.keyword.bpshort}} Job files. For more information, see [Download {{site.data.keyword.bpshort}} Job files](/docs/schematics?topic=schematics-job-download).

##	How can I resolve Terraform resource timeout failures? 
{: #logdnainstance-faq}
{: faq}
{: support}

 You need to update or increase the timeout value by 5 minutes or 10 minutes depending upon the service as shown in the Terraform block. Or you need to send `null value` to use the default values.

 ```terraform
 variable "create_timeout" 
 {
  type = String
  description = "Timeout duration to create LogDNA instance in Schematics."
  default = "15m"
 }
 ```
 {: codeblock}

## How do I set the TF_CLI_ARGS environment variable?
{: #terraformcli-arguments-faq}
{: faq}
{: support}

 No, you cannot set an environment variable value in the {{site.data.keyword.bpshort}} workspaces console directly. Instead, you can use a CURL by using the [{{site.data.keyword.bpshort}} API](/apidocs/schematics/schematics#create-workspace), or [{{site.data.keyword.bpshort}} command line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new).

 ```json
   "env_values": [
         {
           "TF_LOG": "debug"
         },
   ]    
 ```
 {: codeblock}

## Can I use private Git repositories for modules?
{: #download-module-netrc-faq}
{: faq}
{: support}

Yes, {{site.data.keyword.bpshort}} supports downloading Terraform modules from private repositories. For more information, see [Supporting to download modules from private remote host](/docs/schematics?topic=schematics-download-modules-pvt-git).

## Can I edit all the variables in a workspace?
{: #edit-variables-faq}
{: faq}
{: support}

You can edit only one variable at a time from {{site.data.keyword.bpshort}} console. From the command line you can edit all the variables of the workspace in the JSON format by using [`ibmcloud schematics workspace update`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update) command.

## How do I import keys when importing KMS resources?
{: #kmskey-value-faq}
{: faq}
{: support}

Yes, you can set or manage the keys by using `ibm_kms_key` as shown in the sample code block. For more information, see [ibm_kms_key](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/kms_key#import){: external}.

```terraform
resource "ibm_resource_instance" "kms_instance" {
  name     = "instance-name"
  service  = "kms"
  plan     = "tiered-pricing"
  location = "us-south"
}
resource "ibm_kms_key" "test" {
  instance_id  = ibm_resource_instance.kms_instance.guid
  key_name     = "key-name"
  standard_key = false
  force_delete =true
}
resource "ibm_cos_bucket" "smart-us-south" {
  bucket_name          = "atest-bucket"
  resource_instance_id = "cos-instance-id"
  region_location      = "us-south"
  storage_class        = "smart"
  key_protect          = ibm_kms_key.test.id
}
```

## Can you enable the TRACE to help DEBUG {{site.data.keyword.bpshort}} API while running workspace list command?
{: #traces-api-faq}
{: faq}
{: support}

No, currently {{site.data.keyword.bpshort}} do not support this feature while running `IBMCLOUD_TRACE=true ibmcloud schematics workspace list` command. 

## How do I resolve errors in listing workspaces?
{: #badstatus-workspace-faq}
{: faq}
{: support}

When listing or retrieving workspaces the following error may be received.  `Error while retrieving Schematics Instance for the given account`.


```text
Error:
Bad status code [400] returned when getting workspace from Schematics: {"requestid":"fe5f0d6d-1d43-4643-a689-35d090463ce8","timestamp":"2022-01-25T20:23:54.727208017Z","messageid":"M1070","message":"Error while retrieving Schematics Instance for the given account.","statuscode":400}

```
{: codeblock}

You might have insufficient access for the workspaces in the specified location to fetch the instance. Do check the permission that is provided for your account and the locations where your instance need to be created. For more information, see [Where is an information stored?](/docs/schematics?topic=schematics-secure-data#pi-location)

## How can I use (IBM) GitLab repositories?
{: #gitlab-workspace-faq}
{: faq}
{: support}

Yes, You can access the private (IBM) GitLab repository by using {{site.data.keyword.bpshort}} with the privileges.

- If the private (IBM) GitLab repository `git.cloud.ibm.com` access token is not needed as the IAM token is used.

- If the public GitLab `gitlab.com`, `read_repository`, and `read_api` access are needed to validate the branch name for private repository.

You can use the sample Terraform code block to configure the GitLab repository details.

```terraform
"template_repo": {
"url": "<gitlab_source_repo_url>",
"branch": ""
},
```
{: codeblock}

## Can IAM access groups be managed in {{site.data.keyword.bpshort}}?
{: #manageaccessgrp-iam-faq}
{: faq}
{: support}

Yes, {{site.data.keyword.bpshort}} supports the full {{site.data.keyword.cloud_notm}} provider resource set. For more information about How IAM access group works? see [ibm_iam_access_group](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_access_group).

## How do I work with resources in another account? 
{: #account-resource-faq}
{: faq}
{: support}

Yes, you can create {{site.data.keyword.bpshort}} workspaces in {{site.data.keyword.cloud_notm}} source account. Then, run Terraform providing resources in target account to provision, through CLI, and API calls by using the target account service ID with the authentication, appropriate cross account authorization, or API key. For more information, see [Managing resources in other account](/docs/schematics?topic=schematics-create-tf-config#manage-resource-account).

## What does `North America` location indicate?  
{: #location-faq}
{: faq}
{: support}

North America always indicates both `us-south`, and `us-east` location during the {{site.data.keyword.bpshort}} workspace creation. For more information, see [Where can I create {{site.data.keyword.bpshort}} workspaces?](/docs/schematics?topic=schematics-locations#where-wks-created), and [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location)


## What ports and IP addresses are used by {{site.data.keyword.bpshort}}?
{: #port-faq}
{: faq}
{: support}

{{site.data.keyword.bpshort}} communicates with the ports that are specified by the related resources. For example, VPC related ports, see [VPC: Opening required ports and IP addresses in other network firewalls](/docs/containers?topic=containers-vpc-firewall). 

## When do I use {{site.data.keyword.bpshort}} versus the individual resource dashboards?
{: #schematics-vs-cloud-resource-faq}
{: faq}
{: support}

With {{site.data.keyword.bplong_notm}}, you can run your infrastructure code in {{site.data.keyword.cloud_notm}} to manage the lifecycle of {{site.data.keyword.cloud_notm}} resources. After you provision a resource, you use the dashboard of the individual resource to work and interact with your resource. For example, if you provision a virtual server instance in a Virtual Private Cloud (VPC) with {{site.data.keyword.bplong_notm}}. You can use the VPC console, API, or command-line to `stop`, `reboot`, and `power on` your virtual server instance. However, to remove the virtual server instance, you can use {{site.data.keyword.bplong_notm}}.

## Are changes to Git repos refreshed in {{site.data.keyword.bpshort}}?
{: #edit-resource-confg-faq}
{: faq}
{: support}

No, if you change the code of your Terraform template in GitHub, these changes are not available automatically when you create an execution plan in {{site.data.keyword.bplong_notm}}. To pull the current changes from your GitHub repository, make sure that you click the `Pull latest` option from the workspace `settings` page before you create your execution plan.

## Where is the Terraform state file stored?
{: #resource-state-faq}
{: faq}
{: support}

After you successfully provisioned {{site.data.keyword.cloud_notm}} resources by running a {{site.data.keyword.bpshort}} apply action, the state of resources is stored in a Terraform state file (`terraform.tfstate`). {{site.data.keyword.bpshort}} uses this state file as the single source of truth to determine what resources exist in your account. The state file maps the resources that you specified in your Terraform configuration file to the {{site.data.keyword.cloud_notm}} resource that you provisioned.

## Are resources removed on delete workspace?
{: #delete-resource-wks-faq}
{: faq}
{: support}

Deleting a workspace from {{site.data.keyword.bplong_notm}} does not remove any of your {{site.data.keyword.cloud_notm}} resources. If you delete the workspace before you remove your resources, you must manually remove all your {{site.data.keyword.cloud_notm}} resources from the individual resource dashboard.

Removing {{site.data.keyword.cloud_notm}} resources cannot be undone. Make sure that you have backed up any data before you remove a resource. Resources are removed (deleted) if you remove the resource definition or comment out the resource in your Terraform configuration file. Review the Plan log file to verify that all your resources are included in the removal.
{: important}

## Can I set environment variables for workspaces?
{: #workspace-env-vars}
{: faq}
{: support}

You can set `env values` for a workspace by using the CLI and API. For more information, see [usage of `env_values`](/docs/schematics?topic=schematics-set-parallelism).

Sample payload

```json
{
  "name": "newName",
  "template_data": [
    {
      "type": "<same_as_before>",
      "env_values": [
        {
          "env_key1": "dummy_text"
        },
        {
          "env_key2": "dummy_text"
        }
      ],
      "env_values_metadata": [
        {
          "name": "env_values_1",
          "hidden": false,
          "secure": false
        },
        {
          "name": "env_values_2",
          "hidden": false,
          "secure": false
        }
      ]
    }
  ]
}
```
{: codeblock}

## Does drift detection run automatically?
{: #drift-automatic-faq}
{: faq}
{: support}

No, the drift detection is not an automatic method of detection in the {{site.data.keyword.bplong_notm}}. For more information, see [detecting drift in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-drift-note&interface=api#drift-in-ibm).

## How can I initiate drift detection?
{: #drift-initiate-faq}
{: faq}
{: support}

You can initiate drift detection by using the UI and CLI. For more information, see [detecting drift in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-drift-note).

## Where can I see the status of a drift detection job? 
{: #drift-status-faq}
{: faq}
{: support}

To verify the results of a drift detection job, you need to check the drift detection job log. The job log provides the details of the drift detection as `in progress` or `completed` with the appropriate status such as `failure` or `success`. For more information, see [detecting drift in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-drift-note&interface=api#drift-in-ibm).

## Can I `interrupt` or `terminate` running jobs?
{: #stopping-job-faq}
{: faq}
{: support}

Yes, you can interrupt, force-stop, or terminate the provisioning resources or a running job in {{site.data.keyword.bpshort}} by using the job types. For more information, see [stopping the job types](/docs/schematics?topic=schematics-interrupt-job#interrupt-types).

## How do I correct an `Incorrect Location Input` error?
{: #postcartapi-job-faq}
{: faq}
{: support}

Error

```text
{
    "requestid": "3f59c342-cd2c-4703-aa10-9e8e7072a3ac",
    "timestamp": "2022-06-28T20:02:58.529765308Z",
    "messageid": "M1097",
    "message": "Incorrect Location Input.",
    "statuscode": 400
}
```
{: screen}

The {{site.data.keyword.bpshort}} global endpoint is defaulted to `us` environment. Therefore, you need to use [regional endpoints](/apidocs/schematics/schematics#api-endpoints) to point your location to a `eu-de` region. 

## How can I view workspace resources?
{: #clicmdresource-job-faq}
{: faq}
{: support}

Use the [`state list`](/docs/schematics?topic=schematics-schematics-cli-reference#state-list) CLI command to view the same resources as in {{site.data.keyword.bplong_notm}} UI.

## How do I fix the `CreateworkspaceWithContext failed Bad request` error?
{: #locationres-job-faq}
{: faq}
{: support}

Error

```text
CreateWorkspaceWithContext failed Bad request. Check that the information you entered in the payload is complete and formatted correctly in JSON.
```
{: screen}

The {{site.data.keyword.bpshort}} public or private endpoint global URL by default points to `us` region. As a workaround you can set the [environment variable key](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-reference#config-provider) before the Terraform commands.

    ```sh
    export IBMCLOUD_SCHEMATICS_API_ENDPOINT="https://eu-de.schematics.cloud.ibm.com"
    ```

You can also add the endpoints to a JSON file to categorize the endpoints service as public or private.

Sample provider declaration

```terraform
{
    "IBMCLOUD_SCHEMATICS_API_ENDPOINT":{
        "public":{
            "eu-de":"https://eu-de.schematics.cloud.ibm.com"
        }
         }
}
```
{: codeblock}

Example Provider Block

```terraform
provider "ibm" {

  endpoints_file_path= "endpoints.json"
}
```
{: codeblock}

## Are sensitive values in the state file encrypted? 
{: #encrypt-state-file}
{: faq}
{: support}

{{site.data.keyword.bpshort}} encrypts the Terraform state file when stored and also in transit by using TLS. Terraform does not separately encrypt sensitive values. For more information, see [sensitive-data](https://developer.hashicorp.com/terraform/language/state/sensitive-data) in state file.

## Why do workspace variables that are defined by using CLI throw 400 errors?
{: #wks-list-var}
{: faq}
{: support}

The {{site.data.keyword.bpshort}} workspace list variables store value should always be an HCL string. The `value` field must contain an escaped string for the variable store for the list, map, or complex variable. For more information, see [Providing values to {{site.data.keyword.bpshort}} for the declared variables](/docs/schematics?topic=schematics-create-tf-config#declare-variable).

## Can you update the Terraform version (`TF_VERSION`) using a `JSON` file? 
{: #tf-version-update}
{: faq}
{: support}

Currently, the workaround to updating the `TF_VERSION` is to pass the `TF_VERSION` while updating the variable store. {{site.data.keyword.bpshort}} auto detects what is specified in the Terraform version block in the `TF` files. This is the default behavior.

For more information, see [setting and changing the version](/docs/schematics?topic=schematics-migrating-terraform-version#terraform-version-upgrade1x).

## Can start with a new Terraform state file on each job run?
{: #wks-job-trigger}
{: faq}
{: support}

No, you need to create new workspace. For more information, see [Workspace job execution](/docs/schematics?topic=schematics-job-download#wks-job-execution).

## Can I import an existing Terraform state file?
{: #tf-state-argument}
{: faq}
{: support}

Yes, you can use `--state` flag option through the [ibmcloud schematics workspace new](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new).

## What is the maximum variable length of characters?
{: #wks-name-maxlength}
{: faq}
{: support}

The maximum length of characters that {{site.data.keyword.bpshort}} workspace variables support is 1 MB.

## What is the maximum state file to import? 
{: #wks-statefile-limit}
{: faq}
{: support}

The `terraform.tfstate` file must be less than 16 MB. When you create workspace from an existing Terraform state file, the `terraform.tfstate` file must be less than 16 MB. Greater than 16 MB state file is not supported in the {{site.data.keyword.bpshort}}. You see an error message with `413 Request Entity Too Large error when creating a new workspace`.

## How do I fix authentication errors when using the API?
{: #createworkspace-authentication-error}
{: faq}
{: support}

You need to create the IAM access token for your {{site.data.keyword.cloud_notm}} Account. For more information, see [Get token password](/apidocs/iam-identity-token-api#gettoken-password){: external}. You can see the following sample error message and the solution for the authentication error.

```text
Error: Request fails with status code: 400, BXNIMO137E: For the original authentication, client id 'default' was passed, refresh the token, client id 'bx' is used.
```
{: screen}

The [IAM API](/apidocs/iam-identity-token-api#gettoken-apikey){: external} documentation shows how to create a `default token`. You can use the `refresh token` to get a new IAM access token if that token is expired. When the default client (no basic authorization header) as described in this documentation. The `refresh_token` cannot be used to retrieve a new IAM access token. When the IAM access token is about to be expired, use the API key to create a new access token as listed.

1. You need to create `access_token` and `refresh_token`.

    ```sh
    export IBMCLOUD_API_KEY=<ibmcloud-api_key>
    curl -X POST "https://iam.cloud.ibm.com/identity/token" -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=$IBMCLOUD_API_KEY" -u bx:bx
    ```
    {: pre}

2. Export the `access_token` and `refresh_token` obtained in step 1 as environment variables for `ACCESS_TOKEN` and `REFRESH_TOKEN`.

    ```sh
    export ACCESS_TOKEN=<access_token>
    export REFRESH_TOKEN=<refresh_token>
    ```
    {: pre}

3. Create workspace

    ```sh
    curl --request POST --url https://cloud.ibm.com/schematics/overview/v1/workspaces -H "Authorization: Bearer <access_token>" -d '{"name":"","type": ["terraform_v1.4"],"description": "","resource_group": "","tags": [],"template_repo": {"url": ""},"template_data": [{"folder": ".","type": "terraform_v1.4","variablestore": [{"name": "variable_name1","value": "variable_value1"},{"name": "variable_name2","value": "variable_value2"}]}]}'
    ```
    {: pre}



## How to retrieve the {{site.data.keyword.bpshort}} Workspace ID as environment variable? 
{: #retrieve-wks-id-env-var-faq}
{: faq}
{: support}

You can retrieve the {{site.data.keyword.bpshort}} Workspace ID as environment variable by using the following code. Before running plan or apply, `IC_SCHEMATICS_WORKSPACE_ID`, `TF_VAR_IC_SCHEMATICS_WORKSPACE_ID`, `TF_VAR_IC_SCHEMATICS_WORKSPACE_RG_I`, `IC_IAM_TOKEN`, and `IC_IAM_REFRESH_TOKEN` environment variables are automatically set to your Terraform scripts.

```json
data "external" "env" {
  program = ["jq", "-n", "env"]
}

output "workspace_id" {
value = "${lookup(data.external.env.result, "TF_VAR_IC_SCHEMATICS_WORKSPACE_ID")}"
```
{: codeblock}

If you want to see all the available environment variables in the workspace use `output "${jsonencode(data.external.env.result)}"` code.
{: note}

## How do I rectify 401 errors from the broker call when deleting the {{site.data.keyword.bpshort}} objects?
{: #sch-obj-delete-faq}
{: faq}
{: support}

After the {{site.data.keyword.bpshort}} objects deletion, if the {{site.data.keyword.bpshort}} services fails to delete the objects in your account. You need to raise a [{{site.data.keyword.bpshort}} support ticket](/docs/schematics?topic=schematics-schematics-help) to remove from the resource controller.
