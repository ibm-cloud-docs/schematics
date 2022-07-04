---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-04"

keywords: schematics faqs, infrastructure as code, iac, schematics blueprints faq, blueprints faq, 

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Blueprints
{: #blueprints-faq}

Answers to common questions about the {{site.data.keyword.bpshort}} Blueprints are classified into following sections.
{: shortdesc}

## Why do I perform Blueprint operations using `Blueprint id`?
{: #faqs-bp-install}
{: faq}
{: support}

In Blueprints the displayed name of the Blueprint is not a unique identifier. Only the Blueprint ID that is generated for each Blueprint at create time is unique. This unique Blueprint ID is required by CLI commands to identify the specific Blueprint to be worked on. Blueprint ID's are unique globally and generated from the user supplied Blueprint name at create time.  

## How does resource provisioning happen when using the Blueprints?
{: #faqs-bp-resource}
{: faq}
{: support}

Resource configuration with Blueprints is a two step process, user driven process. Refer to [Blueprints lifecycle](https://test.cloud.ibm.com/docs/schematics?topic=schematics-blueprint-lifecycle-cmds) commands: create, update, and delete for an overview of the Blueprints lifecycle. 


In the first step the Blueprint configuration in {{site.data.keyword.bpshort}} is created or updated. This saves in {{site.data.keyword.bpshort}} the required releases of the Blueprint definition and Input files from Git, and any optional input values that will be used to create cloud resources. For each Blueprint module, Workspaces are initialised or settings updated as required based on the specified release of the Blueprint definition and Blueprint inputs. For more information, refer to [Creating Blueprint](/docs/schematics?topic=schematics-create-blueprint).

In the second step, the user performs the Install operation to deploy new or changed cloud resources. This runs the Infrastructure as code (IaC) automation code modules associated with the Workspaces by using the initial input configuration. For each module {{site.data.keyword.bpshort}} performs a Terraform apply or Ansible playbook run to create or configure the specified cloud resources. For more information, refer to [Installing Blueprint](/docs/schematics?topic=schematics-install-blueprint).

## How do I view the Blueprints provisioned results in my cloud account?
{: #faqs-bp-results}
{: faq}
{: support}

After installing a Blueprint, cloud resources created by a Blueprint can be viewed on the Blueprint `Resources` tab in the UI.  

During the beta, Blueprints will support the application of IBM Cloud tags to all created cloud resources. All resources created by a Blueprint will be automatically tagged with the unique Blueprint ID of the owning Blueprint. With tagging the Cloud Resource List can be filtered by tag to identify all cloud resources created by a Blueprint. 

## How do I securely pass input variables to Blueprints?
{: #faqs-bp-secure-inputs}
{: faq}
{: support}

Sensitive input variables like API Keys or SSH Keys should not be saved in Blueprint input files due to the risk of security exposure from a Git repository. To avoid accidential exposure, sensitive inputs should be passed as dynamic inputs at Blueprint create time using the `--inputs` flag. 

Senstive values can be exported as environment variables and shell variable substituion used to insert the variable. The example here shows the env-var `user_ssh_key` is exported with the value `ssh xxx`. Shell substitution is used to insert this value into the `blueprint create` command using `--inputs sshkey=$user_ssh_key`

```sh
export user_ssh_key="ssh xxx"
ibmcloud schematics blueprint create  ......................   --inputs sshkey=$user_ssh_key
```
{: pre}

## Why does the Blueprints basic example fail in the Install step?
{: #faqs-bp-basic-example}
{: faq}
{: support}

The [blueprints-basic-example](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) as used in the Blueprints [tutorial](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) demonstrates the principles of deploying cloud resources using two {{site.data.keyword.bpshort}} Workspaces which reference and create cloud resources. If the user has insufficient IAM access permissions to this account, Terraform Workspace operations can fail, resulting in an Install failure.   

It is assumed the user has access to a Pay-Go or Subscription account and has IAM access permisions to create resources in the Default resource group and [Cloud Object Storage](https://test.cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-iam) instances. Also permissions to create {{site.data.keyword.bpshort}} [Workspaces and Blueprints](https://test.cloud.ibm.com/docs/schematics?topic=schematics-access). 

The example uses the inputs `provisiong_rg=false` and `resource_group_name=Default` to reference the default resource group and provide this ID for the COS instance creation. 

If alternative input parameters are used to create a new resource group with a user defined name or `Trial/Lite` accounts are used, the following errors may be observed. 

- Trial accounts can only have a single resource group. Additional resource groups cannot be created unless the account is upgraded to a paid account. In a trial account there is already a single `Default` resource group and additional resource groups are not allowed.
- When using a shared paid account, the example can fail due to duplicate resource groups. If the sample Blueprint is deployed multiple times in the same account with the same documented input parameters from the Readme, the install step will fail. This occurs as the second and subsequent Blueprints use the same resource group name as input. 
- Delete and recreate the Blueprint using a different `resource_group_name`.
    - In a shared user account, the resource group creation can fail due to insufficient IAM permissions to create resource groups. To create resource groups, a user needs [Account Management, editor or administrator permissions](/docs/account?topic=account-account-services&interface=ui#account-management-actions-roles). 
- When creating the COS instance, the following errors may occur when using Lite or Trial accounts:
    - "This plan requires a paid account. You can upgrade by adding a credit card to your account or you can select the free plan if it's available."
    - "You can only have one instance of a Lite plan per service. To create a new instance, either delete your existing Lite plan instance or select a paid plan."
- When creating the COS instance, the following error may occur if the user does not have the correct IAM permissions: 
    - `AccessDenied: Access Denied 	status code: 403`.


## What is the time taken to create a IKS cluster using Blueprints?
{: #faqs-bp-time}
{: faq}
{: support}

Blueprints runs IaC automation code modules written in HashiCorp Terraform. These can take minutes to hours to execute depending on the cloud resources being created. Automation modules are coded not to return on initial resource creation, but to only return when the resource is fully initialized and available to be used by subsequent automation modules. Compared to the experience of creating resources via the UI, where control is returned immediately and initialisation continues in the background, automation modules can take significantly longer to execute. 

## How do I tell Blueprints what version of Terraform executable to use?
{: #faqs-bp-tf-version}
{: faq}
{: support}

The version of Terraform used for Blueprint Workspaces can be set using the Blueprint definition setting `TF_VERSION`. At execution time {{site.data.keyword.bpshort}} will programmatically determine the Terraform version supported by the automation modules, looking for a `terraform` block with a [required_version](https://www.terraform.io/language/settings#specifying-a-required-terraform-version){: external} parameter. {{site.data.keyword.bpshort}} will choose the most recent executable release that meets the constraints of the both the `TF_VERSION` and `required_ version` from the Terraform config.