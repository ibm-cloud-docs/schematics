---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-18"

keywords: schematics faqs, infrastructure as code, iac, schematics workspaces faq, workspaces faq

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# Workspaces
{: #workspaces-faq}

Answers to common questions about the {{site.data.keyword.bplong_notm}} Workspaces are classified into following section.
{: shortdesc}

## How do I overcome the authentication error when {{site.data.keyword.bpshort}} Workspaces is created by using API?
{: #createworkspace-authentication-error}
{: faq}
{: support}

You need to create the IAM access token for your {{site.data.keyword.cloud_notm}} Account. For more information, about creating IAM access token, see [Get token password](https://cloud.ibm.com/apidocs/iam-identity-token-api#gettoken-password){: external}. You can refer to, the following sample error message and the solution for the authentication error.
```text
Error: Request failes with status code: 400, BXNIMO137E: For the original authentication, client id 'default' was passed, refresh the token, client id 'bx' is used.
```

The [IAM API](/apidocs/iam-identity-token-api#gettoken-apikey){: external} documentation only shows how to create a `default token`. You can use the `refresh token` to get a new IAM access token if that token is expired. When the default client (no basic authorization header) as described in this documentation. The `refresh_token` cannot be used to retrieve a new IAM access token. When the IAM access token is about to be expired, use the API key to create a new access token as listed.

1. You need to create access_token and refresh_token.

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

While creating {{site.data.keyword.bpshort}} Workspaces or action {{site.data.keyword.bplong_notm}} takes a copy of the Terraform, or Ansible template from your Git repository and stores in a secured location. Before the template files is saved, {{site.data.keyword.bpshort}} analyses the files and are removed, based on the following conditions:

- The allowed file extension are `.cer, .cfg, .conf, .crt, .der, .gitignore, .html, .j2, .jacl, .js, .json, .key, .md, .netrc, .pem, .properties, .ps1, .pub, .py, .service, .sh, .tf, .tf.json, .tfvars, .tmpl, .tpl, .txt, .yaml, .yml, .zip, _rsa, license`.
- The allowed image extension are `.bmp, .gif, .jpeg, .jpg, .png, .so .tif, .tiff`.
- The files that are removed are `.asa, .asax, .exe, .php5, .pht, .phtml, .shtml, .swf, .tfstate, .tfstate.backup, .xap`.
- All files that are larger than 500 KB in size are removed. This file size limit does not apply for the allowed image files.

The allowed extension list is continuously monitored and updated in every release. You can raise an [support ticket](/docs/schematics?topic=schematics-schematics-help) with the justification to add a new file extension to the list.
{: note}

## How do I upgrade the Terraform versions in {{site.data.keyword.bpshort}}? or Can I update the version during workspace recreation?
{: #migrate-terraform-v11}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version).
{: deprecated}

You can follow these topics to upgrade from one Terraform version to another version

- [Upgrading the Terraform template version](/docs/schematics?topic=schematics-migrating-terraform-version&interface=ui#terraform-version-upgrade)
- [Upgrade Terraform version in {{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-migrating-terraform-version&interface=ui#migrate-steps)
- [Upgrade Terraform template from v0.12 to v0.13](/docs/schematics?topic=schematics-migrating-terraform-version&interface=ui#upgrade-12-to13)
- [Upgrade Terraform template from v0.13 to v0.14](/docs/schematics?topic=schematics-migrating-terraform-version&interface=ui#upgrade-13-to14)
- [Upgrade Terraform template from v0.14 to v1.0](/docs/schematics?topic=schematics-migrating-terraform-version&interface=ui#upgrade-14-to10)

## How do I overcome the downtime while updating the workspace activities? 
{: #impact-downtime-workspace}
{: faq}
{: support}

The unexpected impact due to maintenance results in the failure of the running activities in {{site.data.keyword.bpshort}} workspace. Such workspace and the ongoing activity is marked as `Failed`. The user can then re-execute the activity, for more information, about the workspace state, see [Workspace state diagram](/docs/schematics?topic=schematics-workspace-setup#workspace-state-diagram).

## Why do the jobs delay in a queue when plan is generated?
{: #job-queue-faq}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} queues all the users jobs into a single queue. Depending on the workload generated by the users and the time to run the jobs, the user might experience delays. For more information, about implementation of the {{site.data.keyword.bpshort}} job queue, see [Job queue status](/docs/schematics?topic=schematics-job-queue-process).

## How do I pull latest code from the Workspace through command-line? 
{: #latestcode-workspace-commandline}
{: faq}
{: support}

Updating the {{site.data.keyword.bpshort}} Workspaces through command-line need the required field `name`.

You need to run `ibmcloud schematics workspace update --id <workspace-id>  --file <updatefile.json>`  command. The sample `updatefile.json` contains the name field with the value.
```json
{
    "name":"testWorkspace"
}
```

## What are the development tools and utilities used in the {{site.data.keyword.bpshort}}?
{: #schematics-tools}
{: faq}
{: support}

{{site.data.keyword.bpshort}} runtime is built by using Universal Base Image (UBI-8) and the runtime utilities/softwares that come with the UBI-8 are available for Terraform provisioners and Ansible actions. For more information, refer to, the list of [tools and utilities](/docs/schematics?topic=schematics-sch-utilities) used in {{site.data.keyword.bpshort}} runtime.

## How can I create workspace from command-line by using Git repositories and personal access token with full permission?
{: #create-workspace-cli-tokens}
{: faq}
{: support}

Using `schematics workspace new --file schematic-file.json -g xxxx` command throws an **Access token creation failed status**, as the token name not specified in the command.

You need to check your [authentication](/docs/schematics?topic=schematics-setup-api#cs_api) before performing the post operation through command-line. Then create a workspace by using `schematics workspace new --file schematic-file.json --github-token xxxx` command. For more information, about workspace creation, see [ibmcloud schematics workspace new](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new).

## How do I overcome the authorization issue when creating or updating a workspace or a template?
{: #workspace-auth}
{: faq}
{: support}

You see authorization issues when the roles and permission access is insufficient while performing updates on workspace. For more information, about roles and permission, see [Managing user access](/docs/schematics?topic=schematics-access).

## How can I access the {{site.data.keyword.bpshort}} services for test ID?
{: #global-catalog-faq}
{: faq}
{: support}

The test IDs are considered as a valid IBM IDs to perform the global catalog or resource controller related API calls. If you are unable to access, do [Contact support service](/docs/schematics?topic=schematics-schematics-help).

## How can I download subfolders from the Git repositories through {{site.data.keyword.bpshort}}
{: #compact-faq}
{: faq}
{: support}

{{site.data.keyword.bpshort}} introduced a `compact` flag in the [create workspace](/apidocs/schematics/schematics#create-workspace) and [update workspace](/apidocs/schematics/schematics#replace-workspace) API to download the subfolder from the GIT repositories. If the compact flag is set to **true** you can download and save subfolder recursively, otherwise, you will continue to download and save the full repository on workspace creation.

You can get the response by invoking get workspace API to view the compact flag value. The compact flag can be given only if the `template_repo.url` field is passed. On update, if this field is not passed, but URL is passed, the download will be compact.

Compact usage in the payload is `.template_data[0].compact = true/false`. For more information, about compact, see [Compact download for {{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-compact-download).

## How do I resolve issue while trying to delete a workspace that was created for a cluster that no longer exists, deletion fails because of the cluster not found?
{: #clusterdeletion-warn-faq}
{: faq}
{: support}

You need to delete the workspace and NOT destroying the resources as if there are no resource available. For more information, refer to, [Deleting a workspace](/docs/schematics?topic=schematics-workspace-setup&interface=ui#del-workspace).

## what is the best way to deploy a Helm chart to an existing cluster by using {{site.data.keyword.bpshort}} keeping credentials or secrets?
{: #gherepo-warn-faq}
{: faq}
{: support}

The best way is to use {{site.data.keyword.cloud_notm}} catalog to manage the Helm charts where inside the catalog you can keep the credentials and mark it as secured. For more information, about managing the Helm chart in an existing cluster, refer to, [List of Catalog related to Helm](https://cloud.ibm.com/catalog?search=label%3Ahelm).

## How do I set the release tag through {{site.data.keyword.bpshort}}? 
{: #releasetag-warn-faq}
{: faq}
{: support}

```text
2021/11/08 12:34:06 -----  New Action  -----
 2021/11/08 12:34:06 Request: RepoURL=https://github.ibm.com/wh-hp-insights/hi-cloud-automation, WorkspaceSource=Schematics, Branch=2021.10, Release=, Folder=terraform-v2/workspace-hi-qa-automation-app
 2021/11/08 12:34:06 Related Activity: action=UPDATE_WORKSPACE,processedBy=sandbox-6bcf8bffcd-rxbww_2478
 2021/11/08 12:34:06 Getting download command
 2021/11/08 12:34:11 Fatal, could not download repo, Failed to clone git repository, couldn't find remote ref "refs/heads/2021.10" (most likely invalid branch name is passed)
 2021/11/08 12:34:12 Problems found with the Repository. Please Rectify and Retry
```
{: screen}

If the `Release` parameter is empty and the `Branch` was set with release tag.
{: note}

{{site.data.keyword.bpshort}} does not support `release` tag, as its difficult to identify if it’s a release tag or a branch from the Git repository URL. You need to set the `release` tag through the [{{site.data.keyword.bpshort}} API](/apidocs/schematics/schematics#create-workspace).

## Why I am getting 403 error instead of 404 error when providing an invalid workspace ID?
{: #invalidwspid-warn-faq}
{: faq}
{: support}

```sh
curl -X GET https://schematics.cloud.ibm.com/v1/workspaces/badWOrkspaceId -H "Authorization: $IAM_TOKEN"
{"requestid":"3a3cbffe-e23a-4ccf-b764-042f7379c084","timestamp":"2021-11-11T17:00:07.169953698Z","messageid":"M1078","message":"Error while validating the location in the account. Verify you have permission to the location in the global catalog settings.","statuscode":403}
```
{: pre}

Yes there is a change in the API which checks for the location first and if it doesn’t get proper location for the workspace it returns 403 error instead of 404 error.

## How can I enable Terraform debug through the `ibmcloud schematics` command line?
{: #terraform-debug-ibmcli}
{: faq}
{: support}

You can set the environment variable for setting the Terraform log debug `TF_LOG=debug` trace in the payload, as shown in the sample payload. For more information, about setting the environment variables in the payload, refer to, [{{site.data.keyword.bpshort}} Workspaces update](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update).

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

Use `ibmcloud schematics workspace import --options value, -o value : Optional` command and the sample syntax to import from command-line. For more information, about how {{site.data.keyword.bpshort}} Workspaces import works, see [{{site.data.keyword.bpshort}} Workspaces import](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-import).

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

 You need to update or increase the timeout value by 5 minutes or 10 minutes depending upon the service as shown in the terraform block. Or you need to send **null value** to use the default values.

 ```terraform
 variable "create_timeout" 
 {
  type = String
  description = "Timeout duration to create LogDNA instance in Schematics."
  default = "15m"
 }
 ```
 {: codeblock}

## Can I set TF_CLI_ARGS environment variable in the {{site.data.keyword.bpshort}} Workspaces console without using Catalog service or {{site.data.keyword.bpshort}} command line?
{: #terraformcli-arguments-faq}
{: faq}
{: support}

 No, you cannot set an environment variable values in the {{site.data.keyword.bpshort}} Workspaces console directly. Instead you can use a CURL by using the [{{site.data.keyword.bpshort}} API](/apidocs/schematics/schematics#create-workspace), or [{{site.data.keyword.bpshort}} command line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new).

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

## Can I set or manage keys for  `ibm_kms_key` resource when {{site.data.keyword.bpshort}} Workspaces imports Terraform?
{: #kmskey-value-faq}
{: faq}
{: support}

Yes, you can set or manage the keys by using ibm_kms_key. Here is sample codeblock to provision KMS keys. For more information, about provision KMS key with key policies see [ibm_kms_key]( https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/kms_key#import). 

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

## Can I enable the TRACE to help DEBUG {{site.data.keyword.bpshort}} API while running workspace list command?
{: #traces-api-faq}
{: faq}
{: support}

No, currently {{site.data.keyword.bpshort}} do not support this feature while executing `IBMCLOUD_TRACE=true ibmcloud schematics workspace list` command. 

## How do I overcome the `Error while retrieving {{site.data.keyword.bpshort}} Instance for the given account` while trying to fetch {{site.data.keyword.bpshort}} Workspaces?
{: #badstatus-workspace-faq}
{: faq}
{: support}

```text
Error:
Bad status code [400] returned when getting workspace from Schematics: {"requestid":"fe5f0d6d-1d43-4643-a689-35d090463ce8","timestamp":"2022-01-25T20:23:54.727208017Z","messageid":"M1070","message":"Error while retrieving Schematics Instance for the given account.","statuscode":400}
```

You might have insufficient access for the workspaces in specified location to fetch the instance. Do check the permission provided for your account and the locations where your instance need to be created. For more information, about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).

## How can I configure private (IBM) GitLab repository in {{site.data.keyword.bpshort}} workspace?
{: #gitlab-workspace-faq}
{: faq}
{: support}

Yes, You can access the private (IBM) GitLab repository by using {{site.data.keyword.bpshort}} with required privileges.
- In case of private (IBM) GitLab repository **git.cloud.ibm.com**, access token is not needed as the IAM token is used.
- In case of public GitLab **gitlab.com**, `read_repository` and `read_api` access is needed to validate the given branch name for private repository.

You can use the sample Terraform codeblock to configure the GitLab repository details.

```terraform
"template_repo": {
"url": "<gitlab_source_repo_url>",
"branch": ""
},
```
{: codeblock}

## Does {{site.data.keyword.cloud_notm}} provider supports managing IAM access groups in {{site.data.keyword.bpshort}}?
{: #manageaccessgrp-iam-faq}
{: faq}
{: support}

Yes, {{site.data.keyword.bpshort}} supports the full {{site.data.keyword.cloud_notm}} provider resource set. For more information, about How IAM access group works? see [ibm_iam_access_group](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_access_group).

## Could I create {{site.data.keyword.bpshort}} Workspaces in {{site.data.keyword.cloud_notm}} source account and execute Terraform providing resources in {{site.data.keyword.cloud_notm}} target account to provision?
{: #account-resource-faq}
{: faq}
{: support}

Yes, you can create {{site.data.keyword.bpshort}} Workspaces in {{site.data.keyword.cloud_notm}} source account and execute Terraform providing resources in target account to provision, through command-line and API calls by using the target account service ID with authentication, appropriate cross account authorization, or API key. For more information, refer to, [Managing resources in other account](/docs/schematics?topic=schematics-create-tf-config#manage-resource-account).

## Does `North America` location indicates `us-south`, `us-east`, or `both` during the {{site.data.keyword.bpshort}} Workspaces creation?
{: #location-faq}
{: faq}
{: support}

North America always indicates both `us-south` and `us-east` location during the {{site.data.keyword.bpshort}} Workspaces creation. For more information, refer to, [Where can I create {{site.data.keyword.bpshort}} Workspaces?](/docs/schematics?topic=schematics-locations#where-can-i-create-schematics-workspaces) and [Where is my information stored](/docs/schematics?topic=schematics-secure-data#pi-location)?

## What are the port used to communicate with {{site.data.keyword.bpshort}} and resources, such as VPC services?
{: #port-faq}
{: faq}
{: support}

{{site.data.keyword.bpshort}} communicates with the ports specified by the related resources. For example, VPC related ports, refer to, [VPC: Opening required ports and IP addresses in other network firewalls](/docs/containers?topic=containers-vpc-firewall). 

## When do I use {{site.data.keyword.bplong_notm}} versus the individual resource dashboards?
{: #schematics-vs-cloud-resource-faq}
{: faq}
{: support}

With {{site.data.keyword.bplong_notm}}, you can run your infrastructure code in {{site.data.keyword.cloud_notm}} to manage the lifecycle of {{site.data.keyword.cloud_notm}} resources. After you provision a resource, you use the dashboard of the individual resource to work and interact with your resource. For example, if you provision a virtual server instance in a Virtual Private Cloud (VPC) with {{site.data.keyword.bplong_notm}}, you use the VPC console, API, or command-line to stop, reboot, and power on your virtual server instance. However to remove the virtual server instance, you use {{site.data.keyword.bplong_notm}}.

## When I change my configuration file in GitHub, is my change automatically available in the next execution plan?
{: #edit-resource-confg-faq}
{: faq}
{: support}

If you change the code of your Terraform template in GitHub, these changes are not available automatically when you create an execution plan in {{site.data.keyword.bplong_notm}}. To pull the latest changes from your GitHub repository, make sure that you click the **Pull latest** button from the workspace settings page before you create your execution plan.

## Where does {{site.data.keyword.bpshort}} store the state of my cloud resources?
{: #resource-state-faq}
{: faq}
{: support}

After you successfully provisioned {{site.data.keyword.cloud_notm}} resources by running a {{site.data.keyword.bpshort}} apply action, the state of resources is stored in a Terraform state file (`terraform.tfstate`). {{site.data.keyword.bpshort}} uses this state file as the single source of truth to determine what resources exist in your account. The state file maps the resources that you specified in your Terraform configuration file to the {{site.data.keyword.cloud_notm}} resource that you provisioned.

## Are my resources removed when I remove the workspace?
{: #delete-resource-wks-faq}
{: faq}
{: support}

Removing the workspace from {{site.data.keyword.bplong_notm}} does not remove any of your {{site.data.keyword.cloud_notm}} resources. If you remove the workspace before you removed your resources, you must manually remove all your {{site.data.keyword.cloud_notm}} resources from the individual resource dashboard.

Removing an {{site.data.keyword.cloud_notm}} resource cannot be undone. Make sure that you backed up your data before you remove a resource. If you choose to remove the infrastructure code, or comment out the resource in your Terraform configuration file, make sure to thoroughly review the log file of your execution plan to verify that all your resources are included in the removal.    
{: important}

## How can I update a workspace created through payload in command-line to resolve invalid payload issue?
{: #invalid-paylaod-cli}
{: faq}
{: support}

You can use `env values` as shown in the sample payload to update the workspace in the command-line. For more information, about creating and using environment variables, see [usage of `env_values`](/docs/schematics?topic=schematics-set-parallelism#parelleism-usage).

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

## Is the drift detection a automatic in the {{site.data.keyword.bplong_notm}}?
{: #drift-automatic-faq}
{: faq}
{: support}

No, the drift detection is not a automatic method of detection in the {{site.data.keyword.bplong_notm}}. For more information, refer to, [detecting drift in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-drift-note&interface=api#drift-in-ibm).

## Can I initiate the drift detection?
{: #drift-initiate-faq}
{: faq}
{: support}

No, you cannot initiate the drift detection. For more information, refer to, [detecting drift in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-drift-note&interface=api#drift-in-ibm).

## Where can I see the status of the drift detection? Or How can I know if the workspace has drifted?
{: #drift-status-faq}
{: faq}
{: support}

In order to know the details of the drift detection job, you need to check the drift detection job log. The job log provides the details of the drift detection as `in progress` or `completed` with the appropriate status such as `failure` or `success`. For more information, refer to, [detecting drift in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-drift-note&interface=api#drift-in-ibm).

## Can I interrupt, force-stop, or terminate the provisioning resources or a running job in {{site.data.keyword.bpshort}}?
{: #stopping-job-faq}
{: faq}
{: support}

Yes, you can interrupt, force-stop, or terminate the provisioning resources or a running job in {{site.data.keyword.bpshort}} by using the job types. For more information, refer to, [stopping the job types](/docs/schematics?topic=schematics-interrupt-job&interface=ui#interrupt-types).

