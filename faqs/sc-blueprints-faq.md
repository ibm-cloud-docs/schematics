---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-27"

keywords: schematics faqs, infrastructure as code, iac, schematics blueprints faq, blueprints faq, 

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Blueprints
{: #blueprints-faq}

Answers to common questions about using {{site.data.keyword.bpshort}} blueprints are classified under the following sections.
{: shortdesc}

## Why do blueprint operations require a `blueprint ID`?
{: #faqs-bp-install}
{: faq}
{: support}

In {{site.data.keyword.bpshort}} blueprints, the displayed name is not a unique identifier. Only the blueprint ID that is generated at create time is unique. This unique blueprint ID is needed by the command line to identify the specific environment to be worked on. Blueprint IDs are unique globally and generated from the user supplied name at create time.  

## How is resource provisioning performed?
{: #faqs-bp-resource}
{: faq}
{: support}

Cloud resource deployment with {{site.data.keyword.bpshort}} blueprints is a two-step process, user driven process. Refer to [deploying blueprints](/docs/schematics?topic=schematics-deploy-blueprints) for an overview of the deployment and resource provisioning process.  

## How do you view the blueprint provisioned resources in your cloud account?
{: #faqs-bp-results}
{: faq}
{: support}

A cloud resource that are created by a blueprint can be viewed on the blueprint `Resources` tab in the UI.
{: shortdesc}

All cloud resources created by a blueprint configuration are automatically tagged with the unique blueprint ID of the environment. The Cloud Resource List can be filtered by tag to identify all cloud resources belonging to the environment. 

## How do you securely pass input variables?
{: #faqs-bp-secure-inputs}
{: faq}
{: support}

Sensitive input variables like API Keys or SSH Keys must not be saved in blueprint input files due to the risk of security exposure from a Git repository. To avoid accidental exposure, sensitive inputs must be passed as dynamic inputs at blueprint config create time by using the CLI `--inputs` flag. 

Sensitive values can be exported as environment variables and shell variable substitution that is used to insert the variable. The example here shows the env-var `user_ssh_key` is exported with the value `ssh xxx`. Shell substitution is used to insert this value into the `blueprint config create` command by using `--inputs sshkey=$user_ssh_key`

```sh
export user_ssh_key="ssh xxx"
ibmcloud schematics blueprint config create  ......................   --inputs sshkey=$user_ssh_key
```
{: pre}

## Why does the blueprint template, basic example, fail in the run apply step?
{: #faqs-bp-basic-example}
{: faq}
{: support}

The [blueprints-basic-example](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) as used in the [tutorial](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) demonstrates the principles of deploying cloud resources by using two modules, which reference and create cloud resources. If the user has insufficient IAM access permissions to these resources, the Terraform Apply operations can fail, resulting in an Apply failure.   

This example assumes, the user has access to a Pay-Go or Subscription account and has IAM access permissions to create resources in the Default resource group and also [Cloud Object Storage](/docs/cloud-object-storage?topic=cloud-object-storage-iam) instances. Additionally the user must also have permissions to create {{site.data.keyword.bpshort}} [Workspaces and Blueprints](/docs/schematics?topic=schematics-access). 

The example uses the inputs `provisioning_rg=false` and `resource_group_name=Default` to reference the default resource group and provide this ID for the Cloud Object Storage instance creation. 

If alternative input parameters are used to create a new resource group with a user-defined name or `Trial/Lite` accounts are used, the following errors can be observed. 

- Trial accounts can have a single resource group. More resource groups cannot be created unless the account is upgraded to a paid account. In a trial account, a single `Default` resource group exists, and more resource groups are not allowed.
- In a shared paid account, the example can fail due to duplicate resource groups. If the sample blueprint template is deployed multiple times in the same account with the same documented input parameters from the readme file, the installation step fails. It occurs as the second and subsequent blueprints use the same resource group name as input. 
- Delete and re-create the blueprint by using a different `resource_group_name`.
    - In a shared user account, the resource group creation can fail due to insufficient IAM permissions to create resource groups. To create resource groups, a user needs [Account Management, editor or administrator permissions](/docs/account?topic=account-account-services#account-management-actions-roles). 
- Creating the Cloud Object Storage instance, the following errors can occur in `Lite` or `Trial` accounts:
    - This plan requires a paid account. You can upgrade by adding a credit card to your account or you can select the `free plan` if it's available.
    - You can have one instance of a `Lite plan` per service. To create a new instance, either delete your existing `Lite plan` instance or select a paid plan.
- Creating the Cloud Object Storage instance, the following error can occur if the user does not have the correct IAM permissions: 
    - `AccessDenied: Access Denied 	status code: 403`.


## What is the time taken to create an IBM Kubernetes Service cluster and other resources?
{: #faqs-bp-time}
{: faq}
{: support}

{{site.data.keyword.bpshort}} blueprint utilizes IaC automation code modules that are written in Terraform HCL. The time taken to create resources is dependent on the resource type. Terraform automation modules are written not to return on initial resource creation, but to return only when the resource is fully initialized and available to be used by subsequent automation modules. This is different compared to the experience of creating resources through the UI. In the UI control is returned immediately and initialization continues in the background. With automation modules the perception is that they can take significantly longer to run. 

## How do you configure the version of Terraform to used?
{: #faqs-bp-tf-version}
{: faq}
{: support}

The version of Terraform used for an environment is user determined by the blueprint template `TF_VERSION` parameter. This parameter can be  used to control when the Terraform version for environment is updated to the next release or version.   

The Terraform version in use and allowable for modules is constrained by the value of `required_ version` in the Terraform module code.   

During Apply operations Terraform programmatically determines the Terraform version that is supported by the automation modules, looking for a `terraform` block with a [`required_version`](https://developer.hashicorp.com/terraform/language/settings#specifying-a-required-terraform-version){: external} parameter. If the module `required_version` constraint does not support the desired template version, the operation will be failed. 

## Is it possible to specify the CLI parameters as a file?
{: #faqs-bp-location-override}
{: faq}
{: support}

There two ways to create a blueprint configuration using the CLI.

For {{site.data.keyword.bpshort}} blueprints, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.12.3`.
{: important}

1. Using a local file in JSON format.

   Example

   ```sh
   ibmcloud schematics blueprint config create --file payload_complex.json
   ```
   {: pre}

2. Using command-line arguments.

   Example

   ```sh
   ibmcloud schematics blueprint config create --name bp-useast-basic1 --resource-group Default --bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example --bp-git-file basic-blueprint.yaml --bp-git-branch main --input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example --input-git-file basic-input.yaml --input-git-branch main --inputs provision_rg=true,resource_group_name=bp-useast-rg1,cos_instance_name=bp-useast-cos1
   ```
   {: pre}

   The `--inputs provision_rg=true,resource_group_name=bp-useast-rg1,cos_instance_name=bp-useast-cos1` overrides the values from GitHub config file.

## When you create a blueprint config in the `us-south` target region, why is the blueprint job ID indicating `us-east`?
{: #faqs-bp-target-region}
{: faq}
{: support}

The CLI uses geo specific API endpoints which direct job requests to the first available region within a geo. `us.schematics.cloud.ibm.com` is called irrespective of the target `us-south` or `us-east` region and similarly, `eu.schematics.cloud.ibm.com` is called irrespective of the target `eu-gb` or `eu-de` region. {{site.data.keyword.bpshort}} dynamically determines which region to send the request based on region availability. Config's targeted to `us-south` during creation, will be automatically run on `us-east` if `us-south` is not available. 

This behavior is similar in UI, for example, in the {{site.data.keyword.bpshort}} workspace creation page, you select `North America` region from the list.

## Is it possible to delete the {{site.data.keyword.bpshort}} service instance by using the Resource Controller API or CLI?
{: #faqs-bp-schematics-instance}
{: faq}
{: support}

You cannot delete the {{site.data.keyword.bpshort}} service instance. Instead you can `destroy` the cloud resources that are created by using {{site.data.keyword.bpshort}} Terraform and `delete` the {{site.data.keyword.bpshort}} objects such as Workspace, or an Action.
{: shortdesc}

{{site.data.keyword.bpshort}} service is provisioned for the following reasons.

- Whenever {{site.data.keyword.bpshort}} service is used for the first time, you do not explicitly provision the {{site.data.keyword.bpshort}}.
- {{site.data.keyword.bpshort}} is a free service, not charged for the service instance.

There is no plan to add this feature to delete the {{site.data.keyword.bpshort}} service instance.
{: note}
