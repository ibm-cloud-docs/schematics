---

copyright:
  years: 2017, 2022
lastupdated: "2022-04-15"

keywords: schematics faqs, what is terraform, infrastructure as code, iac, schematics price, schematics pricing, schematics cost, schematics charges, schematics personal information, schematics pii, delete pii from schematics, schematics compliance

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}


# FAQs
{: #faqs}

Answers to common questions about the {{site.data.keyword.bplong_notm}} service are classified into following section.
{: shortdesc}

## What is {{site.data.keyword.bplong_notm}} and how does it work? 
{: #what-is-schematics}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} provides powerful tools to automate your cloud infrastructure provisioning and management process, the configuration and operation of your cloud resources, and the deployment of your app workloads.

To do so, {{site.data.keyword.bpshort}} leverages open source projects, such as Terraform, Ansible, OpenShift, Operators, and Helm, and delivers these capabilities to you as a managed service. Rather than installing each open source project on your machine, and learning the API or CLI, you declare the tasks that you want to run in {{site.data.keyword.cloud_notm}} and watch {{site.data.keyword.bpshort}} run these tasks for you.

For more information about how {{site.data.keyword.bpshort}} works, see [About {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-about-schematics).

## What is Infrastructure as Code?
{: #what-is-iac}
{: faq}

Infrastructure as Code (IaC) helps you codify your cloud environment so that you can automate the provisioning and management of your resources in the cloud. Rather than manually provisioning and configuring infrastructure resources or by using scripts to adjust your cloud environment, you use a high-level scripting language to specify your resource and its configuration. Then, you use tools like Terraform to provision the resource in the cloud by leveraging its API. Your infrastructure code is treated the same way as your app code so that you can apply DevOps core practices such as version control, testing, and continuous monitoring.

## What am I charged for when I use {{site.data.keyword.bpshort}}?
{: #charges}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} workspaces are provided to you at no cost. However, when you decide to apply your Terraform template in {{site.data.keyword.cloud_notm}} by clicking **Apply plan** from the workspace details page or running the `ibmcloud schematics apply` command, you are charged for the {{site.data.keyword.cloud_notm}} resources that are described in your Terraform template. Review available service plans and pricing information for each resource that you are about to create. Some services come with a limit per {{site.data.keyword.cloud_notm}} account. If you are about to reach the service limit for your account, the resource is not provisioned until you increase the service quota, or remove existing services first.

The {{site.data.keyword.bpshort}} `ibmcloud terraform` command usage displays warning and deprecation message as **Alias 'terraform' will be deprecated. Please use 'schematics' or 'sch' in your commands.**
{: note}

## Can I run Ansible playbooks with {{site.data.keyword.bpshort}}?
{: #ansible-playbooks}
{: faq}
{: support}

Yes, you can run Ansible playbooks or {{site.data.keyword.bpshort}} actions against your {{site.data.keyword.cloud_notm}} by using the Ansible provisioner in your Terraform configuration file. For example, use the Ansible provisioner to deploy software on {{site.data.keyword.cloud_notm}} resources or perform actions against your resources, such as shutting down a virtual server instance. For more information, about how to use the Ansible provisioner, see the following blogs:

- [Discover best-practice VPC configuration for application deployment](https://developer.ibm.com/articles/secure-vpc-access-with-a-bastion-host-and-terraform/){: external}
- [Learn about repeatable and reliable end-to-end app provisioning and configuration](https://developer.ibm.com/articles/application-deployment-with-redhat-ansible-and-ibm-cloud-schematics/){: external}

## Does {{site.data.keyword.bpfull_notm}} support multiple Terraform provider versions?
{: #provider-versions}
{: faq}
{: support}

Yes, {{site.data.keyword.bpfull_notm}} supports multiple Terraform provider versions. You need to add Terraform provider block with the right provider version. By default the provider executes latest version `1.21.0`, and previous four versions such as `1.20.1`, `1.20.0`, `1.19.0`, `1.18.0` are supported.

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

Currently, version 1.21.0 is released. For more information, about provider version, refer to, [provider version](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-setup_cli#install_provider).
{: note}

## When are the new Terraform and Ansible versions added to {{site.data.keyword.bpshort}}?
{: #new-versions}
{: faq}
{: support}

After new Terraform and Ansible versions are released by the community, the IBM team begins hardening and testing the release for {{site.data.keyword.bpshort}}. Availability of new versions depend on the results of these tests, community updates, security patches, and technology changes between versions. Make sure that your Terraform templates and Ansible playbooks are compatible with one of the supported versions so that you can run them in {{site.data.keyword.bpshort}}.

## How do I generate IAM access token, if client id `bx` is used?
{: #createworkspace-generate-tokens}
{: faq}
{: support}

To create IAM access token, use `export IBMCLOUD_API_KEY=<ibmcloud_api_key>` and execute 
```sh
curl -X POST "https://iam.cloud.ibm.com/identity/token" -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=$IBMCLOUD_API_KEY" -u bx:bx.
``` 

For more information, about creating IAM access token and API Docs, see [IAM access token](https://cloud.ibm.com/apidocs/iam-identity-token-api#gettoken-password) and [Create API key](https://cloud.ibm.com/apidocs/iam-identity-token-api#create-api-key). <br> You can set the environment values  `export ACCESS_TOKEN=<access_token>`, and `export REFRESH_TOKEN=<refresh_token>`. 

## How do I overcome the authentication error when {{site.data.keyword.bpshort}} workspace is created by using API?
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

## How do I rectify 'Failed to clone git repository, couldn’t find remote ref “refs/heads/master” (most likely invalid branch name is passed)'?
{: #template-errors}
{: faq}
{: support}

Usage of the branch `https://github.com/guruprasad0110/tf_cloudless_sleepy_13/` repository, after 1st October 2020, can see this error message. 

If the repository is created after 1st October 2020, the main branch syntax needs to be `https://github.com/username/reponame/tree/main`. For example, `https://github.com/guruprasad0110/tf_cloudless_sleepy_13/tree/main`


## Can I increase the timeout for null-exec and remote-exec resource?
{: #timeout-null-resource}
{: faq}
{: support}

No, the null-exec (null_resources) and remote-exec resources has maximum timeout of `60 minutes`. Longer jobs need to be broken into shorter blocks to provision the infrastructure faster. Otherwise the execution times out automatically after `60 minutes`.

## How do {{site.data.keyword.bpshort}} decide to remove the files from the Terraform or Ansible templates?
{: #clone-file-extension}
{: faq}
{: support}

While creating {{site.data.keyword.bpshort}} workspace or action {{site.data.keyword.bplong_notm}} takes a copy of the Terraform or Ansible template from your Git repository and stores in a secured location. Before the template files is saved, {{site.data.keyword.bpshort}} analyses the files and are removed, based on the following conditions:

- The allowed file extension are `.cer, .cfg, .conf, .crt, .der, .gitignore, .html, .j2, .jacl, .js, .json, .key, .md, .netrc, .pem, .properties, .ps1, .pub, .py, .service, .sh, .tf, .tf.json, .tfvars, .tmpl, .tpl, .txt, .yaml, .yml, .zip, _rsa, license`.
- The allowed image extension are `.bmp, .gif, .jpeg, .jpg, .png, .so .tif, .tiff`.
- The files that are removed are `.asa, .asax, .exe, .php5, .pht, .phtml, .shtml, .swf, .tfstate, .tfstate.backup, .xap`.
- All files that are larger than 500 KB in size are removed. This file size limit does not apply for the allowed image files.

The allowed extension list is continuously monitored and updated in every release. You can raise an [support ticket](/docs/schematics?topic=schematics-schematics-help) with the justification to add a new file extension to the list.
{: note}

## How can I save user-defined files generated by the Terraform modules and use them across multiple Terraform plan, apply, destroy, refresh, or import commands?
{: #persist-file}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} already persists and securely manages the state file generated by the Terraform engine in a {{site.data.keyword.bpshort}} workspace. {{site.data.keyword.bpshort}} periodically saves the state file in the secured location. Further the state file is automatically restored before running the {{site.data.keyword.bpshort}} job or Terraform plan, apply, destroy, refresh, or import commands.

In the same way {{site.data.keyword.bplong_notm}} supports the ability to persist user-defined files, that are generated by the Terraform template or modules. {{site.data.keyword.bpshort}} expects the user-defined Terraform template or modules to generate and place the files in a predefined location. {{site.data.keyword.bpshort}} will automatically save and restore them, before and after running the {{site.data.keyword.bpshort}} jobs or Terraform command.

Your files must be placed in the `/tmp/.schematics` folder and the size limit is set to `10 MB`. {{site.data.keyword.bpshort}} backups and restores all the files in the `/tmp/.schematics` folder.

## How do I upgrade the Terraform versions in {{site.data.keyword.bpshort}}? or Can I update the version during workspace recreation?
{: #migrate-terraform-v11}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} deprecates older version of Terraform. For more information, see [Deprecating older version of Terraform process in {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-deprecate-tf-version).
{: deprecated}

You can follow these topics to upgrade from one Terraform version to another version

- [Upgrading the Terraform template version](/docs/schematics?topic=schematics-migrating-terraform-version&interface=ui#terraform-version-upgrade)
- [Upgrade Terraform version in {{site.data.keyword.bpshort}} workspace](/docs/schematics?topic=schematics-migrating-terraform-version&interface=ui#migrate-steps)
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

Updating the {{site.data.keyword.bpshort}} workspace through command-line need the required field `name`.

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

{{site.data.keyword.bpshort}} functions are built by using [Universal Base Image (UBI-8)](/docs/RegistryImages?topic=RegistryImages-ibmliberty#ibmliberty_get_started) and the runtimes that come with the UBI-8 are available in {{site.data.keyword.bpshort}} workspace and actions. For more information, to use these tools in multiple Operating System, refer to, [Solution tutorials](/docs/solution-tutorials?topic=solution-tutorials-tutorials).

The following table describes the utilities that are used in {{site.data.keyword.bpshort}} functions and an automation scripts:

|Utilities | Description | 
|---------|----------|
| `Python 3` | {{site.data.keyword.bpshort}} uses [Python 3](/docs/cli?topic=cli-enable-existing-python) and higher version to analyze and organize the data, and to automate DevOps. | 
| `Python 3 - pip` |Standard package manager for Python. Allows you to install and manage modules that are not part of the [Python standard library](https://docs.python.org/3/library/){: external}. For example, `netaddr`, `kubernetes`, `OpenShift`, `ibm-cloud-sdk-core`, `ibm-vpc`.| 
| `OpenShift client` |A [service built docker containers](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_oc) that are orchestrated and managed by Kubernetes on a foundation of Red Hat Enterprise Linux.| 
| `Ansible 2.9.23`| {{site.data.keyword.bpshort}} uses [Ansible v2.9.23](/docs/cloud-pak-multicloud-management?topic=cloud-pak-multicloud-management-ansible-getting-started) and higher. |
| `Terraform v12 and v13`|   Automates your resource provisioning. [Terraform v12, and v13](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started) to use the modules and resource in {{site.data.keyword.bpshort}} workspace. Actions support Terraform v11 and v12. |
| `kubectl`| A command-line interface for running commands against the [Kubernetes](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_kubectl) clusters.|
| `{{site.data.keyword.cloud_notm}} CLI v.1.2.0`| The [command-line](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_cli) to interact with {{site.data.keyword.cloud_notm}} API.|
| `JQ 1.6`| A lightweight and flexible command-line [JSON processor](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_jq).|
| `Helm` |A chart to define, install, and upgrade the most complex [Kubernetes application](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_helm).|
{: caption="Utilities to create script for automation." caption-side="top"}

To avoid the installation of these tools, you can also use the [Cloud Shell](https://cloud.ibm.com/shell) from the {{site.data.keyword.cloud_notm}} console.
{: tip}


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

## How do I identify the best way to synchronize a deleted resource with the Terraform state?
{: #sync-delresource-terraform}
{: faq}
{: support}

Currently, the {{site.data.keyword.bplong_notm}} service does not support the ability to import or synchronize the {{site.data.keyword.cloud_notm}} resource state into the {{site.data.keyword.bpshort}} workspace. It is planned in the future roadmap.

## How can I access the {{site.data.keyword.bpshort}} services for test ID?
{: #global-catalog-faq}
{: faq}
{: support}

The test IDs are considered as a valid IBM IDs to perform the global catalog or resource controller related API calls. If you are unable to access, please do [Contact support service](/docs/schematics?topic=schematics-schematics-help).

## How can I download sub-folders from the Git repositories through {{site.data.keyword.bpshort}}
{: #compact-faq}
{: faq}
{: support}

{{site.data.keyword.bpshort}} introduced a `compact` flag in the [create workspace](/apidocs/schematics/schematics#create-workspace) and [update workspace](/apidocs/schematics/schematics#replace-workspace) API to download the sub-folder from the GIT repositories. If the compact flag is set to **true** you can download and save sub-folder recursively, otherwise, you will continue to download and save the full repository on workspace creation.

You can get the response by invoking get workspace API to view the compact flag value. The compact flag can be given only if the `template_repo.url` field is passed. On update, if this field is not passed, but URL is passed, the download will be compact.

Compact usage in the payload is `.template_data[0].compact = true/false`. For more information, about compact, see [Compact download for {{site.data.keyword.bpshort}} workspace](/docs/schematics?topic=schematics-compact-download).

## Why is my success Action job execution displays DEPRECATION WARNING message?
{: #deprecation-warn-faq}
{: faq}
{: support}

In the Action settings page you need to set the input variable as `ansible_python_interpreter = auto` as shown in the screen capture to avoid `DEPRECATION WARNING` message.

![Configuring input variable to silence warning message](images/advanced_inputvariable.png "Embedded {{site.data.keyword.bplong_notm}} service flow"){: caption="Configuring input variable to silence warning message" caption-side="bottom"}

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

If the `Release` parameter is empty and the `Branch` was set with release tag.
{: note}

{{site.data.keyword.bpshort}} does not support `release` tag, as its difficult to identify if it’s a release tag or a branch from the Git repository URL. You need to set the `release` tag through the [{{site.data.keyword.bpshort}} API](https://cloud.ibm.com/apidocs/schematics#create-workspace).

##  How do I overcome the request exceeding the Cluster resource quota of '100' for the account in any region?
{: #clusterquota-warn-faq}
{: faq}
{: support}

```text
Error: Request failed with status code: 403, ServerErrorResponse: {"incidentID":"706efb2c-3461-4b9d-a52c-038fda3929ea,706efb2c-3461-4b9d-a52c-038fda3929ea","code":"E60b6","description":"This request exceeds the 'Cluster' resource quota of '100' for the account in this region. Your account already has '100' of the resource in the region, and the request would add '1'. Revise your request, remove any unnecessary resources, or contact IBM support to increase your quota.","type":"General"}
```

You see this quota validation error when the `Cluster` resource quota of `100` for the account in this region is exceeded. You can consider deleting the existing resources and try performing the executing operation again.

## Why I am getting 403 error instead of 404 error when providing an invalid workspace ID?
{: #invalidwspid-warn-faq}
{: faq}
{: support}

```sh
curl -X GET https://schematics.test.cloud.ibm.com/v1/workspaces/badWOrkspaceId -H "Authorization: $IAM_TOKEN"
{"requestid":"3a3cbffe-e23a-4ccf-b764-042f7379c084","timestamp":"2021-11-11T17:00:07.169953698Z","messageid":"M1078","message":"Error while validating the location in the account. Please verify you have permission to the location in the global catalog settings.","statuscode":403}
```
Yes there is a change in the API which checks for the location first and if it doesn’t get proper location for the workspace it returns 403 error instead of 404 error.

## While creating OpenShift or Kubernetes resources, can I tune 90 minutes time out to higher?
{: #resourcetimeout-warn-faq}
{: faq}
{: support}

Yes, you can increase the time out for OpenShift or Kubernetes resources. For more information, about managing or adding the time out option for the cluster resource, see [ibm_container_vpc_cluster](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/container_vpc_cluster#timeouts) provides the following Timeouts configuration options.


## How can I enable Terraform debug through the `ibmcloud schematics` command line?
{: #terraform-debug-ibmcli}
{: faq}
{: support}

You can set the environment variable for setting the Terraform log debug `TF_LOG=debug` trace in the payload, as shown in the sample payload. For more information, about setting the environment variables in the payload, refer to [{{site.data.keyword.bpshort}} workspace update](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update).

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

## How can I generate {{site.data.keyword.bpshort}} workspace import from CLI?
{: #workspace-import-ibmcli}
{: faq}
{: support}

Use `ibmcloud schematics workspace import --options value, -o value : Optional` command and the sample syntax to import from command-line. For more information, about how {{site.data.keyword.bpshort}} workspace import works, see [{{site.data.keyword.bpshort}} workspace import](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-import).

``` sh

ibmcloud {{site.data.keyword.bpshort}} workspace import --id <workspace_id> --address <my terraform resource address> --resourceID <the CRN of the item to import> --options "-var IC_API_KEY=XXXXXXXX"

or 

ibmcloud {{site.data.keyword.bpshort}} workspace import --id <workspace_id> --address <my terraform resource address> --resourceID <the CRN of the item to import> --options "--var-file=<path-to-var-file>"
```
{: pre}

## Can I download the {{site.data.keyword.bpshort}} Job files?
{: #download-jobfile}
{: faq}
{: support}

Yes, you can download the {{site.data.keyword.bpshort}} Job files. For more information, see [Download {{site.data.keyword.bpshort}} Job files](/docs/schematics?topic=schematics-job-download).

## How can I rectify the 403 Error while validating the location in the account. Please verify you have permission to the location in the global catalog settings?
{: #global-setting-location}
{: faq}
{: support}

You can verify the location access to create or view the resource in the catalog settings for your account. For more information, about how to verify the location permission?, see [Manage location settings in global catalog](/docs/schematics?topic=schematics-access-ibm-cloud-catalog).

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

## Can I set TF_CLI_ARGS environment variable in the {{site.data.keyword.bpshort}} workspace console without using Catalog service or {{site.data.keyword.bpshort}} command line?
{: #terraformcli-arguments-faq}
{: faq}
{: support}

 No, you cannot set an environment variable values in the {{site.data.keyword.bpshort}} workspace console directly. Instead you can use a CURL by using the [{{site.data.keyword.bpshort}} API](/apidocs/schematics/schematics#create-workspace), or [{{site.data.keyword.bpshort}} command line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new).

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

## How can I resolve the could not execute action error while provisioning WinRM by using {{site.data.keyword.bpshort}} action?
{: #winrm-faq}
{: faq}
{: support}

 ```text
 Error: 2021/12/06 10:15:49 Terraform apply | Error: Error running command 'ANSIBLE_FORCE_COLOR=true ansible-playbook ansible.yml --inventory-file='inventory.yml' --extra-vars='{"ansible_connection":"winrm","ansible_password":"password","ansible_user":"administrator","ansible_winrm_server_cert_validation":"ignore"}' --forks=15 --user='root' --ssh-extra-args='-p 22 -o ConnectTimeout=120 -o ConnectionAttempts=3 -o StrictHostKeyChecking=no'': exit status 2. Output:
  2021/12/06 10:15:49 Terraform apply | PLAY [Please wait and have a coffee! The show is about to begin....] ***********
  2021/12/06 10:15:49 Terraform apply |
  2021/12/06 10:15:49 Terraform apply | TASK [Gathering Facts] *********************************************************
  2021/12/06 10:15:49 Terraform apply | fatal: [161.156.161.7]: FAILED! => {"msg": "winrm or requests is not installed: No module named 'winrm'"}
  2021/12/06 10:15:49 Terraform apply |
  2021/12/06 10:15:49 Terraform apply | PLAY RECAP *********************************************************************
  2021/12/06 10:15:49 Terraform apply | 161.156.161.7              : ok=0    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
  2021/12/06 10:15:49 Terraform apply |
  2021/12/06 10:15:49 Terraform apply |
  2021/12/06 10:15:49 Terraform apply |
  2021/12/06 10:15:49 Terraform apply |   with null_resource.schematics_for_windows,
  2021/12/06 10:15:49 Terraform apply |   on schematics.tf line 2, in resource "null_resource" "schematics_for_windows":
  2021/12/06 10:15:49 Terraform apply |    2:   provisioner "ansible" {
  2021/12/06 10:15:49 Terraform apply |
  2021/12/06 10:15:50 Terraform APPLY error: Terraform APPLY errorexit status 1
  2021/12/06 10:15:50 Could not execute action
```

WinRM is not supported by {{site.data.keyword.bpshort}} Terraform Ansible provisioner. Alternatively you can use the {{site.data.keyword.bpshort}} actions to run the Ansible playbooks with WinRM. The {{site.data.keyword.bpshort}} actions supports [WinRM](/docs/schematics?topic=schematics-action-setup).

## Can I edit all the variables in the {{site.data.keyword.bpshort}} console instead of editing individually?
{: #edit-variables-faq}
{: faq}
{: support}

You can edit one variable at a time from {{site.data.keyword.bpshort}} console. From the command line you can edit all the variables of the workspace in the JSON format by using [`ibmcloud schematics workspace update`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update) command.

## Can I start or stop the {{site.data.keyword.vsi_is_short}} based on tags and through scheduler or cron job?
{: #vm-tags-faq}
{: faq}
{: support}

 Yes, you can use {{site.data.keyword.openwhisk_short}} to perform the managed operations such as start, stop query based on tags and also through scheduler or cron job to trigger the {{site.data.keyword.bpshort}} action. For more information, see [VSI operations and schedule solution](https://github.com/Cloud-Schematics/vsi-operations-scheduler-solution){: external} GitHub repository.
 

## Can I set or manage keys for  `ibm_kms_key` resource when {{site.data.keyword.bpshort}} workspace imports Terraform?
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

## How do I overcome the `Error while retrieving {{site.data.keyword.bpshort}} Instance for the given account` while trying to fetch {{site.data.keyword.bpshort}} workspaces?
{: #badstatus-workspace-faq}
{: faq}
{: support}

```text
Error:
Bad status code [400] returned when getting workspace from Schematics: {"requestid":"fe5f0d6d-1d43-4643-a689-35d090463ce8","timestamp":"2022-01-25T20:23:54.727208017Z","messageid":"M1070","message":"Error while retrieving Schematics Instance for the given account.","statuscode":400}
```

You might have insufficient access for the workspaces in specified location to fetch the instance. Please do check the permission provided for your account and the locations where your instance need to be created. For more information, about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).

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

## Could I create {{site.data.keyword.bpshort}} workspace in {{site.data.keyword.cloud_notm}} source account and execute Terraform providing resources in {{site.data.keyword.cloud_notm}} target account to provision?
{: #account-resource-faq}
{: faq}
{: support}

Yes, you can create {{site.data.keyword.bpshort}} workspace in {{site.data.keyword.cloud_notm}} source account and execute Terraform providing resources in target account to provision, through command-line and API calls by using the target account service ID with authentication, appropriate cross account authorization, or API key. For more information, refer to [Managing resources in other account](/docs/schematics?topic=schematics-create-tf-config#manage-resource-account).

## Could I create a worker node in an existing worker node pool?
{: #workernode-kubernetes-faq}
{: faq}
{: support}

Yes, you can create or add a worker node inside an existing worker node pool by using {{site.data.keyword.IBM_notm}} container worker pool resource in a Kubernetes cluster through {{site.data.keyword.bpshort}}, or Terraform by using {{site.data.keyword.IBM_notm}} container worker pool zone attachment resource. For more information, about {{site.data.keyword.IBM_notm}} container worker pool zone, refer to [ibm_container_worker_pool_zone_attachment](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/container_worker_pool_zone_attachment){: external}.


## Where can I view the list of public and private allowed IP addresses of `us-south`, `us-east`, `eu-gb`, and `eu-de` regions?
{: #privateip-workspace-faq}
{: faq}
{: support}

You can view the list of public and private allowed IP addresses of `us-south`, `us-east`, `eu-gb`, and `eu-de` regions in [{{site.data.keyword.bpshort}} allowed IP addresses](/docs/schematics?topic=schematics-allowed-ipaddresses).

## Does `North America` location indicates `us-south`, `us-east`, or `both` during the {{site.data.keyword.bpshort}} workspace creation?
{: #location-faq}
{: faq}
{: support}

North America always indicates both `us-south` and `us-east` location during the {{site.data.keyword.bpshort}} workspace creation. For more information, refer to [Where can I create {{site.data.keyword.bpshort}} workspaces?](/docs/schematics?topic=schematics-locations#where-can-i-create-schematics-workspaces) and [Where is my information stored](/docs/schematics?topic=schematics-secure-data#pi-location)?

## What are the port used to communicate with {{site.data.keyword.bpshort}} and resources, such as VPC services?
{: #port-faq}
{: faq}
{: support}

{{site.data.keyword.bpshort}} communicates with the ports specified by the related resources. For example, VPC related ports, refer to [VPC: Opening required ports and IP addresses in other network firewalls](/docs/containers?topic=containers-vpc-firewall). 



## When do I use {{site.data.keyword.bplong_notm}} versus the individual resource dashboards?
{: #schematics-vs-cloud-resource-faq}
{: faq}
{: support}

With {{site.data.keyword.bplong_notm}}, you can run your infrastructure code in {{site.data.keyword.cloud_notm}} to manage the lifecycle of {{site.data.keyword.cloud_notm}} resources. After you provision a resource, you use the dashboard of the individual resource to work and interact with your resource. For example, if you provision a virtual server instance in a Virtual Private Cloud (VPC) with {{site.data.keyword.bplong_notm}}, you use the VPC console, API, or command-line to stop, reboot, and power on your virtual server instance. However to remove the virtual server instance, you use {{site.data.keyword.bplong_notm}}.

## Can I manually add, or remove a resource from the service dashboard directly?
{: #add-remove-resource-faq}
{: faq}
{: support}

When you provision resources with {{site.data.keyword.bplong_notm}}, the state of your resources is stored in a local {{site.data.keyword.bplong_notm}} state file. This state file is the single source of truth for {{site.data.keyword.bplong_notm}} to determine what resources are provisioned in your {{site.data.keyword.cloud_notm}} account. If you manually add a resource without {{site.data.keyword.bplong_notm}}, this resource is not stored in the {{site.data.keyword.bplong_notm}} state file, and as a consequence cannot be managed with {{site.data.keyword.bplong_notm}}. 

When you manually remove a resource that you provisioned with {{site.data.keyword.bplong_notm}}, the state file is not updated automatically and becomes out of sync. When you create your next Terraform execution plan or apply a new template version, {{site.data.keyword.bpshort}} verifies that the {{site.data.keyword.cloud_notm}} resources in the state file exist in your {{site.data.keyword.cloud_notm}} account with the state that is captured in your state file. If the resource is not found, the state file is updated and the Terraform execution plan is changed accordingly. 

To keep your {{site.data.keyword.bplong_notm}} state file and the {{site.data.keyword.cloud_notm}} resources in your account in sync, use {{site.data.keyword.bplong_notm}} to provision, or remove your resources. 
{: important}

## What changes can I make to my resources?
{: #edit-resource-faq}
{: faq}
{: support}

You can choose to add, modify, or remove infrastructure code in your Terraform template in GitHub, or update variable values from the workspace dashboard in {{site.data.keyword.bplong_notm}}.  

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

## How can I compare the required state of my cloud resources against the actual state of my resources?
{: #required-resource-state-faq}
{: faq}
{: support}

To create a deviation report and view the changes between the infrastructure and platform services that you specified in your Terraform configuration files and the resources that exist in your {{site.data.keyword.cloud_notm}} account, you can use Terraform execution plans. A Terraform execution plan summarizes what actions {{site.data.keyword.bpshort}} needs to take to provision the cloud environment that is described in your Terraform configuration files. These actions can include adding, modifying, or removing {{site.data.keyword.cloud_notm}} resources.

## What deviations cannot be detected?
{: #edit-resource-faq}
{: faq}
{: support}

A Terraform execution plan is based on the Terraform state file that was created when you ran your first {{site.data.keyword.bpshort}} apply action. Resources that you provisioned in other {{site.data.keyword.bpshort}} workspaces by using automation tools such as Ansible or Chef, or that you added without {{site.data.keyword.bpshort}} are not considered and not included in the Terraform execution plan.

## How should I remove resources with {{site.data.keyword.bplong_notm}}?
{: #remove-resource-faq}
{: faq}
{: support}

You can use the {{site.data.keyword.bplong_notm}} console or command-line to remove all the resources that you provisioned with {{site.data.keyword.bpshort}}. To stay in sync with your Terraform template, make sure to also remove the associated infrastructure code from your Terraform template so that your resources are not re-added when you apply a new version of your Terraform template. 

## What happens if I choose to delete my resource directly from the resource dashboard?
{: #delete-resource-directly-faq}
{: faq}
{: support}

When you manually remove a resource that you provisioned with {{site.data.keyword.bplong_notm}}, the state file is not updated automatically and becomes out of sync. When you create your next Terraform execution plan or apply a new template version, {{site.data.keyword.bpshort}} verifies that the {{site.data.keyword.cloud_notm}} resources in the state file exist in your {{site.data.keyword.cloud_notm}} account with the state that is captured. If the resource is not found, the state file is updated and the Terraform execution plan is changed accordingly. 

Although the state file is updated before new changes to your {{site.data.keyword.cloud_notm}} resources are applied, do not manually remove resources from the resource dashboard to avoid unexpected results. Instead, use the {{site.data.keyword.bplong_notm}} console or command-line to remove your resources, or remove the associated infrastructure code from your Terraform template. 
{: important}

## Are my resources removed when I remove the workspace?
{: #delete-resource-wks-faq}
{: faq}
{: support}

Removing the workspace from {{site.data.keyword.bplong_notm}} does not remove any of your {{site.data.keyword.cloud_notm}} resources. If you remove the workspace before you removed your resources, you must manually remove all your {{site.data.keyword.cloud_notm}} resources from the individual resource dashboard.

Removing an {{site.data.keyword.cloud_notm}} resource cannot be undone. Make sure that you backed up your data before you remove a resource. If you choose to remove the infrastructure code, or comment out the resource in your Terraform configuration file, make sure to thoroughly review the log file of your execution plan to verify that all your resources are included in the removal.    
{: important}

## Does {{site.data.keyword.bpshort}} supports `ibmcloud terraform` command?
{: #ibmcloud-terraform-cmd-faq}
{: faq}
{: support}

Using `ibmcloud terraform` command from CLI release v1.8.0 displays a warning message as **Alias 'terraform' will be deprecated. Please use 'schematics' or 'sch' in your commands.**. For more information, about CLI version history, see [CLI version history](/docs/schematics?topic=schematics-cli_version-releases).

## Can I access private network through {{site.data.keyword.bpshort}}?
{: #private-endpoint-faq}
{: faq}
{: support}

Yes, from [CLI release v1.8.0](/docs/schematics?topic=schematics-cli_version-releases) {{site.data.keyowrd.bpshort}} supports private {{site.data.keyword.bpshort}} endpoint to access your private network. For more information, see [private {{site.data.keyword.bpshort}} endpoint](/docs/schematics?topic=schematics-private-endpoints#private-cse).

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

## How can I resolve the error message while connecting to Bastion host IP addresses through {{site.data.keyword.bplong_notm}}?
{: #bastion-ipaddress-faq}
{: faq}
{: support}

**Error:**

```text
timeout - last error: Error connecting to bastion: dial tcp
 2022/03/02 03:59:37 Terraform apply | 52.118.101.204:22: connect: connection timed out
 2022/03/02 03:59:37 Terraform apply | 
 2022/03/02 03:59:37 Terraform apply | Error: file provisioner error
```
{: screen}

You can access your {{site.data.keyword.bpshort}} workspace and connect to Bastion host IP addresses for your region or zone based on the private or public endpoint IP addresses that are listed in the {{site.data.keyword.bpshort}} documentation. For more information, about the region and the zone based private and public IP addresses, see [Opening required IP addresses for the {{site.data.keyword.bplong_notm}} in your firewall](/docs/schematics?topic=schematics-allowed-ipaddresses).

## How do I create a cluster by using Terraform on {{site.data.keyword.cloud_notm}} environment?
{: #newcluster-workspace-faq}
{: faq}
{: support}

You can refer to create [single and multizone {{site.data.keyword.openshiftshort}} and {{site.data.keyword.containershort_notm}} cluster](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-tutorial-tf-clusters#create-cluster) tutorial.
