---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-28"

keywords: schematics faqs, what is terraform, infrastructure as code, iac, schematics price, schematics pricing, schematics cost, schematics charges, schematics personal information, schematics pii, delete pii from schematics, schematics compliance

subcollection: schematics

---
{:new_window: target="_blank"} 
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:faq: data-hd-content-type='faq'}
{:external: target="_blank" .external}
{:support: data-reuse='support'}


# FAQs
{: #faqs}

Answers to common questions about the {{site.data.keyword.bplong_notm}} service.
{:shortdesc}


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

{{site.data.keyword.bplong_notm}} workspaces are provided to you at no cost. However, when you decide to apply your Terraform template in {{site.data.keyword.cloud_notm}} by clicking **Apply plan** from the workspace details page or running the `ibmcloud terraform apply` command, you are charged for the {{site.data.keyword.cloud_notm}} resources that are described in your Terraform template. Review available service plans and pricing information for each resource that you are about to create. Some services come with a limit per {{site.data.keyword.cloud_notm}} account. If you are about to reach the service limit for your account, the resource is not provisioned until you increase the service quota, or remove existing services first.

## Can I run Ansible playbooks with {{site.data.keyword.bpshort}}?
{: #ansible-playbooks}
{: faq}
{: support}

With {{site.data.keyword.bplong_notm}}, you can run Ansible playbooks or {{site.data.keyword.bpshort}} actions against your {{site.data.keyword.cloud_notm}} by using the Ansible provisioner in your Terraform configuration file. For example, use the Ansible provisioner to deploy software on {{site.data.keyword.cloud_notm}} resources or perform actions against your resources, such as shutting down a virtual server instance. For more information about how to use the Ansible provisioner, see the following blogs:

- [Discover best-practice VPC configuration for application deployment](https://developer.ibm.com/articles/secure-vpc-access-with-a-bastion-host-and-terraform/){: external}
- [Learn about repeatable and reliable end-to-end app provisioning and configuration](https://developer.ibm.com/articles/application-deployment-with-redhat-ansible-and-ibm-cloud-schematics/){: external}

## Does {{site.data.keyword.bpfull_notm}} support multiple Terraform provider versions?
{: #provider-versions}
{: faq}
{: support}

Yes, {{site.data.keyword.bpfull_notm}} supports multiple Terraform provider versions. You need to add Terraform provider block with the right provider version. By default the provider executes latest version `1.21.0`, and previous four versions such as `1.20.1`, `1.20.0`, `1.19.0`, `1.18.0` are supported.


Example for a multiple provider configuration:

```
terraform{
  required_providers{
   ibm = ">= 1.21.0" // Error !! version unavailable.
   ibm = ">= 1.20.0" // Execute against latest version.
   ibm = "== 1.20.1" // Executes version v1.20.1. 
  }
}

```
{: pre}

Currently, version 1.21.0 is released. For more information, about provider version, refer to [provider version](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-setup_cli#install_provider).
{: note}

## When are new Terraform and Ansible versions added to {{site.data.keyword.bpshort}}?
{: #new-versions}
{: faq}
{: support}

After new Terraform and Ansible versions are released by the community, the IBM team begins a process of hardening and testing the release for {{site.data.keyword.bpshort}}. Availability of new versions depend on the results of these tests, community updates, security patches, and technology changes between versions. Make sure that your Terraform templates and Ansible playbooks are compatible with one of the supported versions so that you can run them in {{site.data.keyword.bpshort}}. 

## How do I generate IAM access token, if client id `bx` is used?
{: #createworkspace-generate-tokens}
{: faq}
{: support}

To create IAM access token, use `export IBMCLOUD_API_KEY=<ibmcloud_api_key>` and execute `curl -X POST "https://iam.cloud.ibm.com/identity/token" -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=$IBMCLOUD_API_KEY" -u bx:bx`. For more information, about creating IAM access token and API Docs, see [IAM access token](https://cloud.ibm.com/apidocs/iam-identity-token-api#gettoken-password) and [Create API key](https://cloud.ibm.com/apidocs/iam-identity-token-api#create-api-key). <br> You can set the environment values  `export ACCESS_TOKEN=<access_token>`, and `export REFRESH_TOKEN=<refresh_token>`. 

## How do I overcome the authentication error when Schematics workspace is created by using API?
{: #createworkspace-authentication-error}
{: faq}
{: support}

You need to create the IAM access token for your {{site.data.keyword.cloud_notm}} Account. For more information, about creating IAM access token, see [Get token password](https://cloud.ibm.com/apidocs/iam-identity-token-api#gettoken-password){: external}. You can refer to the following sample error message and the solution for the authentication error.

The [IAM API](https://cloud.ibm.com/apidocs/iam-identity-token-api#gettoken-apikey){: external} documentation only shows how to create a 'default token'. You can use the `refresh token` to get a new IAM access token if that token is expired. When the default client (no basic authorization header) as described in this documentation, this refresh_token cannot be used to retrieve a new IAM access token. When the IAM access token is about to be expired, use the API key to create a new access token.

**Error message**

  ```
  Error: Request failes with status code: 400, BXNIMO137E: For the original authentication, client id 'default' was passed, refresh the token, client id 'bx' is used.
  ```
**Solution**
1. You need to create access_token and refresh_token.

   ```
   export IBMCLOUD_API_KEY=<ibmcloud-api_key>
   curl -X POST "https://iam.cloud.ibm.com/identity/token" -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=$IBMCLOUD_API_KEY" -u bx:bx
   ```
2. Export the access_token and refresh_token obtained in step 1 as environment variables for ACCESS_TOKEN and REFRESH_TOKEN.

   ```
   export ACCESS_TOKEN=<access_token>
   export REFRESH_TOKEN=<refresh_token>
   ```
3. Create workspace

   ```
   curl --request POST --url https://cloud.ibm.com/schematics/overview/v1/workspaces -H "Authorization: Bearer <access_token>" -d '{"name":"","type": ["terraform_v0.12"],"description": "","resource_group": "","tags": [],"template_repo": {"url": ""},"template_data": [{"folder": ".","type": "terraform_v0.12","variablestore": [{"name": "variable_name1","value": "variable_value1"},{"name": "variable_name2","value": "variable_value2"}]}]}'
   ```
   
## Template Error
{: #template-error}

How do I rectify 'Failed to clone git repository, couldn’t find remote ref “refs/heads/master” (most likely invalid branch name is passed)'?

Usage of the branch `https://github.com/guruprasad0110/tf_cloudless_sleepy_13/ ` repository, after 1st October 2020, can see this error message. 

Solution
If the repository is created after 1st October 2020, the main branch syntax needs to be `https://github.com/username/reponame/tree/main`. For example, `https://github.com/guruprasad0110/tf_cloudless_sleepy_13/tree/main`


## Can I increase the timeout for null-exec and remote-exec resource?
{: #timeout-null-resource}
{: faq}
{: support}

No, the null-exec (null_resources) and remote-exec resources has maximum timeout of 60 minutes. Longer jobs need to be broken into shorter blocks to provision the infrastructure faster. Otherwise the execution times out automatically after 60 minutes.

## How does {{site.data.keyword.bpshort}} decide to remove the files from my Terraform or Ansible templates?
{: #clone-file-extension}
{: faq}
{: support}

**Issue**

While creating the {{site.data.keyword.bpshort}} workspace or action you notice the {{site.data.keyword.bpshort}} is marking some files as vulnerable, and is removing the file from the template.

**Solution**

While creating {{site.data.keyword.bpshort}} workspace or action {{site.data.keyword.bplong_notm}} takes a copy of the Terraform or Ansible template from your Git repository and stores in a secured location. Before the template files is saved, {{site.data.keyword.bpshort}} analyses the files and are removed, based on the following conditions:

- The allowed file extension are `.tf, .tfvars, .md, .yaml ,.sh, .txt, .yml, .html, .gitignore, .tf.json, license, .js, .pub, .service, _rsa, .py, .json, .tpl, .cfg, .ps1, .j2, .zip, .conf, .crt,.key, .der, .jacl, .properties, .cer, .pem`.
- The allowed image extension are `.tif .tiff .gif .png .bmp .jpg .jpeg`.
- The files that are removed are `.tfstate, .tfstate.backup, .exe, .php5, .pht, .phtml, .shtml, .asa, .asax, .swf, .xap`.
- All files that are larger than 500 KB in size are removed. This file size limit does not apply for the allowed image files.

The allowed extension list is continuously monitored and updated in every release. You can raise an [support ticket](/docs/schematics?topic=schematics-schematics-help) with the justification to add a new file extension to the list.
{: note}

## How can you save user-defined files generated by the Terraform modules and use them across multiple Terraform plan, apply, destroy, refresh, or import commands?
{: #persist-file}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} already persists and securely manages the state file generated by the Terraform engine in a {{site.data.keyword.bpshort}} workspace. {{site.data.keyword.bpshort}} periodically saves the state file in the secured location. Further the state file is automatically restored before running the {{site.data.keyword.bpshort}} job or Terraform plan, apply, destroy, refresh, or import commands.

In the same way {{site.data.keyword.bplong_notm}} supports the ability to persist user-defined files, that are generated by the Terraform template or modules. {{site.data.keyword.bpshort}} expects the user-defined Terraform template or modules to generate and place the files in a pre-defined location. {{site.data.keyword.bpshort}} will automatically save and restore them, before and after running the {{site.data.keyword.bpshort}} jobs or Terraform command.

Your files must be placed in the `/tmp/.schematics` folder and the size limit is set to `10 MB`. {{site.data.keyword.bpshort}} backups and restores all the files in the `/tmp/.schematics` folder.

## How to upgrade the Terraform versions in {{site.data.keyword.bpshort}}?
{: #migrate-terraform-v11}
{: faq}
{: support}

You can follow these steps to upgrade Terraform v0.11 to Terraform higher version in {{site.data.keyword.bpshort}}.
- Export the Terraform state file, from the {{site.data.keyword.bpshort}} workspace by using the [ibmcloud schematics state pull](/docs/schematics?topic=schematics-schematics-cli-reference#state-pull) command.
- Follow the steps described by [Hashicorp](https://www.terraform.io/upgrade-guides/index.html){: external} to upgrade from `Terraform v0.11 to v0.12`, `Terraform v0.12 to v0.13`, or higher. Upgrade your Terraform configuration `.tf` file and Terraform state file as per the latest Terraform version requirement. **Note** Use your own machine or laptop to perform these operations.
- Upload the upgraded Terraform configuration `.tf ` file, to an existing or a new Git repository.
- Create the {{site.data.keyword.bpshort}} workspace with the upgraded Terraform configuration `.tf` file in the Git repository and the upgraded state file by using the [ibmcloud schematics workspace new](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) command.
- Run the {{site.data.keyword.bpshort}} workspace [refresh](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-refresh) and [plan](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) commands, to verify the newly created workspace is able to connect and work with the existing {{site.data.keyword.cloud_notm}} resources.
- Delete the old {{site.data.keyword.bpshort}} workspace without destroying the {{site.data.keyword.cloud_notm}} resources.

You need to be an expert user to upgrade the Terraform version to perform these steps.
{: note}




