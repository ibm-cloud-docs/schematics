---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-15"

keywords: schematics faqs, infrastructure as code, iac, schematics workspaces faq, workspaces faq

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# Workspaces
{: #workspaces-faq}

Answers to common questions about the {{site.data.keyword.bplong_notm}} Workspaces are classified into following section.
{: shortdesc}

## How do you overcome the authentication error when {{site.data.keyword.bpshort}} Workspaces is created by using API?
{: #createworkspace-authentication-error}
{: faq}
{: support}

You need to create the IAM access token for your {{site.data.keyword.cloud_notm}} Account. For more information, see [Get token password](/apidocs/iam-identity-token-api#gettoken-password){: external}. You can see the following sample error message and the solution for the authentication error.
```text
Error: Request fails with status code: 400, BXNIMO137E: For the original authentication, client id 'default' was passed, refresh the token, client id 'bx' is used.
```

The [IAM API](/apidocs/iam-identity-token-api#gettoken-apikey){: external} documentation shows how to create a `default token`. You can use the `refresh token` to get a new IAM access token if that token is expired. When the default client (no basic authorization header) as described in this documentation. The `refresh_token` cannot be used to retrieve a new IAM access token. When the IAM access token is about to be expired, use the API key to create a new access token as listed.

1. You need to create `access_token` and `refresh_token`.

    ```sh
    export IBMCLOUD_API_KEY=<ibmcloud-api_key>
    curl -X POST "https://iam.cloud.ibm.com/identity/token" -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=$IBMCLOUD_API_KEY" -u bx:bx
    ```
2. Export the `access_token` and `refresh_token` obtained in step 1 as environment variables for `ACCESS_TOKEN` and `REFRESH_TOKEN`.

    ```sh
    export ACCESS_TOKEN=<access_token>
    export REFRESH_TOKEN=<refresh_token>
    ```
3. Create workspace

    ```sh
    curl --request POST --url https://cloud.ibm.com/schematics/overview/v1/workspaces -H "Authorization: Bearer <access_token>" -d '{"name":"","type": ["terraform_v0.12"],"description": "","resource_group": "","tags": [],"template_repo": {"url": ""},"template_data": [{"folder": ".","type": "terraform_v0.12","variablestore": [{"name": "variable_name1","value": "variable_value1"},{"name": "variable_name2","value": "variable_value2"}]}]}'
    ```

## How do {{site.data.keyword.bpshort}} decide to remove the files from the Terraform, or Ansible templates?
{: #clone-file-extension}
{: faq}
{: support}

Creating {{site.data.keyword.bpshort}} Workspaces or action {{site.data.keyword.bplong_notm}} takes a copy of the Terraform, or Ansible template from your Git repository and stores in a secured location. Before the template files are saved, {{site.data.keyword.bpshort}} analyses the files and are removed, based on the following conditions:

- The allowed file extensions are `.cer, .cfg, .conf, .crt, .der, .gitignore, .html, .j2, .jacl, .js, .json, .key, .md, .netrc, .pem, .properties, .ps1, .pub, .py, .service, .sh, .tf, .tf.json, .tfvars, .tmpl, .tpl, .txt, .yaml, .yml, .zip, _rsa, license`.
- The allowed image extensions are `.bmp, .gif, .jpeg, .jpg, .png, .so .tif, .tiff`.
- The files that are removed are `.asa, .asax, .exe, .php5, .pht, .phtml, .shtml, .swf, .tfstate, .tfstate.backup, .xap`.
- All files that are larger than 500 KB are removed. This file size limit does not apply for the allowed image files.
- The folder name starts with a (period) `.` is treated as vulnerable. 

The allowed extension list is continuously monitored and updated in every release. You can raise a [support ticket](/docs/schematics?topic=schematics-schematics-help) with the justification to add a file extension to the list.
{: note}

## How do you upgrade the Terraform versions in {{site.data.keyword.bpshort}}? or Can I update the version during workspace recreation?
{: #migrate-terraform-v11}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version).
{: deprecated}

You can follow the topics to upgrade from one Terraform version to another version

- [Upgrading the Terraform template version](/docs/schematics?topic=schematics-migrating-terraform-version#terraform-version-upgrade0x)
- [Upgrade Terraform version in {{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-migrating-terraform-version#migrate-steps12)
- [Upgrade Terraform template from v0.12 to v0.13](/docs/schematics?topic=schematics-migrating-terraform-version#migrate-steps12)
- [Upgrade Terraform template from v0.13 to v0.14](/docs/schematics?topic=schematics-migrating-terraform-version#upgrade-13-to10)
- [Upgrade Terraform template from v0.14 to v1.0](/docs/schematics?topic=schematics-migrating-terraform-version#upgrade-13-to10)

## How do I overcome the downtime updating the workspace activities? 
{: #impact-downtime-workspace}
{: faq}
{: support}

The unexpected impact due to maintenance results in the failure of the running activities in {{site.data.keyword.bpshort}} Workspace. Such workspace and the ongoing activity are marked as `Failed`. The user can then re-execute the activity. For more information, see [workspace state diagram](/docs/schematics?topic=schematics-workspace-setup#workspace-state-diagram).

## Why do the jobs delay in a queue when plan is generated?
{: #job-queue-faq}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} queues all the users jobs into a single queue. Depending on the workload generated by the users and the time to run the jobs, the user might experience delays. For more information, see [Job queue status](/docs/schematics?topic=schematics-job-queue-process).

## How do I `pull latest` code from the workspace through command line? 
{: #latestcode-workspace-commandline}
{: faq}
{: support}

Updating the {{site.data.keyword.bpshort}} Workspaces through command line need the needed field `name`.

You need to run `ibmcloud schematics workspace update --id <workspace-id>  --file <updatefile.json>`  command. The sample `updatefile.json` contains the name field with the value.
```json
{
    "name":"testworkspace"
}
```

## What are the development tools and utilities used in the {{site.data.keyword.bpshort}}?
{: #schematics-tools}
{: faq}
{: support}

{{site.data.keyword.bpshort}} runtime is built by using Universal Base Image (UBI-8) and the runtime utilities/softwares that come with the UBI-8 are available for Terraform provisions and Ansible actions. For more information, see the list of [tools and utilities](/docs/schematics?topic=schematics-sch-utilities) used in {{site.data.keyword.bpshort}} runtime.

## How can I create workspace from command-line by using Git repositories and personal access token with full permission?
{: #create-workspace-cli-tokens}
{: faq}
{: support}

Using `schematics workspace new --file schematic-file.json -g xxxx` command throws an `Access token creation failed status`, as the token name not specified in the command.

You need to check your [authentication](/docs/schematics?topic=schematics-setup-api#cs_api) before setting the post operation through command-line. Then, create a workspace by using `schematics workspace new --file schematic-file.json --github-token xxxx` command. For more information, see [`ibmcloud schematics workspace new`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) command.

## How do you overcome the authorization issue when create or update a workspace or a template?
{: #workspace-auth}
{: faq}
{: support}

You see authorization issues when the roles and permission access is insufficient while updating workspace. For more information, see [Managing user access](/docs/schematics?topic=schematics-access).

## How can I access the {{site.data.keyword.bpshort}} services for test ID?
{: #global-catalog-faq}
{: faq}
{: support}

The test IDs are considered as a valid `IBM ID` to set the global catalog or resource controller-related API calls. If you are unable to access, do [Contact support service](/docs/schematics?topic=schematics-schematics-help).

## How can you download `subfolder`s from the Git repositories through {{site.data.keyword.bpshort}}
{: #compact-faq}
{: faq}
{: support}

{{site.data.keyword.bpshort}} introduced a `compact` flag in the [create workspace](/apidocs/schematics/schematics#create-workspace) and [update workspace](/apidocs/schematics/schematics#replace-workspace) API to download the `subfolder` from the Git repositories. If the compact flag is set to **true** you can download and save `subfolder` recursively, otherwise, you can continue to download and save the full repository on workspace creation.

You can get the response by starting `get workspace API` to view the compact flag value. The compact flag can be given only if the `template_repo.url` field is passed. On update, if this field is not passed, but URL is passed, the download is compact.

Compact usage in the payload is `.template_data[0].compact = true/false`. For more information, see [Compact download for {{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-compact-download).

## How do I resolve issue while trying to delete a workspace that was created for a cluster that no longer exists, deletion fails because of the cluster not found?
{: #clusterdeletion-warn-faq}
{: faq}
{: support}

You need to delete the workspace and NOT destroying the resources as if resource is not available. For more information, see [Deleting a workspace](/docs/schematics?topic=schematics-workspace-setup#del-workspace).

## What is the best way to deploy a Helm chart to an existing cluster by using {{site.data.keyword.bpshort}} keeping credentials or secrets?
{: #gherepo-warn-faq}
{: faq}
{: support}

The best way is to use {{site.data.keyword.cloud_notm}} catalog to manage the Helm charts where inside the catalog you can keep the credentials and mark it as secured. For more information, see [List of catalog that is related to Helm](https://cloud.ibm.com/catalog?search=label%3Ahelm).

## How do you set the release tag through {{site.data.keyword.bpshort}}? 
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

## Why you are getting 403 error instead of 404 error when providing an invalid workspace ID?
{: #invalidwspid-warn-faq}
{: faq}
{: support}

```sh
curl -X GET https://schematics.cloud.ibm.com/v1/workspaces/badWOrkspaceId -H "Authorization: $IAM_TOKEN"
{"requestid":"3a3cbffe-e23a-4ccf-b764-042f7379c084","timestamp":"2021-11-11T17:00:07.169953698Z","messageid":"M1078","message":"Error while validating the location in the account. Verify you have permission to the location in the global catalog settings.","statuscode":403}
```
{: pre}

Yes there is a change in the API that checks for the location first and if it doesn’t get proper location for the workspace it returns 403 error instead of 404 error.

## How can you enable Terraform debug through the `ibmcloud schematics` command line?
{: #terraform-debug-ibmcli}
{: faq}
{: support}

You can set the environment variable for setting the Terraform log debug `TF_LOG=debug` trace in the payload, as shown in the sample payload. For more information, see [{{site.data.keyword.bpshort}} Workspaces update](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update).

```json
{
  "name": "sample",
  "type": [
    "terraform_v0.12"
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
      "type": "terraform_v0.12",
      "env_values":[
      {
"TF_LOG":"debug"
      }
   ]
    }
  ]
}
```

## How can I generate {{site.data.keyword.bpshort}} Workspaces import from CLI?
{: #workspace-import-ibmcli}
{: faq}
{: support}

Use `ibmcloud schematics workspace import --options value, -o value : Optional` command and the sample syntax to import from command line. For more information, see [{{site.data.keyword.bpshort}} Workspaces import](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-import).

``` sh

ibmcloud {{site.data.keyword.bpshort}} Workspaces import --id <workspace_id> --address <my terraform resource address> --resourceID <the CRN of the item to import> --options "-var IC_API_KEY=XXXXXXXX"

or 

ibmcloud {{site.data.keyword.bpshort}} Workspaces import --id <workspace_id> --address <my terraform resource address> --resourceID <the CRN of the item to import> --options "--var-file=<path-to-var-file>"
```
{: pre}

## Can I download the {{site.data.keyword.bpshort}} Job files?
{: #download-jobfile}
{: faq}
{: support}

Yes, you can download the {{site.data.keyword.bpshort}} Job files. For more information, see [Download {{site.data.keyword.bpshort}} Job files](/docs/schematics?topic=schematics-job-download).

##	How can I resolve the error when using {{site.data.keyword.bpshort}} to create a LogDNA instance? 
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

## Can I set TF_CLI_ARGS environment variable in the {{site.data.keyword.bpshort}} Workspaces console without using catalog service or {{site.data.keyword.bpshort}} command line?
{: #terraformcli-arguments-faq}
{: faq}
{: support}

 No, you cannot set an environment variable value in the {{site.data.keyword.bpshort}} Workspaces console directly. Instead, you can use a CURL by using the [{{site.data.keyword.bpshort}} API](/apidocs/schematics/schematics#create-workspace), or [{{site.data.keyword.bpshort}} command line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new).

 ```json
   "env_values": [
         {
           "TF_LOG": "debug"
         },
   ]    
 ```
 {: codeblock}

## Does {{site.data.keyword.bpshort}} support to download the Terraform modules template from the private repository?
{: #download-module-netrc-faq}
{: faq}
{: support}

Yes, {{site.data.keyword.bpshort}} supports to download the Terraform modules template from the private repository. For more information, see [Supporting to download modules from private remote host](/docs/schematics?topic=schematics-download-modules-pvt-git).

## Can I edit all the variables in the {{site.data.keyword.bpshort}} console instead of editing individually?
{: #edit-variables-faq}
{: faq}
{: support}

You can edit one variable at a time from {{site.data.keyword.bpshort}} console. From the command line you can edit all the variables of the workspace in the JSON format by using [`ibmcloud schematics workspace update`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update) command.

## Can I set or manage keys for `ibm_kms_key` resource when {{site.data.keyword.bpshort}} Workspaces imports Terraform?
{: #kmskey-value-faq}
{: faq}
{: support}

Yes, you can set or manage the keys by using `ibm_kms_key` as shown in the sample code block. For more information, see [ibm_kms_key]( https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/kms_key#import). 

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

## How do I overcome the `Error while retrieving {{site.data.keyword.bpshort}} Instance for the given account` to fetch {{site.data.keyword.bpshort}} Workspaces?
{: #badstatus-workspace-faq}
{: faq}
{: support}

```text
Error:
Bad status code [400] returned when getting workspace from Schematics: {"requestid":"fe5f0d6d-1d43-4643-a689-35d090463ce8","timestamp":"2022-01-25T20:23:54.727208017Z","messageid":"M1070","message":"Error while retrieving Schematics Instance for the given account.","statuscode":400}
```

You might have insufficient access for the workspaces in specified location to fetch the instance. Do check the permission that is provided for your account and the locations where your instance need to be created. For more information, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location)

## How can I configure private (IBM) GitLab repository in {{site.data.keyword.bpshort}} Workspace?
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

## Does {{site.data.keyword.cloud_notm}} provider support manages IAM access groups in {{site.data.keyword.bpshort}}?
{: #manageaccessgrp-iam-faq}
{: faq}
{: support}

Yes, {{site.data.keyword.bpshort}} supports the full {{site.data.keyword.cloud_notm}} provider resource set. For more information about How IAM access group works? see [ibm_iam_access_group](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_access_group).

## Might I create {{site.data.keyword.bpshort}} Workspaces in {{site.data.keyword.cloud_notm}} source account and run Terraform providing resources in {{site.data.keyword.cloud_notm}} target account to provision?
{: #account-resource-faq}
{: faq}
{: support}

Yes, you can create {{site.data.keyword.bpshort}} Workspaces in {{site.data.keyword.cloud_notm}} source account. Then, run Terraform providing resources in target account to provision, through CLI, and API calls by using the target account service ID with the authentication, appropriate cross account authorization, or API key. For more information, see [Managing resources in other account](/docs/schematics?topic=schematics-create-tf-config#manage-resource-account).

## Does `North America` location indicate `us-south`, `us-east`, or `both` during the {{site.data.keyword.bpshort}} Workspaces creation?
{: #location-faq}
{: faq}
{: support}

North America always indicates both `us-south`, and `us-east` location during the {{site.data.keyword.bpshort}} Workspaces creation. For more information, see [Where can I create {{site.data.keyword.bpshort}} Workspaces?](/docs/schematics?topic=schematics-locations#where-wks-created), and [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location)


## What are the port used to communicate with {{site.data.keyword.bpshort}} and resources, such as VPC services?
{: #port-faq}
{: faq}
{: support}

{{site.data.keyword.bpshort}} communicates with the ports that are specified by the related resources. For example, VPC related ports, see [VPC: Opening required ports and IP addresses in other network firewalls](/docs/containers?topic=containers-vpc-firewall). 

## When do I use {{site.data.keyword.bplong_notm}} versus the individual resource dashboards?
{: #schematics-vs-cloud-resource-faq}
{: faq}
{: support}

With {{site.data.keyword.bplong_notm}}, you can run your infrastructure code in {{site.data.keyword.cloud_notm}} to manage the lifecycle of {{site.data.keyword.cloud_notm}} resources. After you provision a resource, you use the dashboard of the individual resource to work and interact with your resource. For example, if you provision a virtual server instance in a Virtual Private Cloud (VPC) with {{site.data.keyword.bplong_notm}}. You can use the VPC console, API, or command-line to stop, reboot, and power on your virtual server instance. However, to remove the virtual server instance, you can use {{site.data.keyword.bplong_notm}}.

## When I change my configuration file in GitHub, is my change automatically available in the next execution plan?
{: #edit-resource-confg-faq}
{: faq}
{: support}

If you change the code of your Terraform template in GitHub, these changes are not available automatically when you create an execution plan in {{site.data.keyword.bplong_notm}}. To pull the current changes from your GitHub repository, make sure that you click the `Pull latest` option from the workspace `settings` page before you create your execution plan.

## Where does {{site.data.keyword.bpshort}} store the state of the cloud resources?
{: #resource-state-faq}
{: faq}
{: support}

After you successfully provisioned {{site.data.keyword.cloud_notm}} resources by running a {{site.data.keyword.bpshort}} apply action, the state of resources is stored in a Terraform state file (`terraform.tfstate`). {{site.data.keyword.bpshort}} uses this state file as the single source of truth to determine what resources exist in your account. The state file maps the resources that you specified in your Terraform configuration file to the {{site.data.keyword.cloud_notm}} resource that you provisioned.

## Are the resources removed when remove the workspace is run?
{: #delete-resource-wks-faq}
{: faq}
{: support}

Removing the workspace from {{site.data.keyword.bplong_notm}} does not remove any of your {{site.data.keyword.cloud_notm}} resources. If you remove the workspace before you removed your resources, you must manually remove all your {{site.data.keyword.cloud_notm}} resources from the individual resource dashboard.

Removing an {{site.data.keyword.cloud_notm}} resource cannot be undone. Make sure that you backed up your data before you remove a resource. If you choose to remove the infrastructure code, or comment out the resource in your Terraform configuration file. Make sure to review the log file of your execution plan to verify that all your resources are included in the removal.
{: important}

## How can I update a workspace that is created through payload in command line to resolve invalid payload issue?
{: #invalid-paylaod-cli}
{: faq}
{: support}

You can use `env values` as shown in the sample payload to update the workspace in the CLI. For more information, see [usage of `env_values`](/docs/schematics?topic=schematics-set-parallelism#parelleism-usage).

**Sample payload:**

```json
{
  "name": "newName",
  "template_data": [
    {
      "type": "<same_as_before>",
      "env_values": [
        {
          "env_values_1": "dummy_text"
        },
        {
          "env_values_2": "dummy_text"
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
{: pre}

## Is the drift detection an automatic in the {{site.data.keyword.bplong_notm}}?
{: #drift-automatic-faq}
{: faq}
{: support}

No, the drift detection is not an automatic method of detection in the {{site.data.keyword.bplong_notm}}. For more information, see [detecting drift in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-drift-note&interface=api#drift-in-ibm).

## Can I initiate the drift detection?
{: #drift-initiate-faq}
{: faq}
{: support}

No, you cannot initiate the drift detection. For more information, see [detecting drift in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-drift-note&interface=api#drift-in-ibm).

## Where can I see the status of the drift detection? Or How can I know whether the workspace is in drift?
{: #drift-status-faq}
{: faq}
{: support}

To know the details of the drift detection job, you need to check the drift detection job log. The job log provides the details of the drift detection as `in progress` or `completed` with the appropriate status such as `failure` or `success`. For more information, see [detecting drift in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-drift-note&interface=api#drift-in-ibm).

## Can I `interrupt`, `force-stop`, or `terminate` the provisioning resources or a running job in {{site.data.keyword.bpshort}}?
{: #stopping-job-faq}
{: faq}
{: support}

Yes, you can interrupt, force-stop, or terminate the provisioning resources or a running job in {{site.data.keyword.bpshort}} by using the job types. For more information, see [stopping the job types](/docs/schematics?topic=schematics-interrupt-job#interrupt-types).

## How can I `POST` Cart API with a location as `eu-de` region and resolve `Incorrect Location Input` error?
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

## What CLI command is used to view the resources as in the {{site.data.keyword.bpshort}} Workspace resources?
{: #clicmdresource-job-faq}
{: faq}
{: support}

Use the [`state list`](/docs/schematics?topic=schematics-schematics-cli-reference#state-list) CLI command to view the same resources as in {{site.data.keyword.bplong_notm}} UI.


## How do I fix the `CreateworkspaceWithContext failed Bad request` error in creating {{site.data.keyword.bpshort}} resource to `eu-de` region by using Terraform?
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

## How can I encrypt sensitive value in Terraform state file when using {{site.data.keyword.bpshort}}?
{: #encrypt-state-file}
{: faq}
{: support}

No, currently Terraform do not support this feature due to its default behaviour. As a result you cannot encrypt the sensitive value in state file using schematics. For more information, see [sensitive-data](https://developer.hashicorp.com/terraform/language/state/sensitive-data) in state file.

Example to download the terraform state file:

``` sh
ibmcloud schematics state pull --id WORKSPACE_ID--template TEMPLATE_ID
```
{: pre}

## Why is {{site.data.keyword.bpshort}} workspace list variable defined by using CLI throws 400 error?
{: #wks-list-var}
{: faq}
{: support}

The {{site.data.keyword.bpshort}} workspace list variables store value should always be a HCL string. The `value` field must contain escaped string for the variable store for the list, map, or complex variable. For more information, see [Providing values to {{site.data.keyword.bpshort}} for the declared variables](/docs/schematics?topic=schematics-create-tf-config#declare-variable).

## Why is the Terraform version (`TF_VERSION`) updated through `JSON` file is not working?
{: #tf-version-update}
{: faq}
{: support}

 Currently, the workaround is to pass the `TF_VERSION` while updating the variable store. Then the {{site.data.keyword.bpshort}} will auto detect what is specified in the Terraform version block in the `TF` files. This is the default behaviour. The fact is that `v1.3.0` selects and suggests the version is not defined or has no upper limit. For more information, see [setting and changing the version](/docs/schematics?topic=schematics-migrating-terraform-version#terraform-version-upgrade1x).

## In each workspace job trigger the previously created resources gets force replaced with the new values. Can I reset or start with the new Terraform state file with each trigger?
{: #wks-job-trigger}
{: faq}
{: support}

No, you need to only create new workspace. For more information, see [Workspace job execution](/docs/schematics?topic=schematics-job-download#wks-job-execution).

## Can I bootstrap a {{site.data.keyword.bpshort}} Workspace with an existing Terraform state that are created elsewhere?
{: #tf-state-argument}
{: faq}
{: support}

Yes, you can use `--state` flag option through the [ibmcloud schematics workspace new](https://cloud.ibm.com/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new).

## What is the maximum length of characters that the {{site.data.keyword.bpshort}} Workspace name variable supports?
{: #wks-name-maxlength}
{: faq}
{: support}

The maximum length of characters that the {{site.data.keyword.bpshort}} Workspace name variable supports is 1 MB.


