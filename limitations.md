---

copyright:
  years: 2017, 2020
lastupdated: "2020-12-09"

keywords: schematics limitations, schematics variables.tf, schematics local variables file, schematics local variable, schematics output.tf, schematics terraform.tfstate

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

# Limitations
{: #schematics-limitations}

Review the following limitations for {{site.data.keyword.bplong_notm}}.
{: shortdesc}

## Differences to native Terraform
{: #terraform-vs-schematics}

If you used native Terraform before and plan to migrate your Terraform templates to {{site.data.keyword.bplong_notm}}, make sure that you understand the differences between the services to adjust your template.  
{: shortdesc}

### Do I need to provide an {{site.data.keyword.cloud_notm}} API key in the `provider` block?
{: #provider-block}

The {{site.data.keyword.cloud_notm}} API key is essential to authenticate with the {{site.data.keyword.cloud_notm}} platform, receive the IAM token and IAM refresh token that {{site.data.keyword.bpshort}} requires to work with the resource's API, and to determine the permissions that you were granted. When you use native Terraform, you must provide the {{site.data.keyword.cloud_notm}} API key at all times. In {{site.data.keyword.bpshort}}, the API key is automatically retrieved for all IAM-enabled resources, including {{site.data.keyword.containerlong_notm}}, and VPC infrastructure resources. However, the API key is not retrieved for Cloud Foundry, Cloud Functions, and classic infrastructure resources and must be provided in the `provider` block.
{: shortdesc}

For more information about how to configure the `provider` block, see [Configuring the `provider` block](/docs/schematics?topic=schematics-create-tf-config#configure-provider). 

### Can I use my local `terraform.tfvars` file?
{: #terraformtfvars}

The `terraform.tfvars` file is a local variables file that you use to store sensitive information, such as your {{site.data.keyword.cloud_notm}} API key or classic infrastructure user name when you use native Terraform. This file must be present on your local machine so that Terraform can load the values for your credentials when you initialize the Terraform CLI. 
{: shortdesc}

With {{site.data.keyword.bplong_notm}}, you do not use a local `terraform.tfvars` file. Instead, you [declare your variables](/docs/schematics?topic=schematics-create-tf-config#configure-variables) in the Terraform configuration files, and enter the values for your variables when you create a workspace. You can later change the values of your variables by updating the variables from your workspace details page. 

Cloning GitHub repository in {{site.data.keyword.bplong_notm}} is allowed only to the listed extension files. The blocked extension files having more than 500 KB in size, and any invalid image is considered as vulnerable files while cloning.
  -	Allowed extension: `.tf` `.tfvars` `.md` `.yaml` `.sh` `.txt` `.yml` `.html` `.tf` `.json` `.gitignore` `license` `.js` `.pub` `.service` `_rsa`
  -	Blocked extension: `.php5` `.pht` `.phtml` `.shtml` `.asa` `.cer` `.asax` `.swf` `.xap` `.tfstate` `.tfstate.backup`
  -	Allowed image extension: `.tif` `.tiff` `.gif` `.png` `.bmp` `.jpg` `.jpeg` 

### Is Terraform remote state supported in {{site.data.keyword.bpshort}}?
{: #tf-remote-state}

You can access workspace state information from other workspaces by using the {{site.data.keyword.bpshort}} `ibm_schematics_output` data source that works similar to the `remote_state` data source in native Terraform. When you use the `remote_state` Terraform data source, you must configure a Terraform remote backend to connect to your Terraform workspaces. With the `ibm_schematics_output` data source, you automatically have access to the built-in {{site.data.keyword.bpshort}} backend and can access workspace information directly.

For more information about how to use this data source, see [Managing cross-workspace state access with Terraform](/docs/schematics?topic=schematics-remote-state). 

### Why is my local-exec and remote-exec provisioner in {{site.data.keyword.bplong_notm}} fails?
{: #local-remote-exec}

The time out is set for `local-exec` and `remote-exec` users by using {{site.data.keyword.bplong_notm}} workspace. You need to ensure the execution completes within 30 minutes. Otherwise, execution times out automatically and the apply state will fail. 

## What is the use of refresh token header?
{: #refresh-token}

If the `destroyresource` flag is set to `true`, refresh token header configuration is required to delete all the IBM Cloud resources, and the Schematics workspace. Following are the uses of refresh token header:
- If the token is expired, you can use `refresh token` to get a new IAM access token, refer[IAM access token](/docs/schematics?topic=schematics-faqs#createworkspace-generate-tokens). 
- The `refresh_token` parameter cannot be used to retrieve a new IAM access token. 
- When the IAM access token is about to expire, use the [API key](https://cloud.ibm.com/apidocs/iam-identity-token-api#create-api-key){: external} to create a new access token.

## Beta limitations
{: #beta-limitations}

During the Beta release of {{site.data.keyword.bplong_notm}} actions, be aware of the following limitations:
* This service might not yet be stable and is not intended for production use. 
* Any projects, apps, or jobs that you create during beta will not carry over to other releases'.

