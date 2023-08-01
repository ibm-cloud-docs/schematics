---

copyright:
  years: 2017, 2023
lastupdated: "2023-08-01"

keywords: schematics limitations, schematics variables.tf, schematics local variables file, schematics local variable, schematics output.tf, schematics terraform.tfstate, adoption, considerations

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Adoption considerations 
{: #schematics-limitations}

Review the following considerations when adopting {{site.data.keyword.bplong_notm}}. Additionally review the section on [workspace setup](/docs/schematics?topic=schematics-workspace-setup#configure-provider) for details of how to work with your Terraform configs stored in Git repositories.  
{: shortdesc}

## Differences to native Terraform
{: #terraform-vs-schematics}

If you used native Terraform before and plan to migrate your Terraform templates to {{site.data.keyword.bplong_notm}}, make sure that you understand the differences between standalone Terraform usage and use via {{site.data.keyword.bpshort}} to modify your templates. 
{: shortdesc}

### Do I need to provide an {{site.data.keyword.cloud_notm}} API key in the `provider` block?
{: #provider-block}

With {{site.data.keyword.bpshort}} it is not necessary to pass an API Key. 

If an API key is not defined in the `provider` block, {{site.data.keyword.bpshort}} passes the users IAM token for all IAM-enabled resources, including {{site.data.keyword.containerlong_notm}} clusters, and VPC infrastructure resources. However, the IAM token is not retrieved for {{site.data.keyword.ibmcf_notm}} and classic infrastructure resources and the API key must be provided in the `provider` block.

If an {{site.data.keyword.cloud}} API key passed, it is used to authenticate with the {{site.data.keyword.cloud_notm}} platform, create the IAM token and IAM refresh token that {{site.data.keyword.bpshort}} requires to work with the resource's API, and to determine the permissions that you have granted to perform the provisioning operation. 
{: shortdesc}

For more information about how to configure the `provider` block, see [Configuring the `provider` block](/docs/schematics?topic=schematics-create-tf-config#configure-provider). 

### Can I use my local `terraform.tfvars` file?
{: #terraformtfvars}

The `terraform.tfvars` file is a local variables file that you can use to store sensitive information, such as your {{site.data.keyword.cloud_notm}} API key or classic infrastructure user name when you use native Terraform. This file, or environment variables must be present on your local machine for Terraform to load the values for your credentials when you initialize the Terraform CLI. 
{: shortdesc}

With {{site.data.keyword.bplong_notm}}, you do not use a local `terraform.tfvars` file. Instead, you [declare your variables](/docs/schematics?topic=schematics-create-tf-config#configure-variables) in the Terraform configuration files, and enter the values for your variables when you create a workspace. You can later change the values of your variables by updating the variables from your workspace details page. 

### Is Terraform remote state supported?
{: #tf-remote-state}

{{site.data.keyword.bpshort}} includes implicit backend support and it is not required to define a remote backend. 

You can access workspace state information from other workspaces by using the {{site.data.keyword.bpshort}} `ibm_schematics_output` data source. This replaces the `remote_state` data source used by native Terraform in conjunction with remote backend support. It works in the same way allowing access to Terraform workspaces.  

With the [ibm_schematics_output](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/schematics_output){: external} data source, you automatically have access to the built-in {{site.data.keyword.bpshort}} backend and can access workspace information directly. See also the [ibm_schematics_state](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/schematics_state){: external} data source. 

For more information about how to use these data sources, see [Managing cross-workspace state access with Terraform](/docs/schematics?topic=schematics-remote-state). 

### Why do local-exec and remote-exec provisioner terminate after 30 minutes?
{: #local-remote-exec} 

The Terraform `local exec` and `remote exec` operations have a time limit of `30 minutes`. This is to ensure fair usage of the {{site.data.keyword.bpshort}} service for all users. If exceeded, the commands will be terminated and job execution fail. 

## What is the use of refresh token header?
{: #refresh-token}

If the `destroyresource` flag is set to `true`, refresh token header configuration is required to delete all the {{site.data.keyword.cloud_notm}} resources, and the {{site.data.keyword.bpshort}} Workspace. Following are the uses of refresh token header:
- If the token is expired, you can use `refresh token` to get a new IAM access token, see [IAM access token](/docs/schematics?topic=schematics-general-faq#createworkspace-generate-tokens). 
- The `refresh_token` parameter cannot be used to retrieve a new IAM access token. 
- When the IAM access token is about to expire, use the [API key](/apidocs/iam-identity-token-api#create-api-key){: external} to create a new access token.

## Git repo restrictions
{: #git-restrictions}

Branch names containing `/` (backslash) are not supported




