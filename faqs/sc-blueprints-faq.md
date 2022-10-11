---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-11"

keywords: schematics faqs, infrastructure as code, iac, schematics blueprints faq, blueprints faq, 

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the beta release.
{: beta}

# Blueprints
{: #blueprints-faq}

Answers to common questions about the {{site.data.keyword.bpshort}} Blueprints are classified into following sections.
{: shortdesc}

## Why do you set blueprint operations by using a `blueprint id`?
{: #faqs-bp-install}
{: faq}
{: support}

In Blueprints, the displayed name of the blueprint is not a unique identifier. Only the blueprint ID that is generated for each blueprint at create time is unique. This unique blueprint ID is needed by command line to identify the specific blueprint to be worked on. blueprint IDs are unique globally and generated from the user supplied blueprint name at create time.
{: shortdesc}

## How does resource provisioning happen by using the Blueprints?
{: #faqs-bp-resource}
{: faq}
{: support}

Cloud resource deployment with blueprints is a two-step process, user driven process. Refer to [Deploying blueprint environments](/docs/schematics?topic=schematics-deploy-blueprints) for an overview of the blueprints deployment and resource provisioning process.
{: shortdesc}

## How do you view the Blueprints provisioned results in your cloud account?
{: #faqs-bp-results}
{: faq}
{: support}

A cloud resource that are created by a blueprint can be viewed on the blueprint `Resources` tab in the UI.
{: shortdesc}

During the Beta, Blueprints support the application of IBM Cloud tags to all created cloud resources. All resources that are created by a blueprint are automatically tagged with the unique blueprint ID of the owning blueprint. With tagging the cloud resource list can be filtered by tag to identify all cloud resources created by a blueprint.

## How do you securely pass input variables to Blueprints?
{: #faqs-bp-secure-inputs}
{: faq}
{: support}

Sensitive input variables like API Keys or SSH Keys must not be saved in blueprint input files due to the risk of security exposure from a Git repository. To avoid accidental exposure, sensitive inputs must be passed as dynamic inputs at blueprint create time by using the `--inputs` flag.

Sensitive values can be exported as environment variables and shell variable substitution that is used to insert the variable. The example here shows the env-var `user_ssh_key` is exported with the value `ssh xxx`. Shell substitution is used to insert this value into the `blueprint create` command by using `--inputs sshkey=$user_ssh_key`

```sh
export user_ssh_key="ssh xxx"
ibmcloud schematics blueprint create  ......................   --inputs sshkey=$user_ssh_key
```
{: pre}

## Why does the Blueprints basic example fail in the Install step?
{: #faqs-bp-basic-example}
{: faq}
{: support}

The [blueprints-basic-example](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) as used in the Blueprints [tutorial](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) demonstrates the principles of deploying cloud resources by using two {{site.data.keyword.bpshort}} Workspaces, which reference and create cloud resources. If the user has insufficient IAM access permissions to this account, Terraform Workspace operations can fail, resulting in an Install failure.   

It is assumed. The user has access to a [{{site.data.keyword.cloud_notm}} Pay-As-You-Go or Subscription](https://cloud.ibm.com/registration){: external} account and has IAM access permissions to create resources in the default resource group and [Cloud Object Storage](/docs/cloud-object-storage?topic=cloud-object-storage-iam) instances. Also, permissions to create {{site.data.keyword.bpshort}} [Workspaces and Blueprints](/docs/schematics?topic=schematics-access). 

The example uses the inputs `provisiong_rg=false` and `resource_group_name=Default` to reference the default resource group and provide this ID for the Cloud Object Storage instance creation. 

If alternative input parameters are used to create a new resource group with a user-defined name or `Trial/Lite` accounts are used, the following errors can be observed. 

- Trial accounts can have a single resource group. More resource groups cannot be created unless the account is upgraded to a paid account. In a trial account, a single `Default` resource group and more resource groups are not allowed.
- In a shared paid account, the example can fail due to duplicate resource groups. If the sample blueprint is deployed multiple times in the same account with the same documented input parameters from the readme file, the installation step fails. It occurs as the second and subsequent Blueprints use the same resource group name as input. 
- Delete and re-create the blueprint by using a different `resource_group_name`.
    - In a shared user account, the resource group creation can fail due to insufficient IAM permissions to create resource groups. To create resource groups, a user needs [Account Management, editor or administrator permissions](/docs/account?topic=account-account-services#account-management-actions-roles). 
- Creating the Cloud Object Storage instance, the following errors can occur in `Lite` or `Trial` accounts:
    - This plan requires a paid account. You can upgrade by adding a credit card to your account or you can select the `free plan` if it's available.
    - You can have one instance of a `Lite plan` per service. To create a new instance, either delete your existing `Lite plan` instance or select a paid plan.
- Creating the Cloud Object Storage instance, the following error can occur if the user does not have the correct IAM permissions: 
    - `AccessDenied: Access Denied 	status code: 403`.

## What is the time taken to create an IBM Kubernetes Service cluster by using Blueprints?
{: #faqs-bp-time}
{: faq}
{: support}

Blueprint runs IaC automation code modules that are written in HashiCorp Terraform. It can take minutes to hours to run depending on the cloud resources. Automation modules are coded not to return on initial resource creation, but to return when the resource is fully initialized and available to be used by subsequent automation modules. Compared to the experience of creating resources through the UI, where control is returned immediately and initialization continues in the background, automation modules can take significantly longer to run. 
{: shortdesc}

## How do you tell Blueprints what version of Terraform to use?
{: #faqs-bp-tf-version}
{: faq}
{: support}

The version of Terraform used for blueprint Workspaces can be set by using the blueprint template as `TF_VERSION`. At execution time {{site.data.keyword.bpshort}} programmatically determine the Terraform version that is supported by the automation modules, looking for a `terraform` block with a [`required_version`](https://www.terraform.io/language/settings#specifying-a-required-terraform-version){: external} parameter. {{site.data.keyword.bpshort}} choose the most recent executable release that meets the constraints of both the `TF_VERSION` and `required_ version` from the Terraform config.
{: shortdesc}

## Is it possible to override the GitHub definition `location` and use a command-line file instead?
{: #faqs-bp-location-override}
{: faq}
{: support}

The two ways to create blueprint by using CLI.
{: shortdesc}

For {{site.data.keyword.bpshort}} Blueprints, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.11.0`.
{: important}

1. Using a local file in JSON format.

   Example

   ```sh
   ibmcloud sch bp create --file payload_complex.json
   ```
   {: pre}

2. Using command-line arguments.

   Example

   ```sh
   ibmcloud schematics blueprint create --name bp-useast-basic1 --resource-group Default --bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example --bp-git-file basic-blueprint.yaml --bp-git-branch main --input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example --input-git-file basic-input.yaml --input-git-branch main --inputs provision_rg=true,resource_group_name=bp-useast-rg1,cos_instance_name=bp-useast-cos1
   ```
   {: pre}

   The `--inputs provision_rg=true,resource_group_name=bp-useast-rg1,cos_instance_name=bp-useast-cos1` overrides the values from GitHub config file.

## When you create a blueprint in `us-south` target region, why is the job type in blueprint job ID indicates `us-east`?
{: #faqs-bp-target-region}
{: faq}
{: support}

The CLI uses general regional API endpoints behind the scenes, as in `us.schematics.cloud.ibm.com` is called irrespective of target `us-south` or `us-east` and similarly, `eu.schematics.cloud.ibm.com` is called irrespective of target `eu-gb` or `eu-de`. The {{site.data.keyword.cis_short}} load balancer decides which region to send the request. So in your case even though `us-south` is targeted during the time of creation the {{site.data.keyword.cis_short}} load balancer for some reason (such as `us-south` might be down at that point or having issues) sent the request to `us-east` and that is the region where blueprint got created.

This behavior is similar in UI, for example, in the {{site.data.keyword.bpshort}} Workspace creation page, you select `North America` region from the list.

## Is it possible to delete the {{site.data.keyword.bpshort}} service instance by using the Resource Controller API or CLI?
{: #faqs-bp-schematics-instance}
{: faq}
{: support}

You cannot delete the {{site.data.keyword.bpshort}} service instance. Instead you can `destory` the cloud resources that are created by using {{site.data.keyword.bpshort}} Terraform and `delete` the {{site.data.keyword.bpshort}} objects such as Workspace, or an Action.
{: shortdesc}

{{site.data.keyword.bpshort}} service is provisioned for the following reasons.

- Whenever {{site.data.keyword.bpshort}} service is used for the first time, you do not explicitly provision the {{site.data.keyword.bpshort}}.
- {{site.data.keyword.bpshort}} is a free service, not charged for the service instance.

There is no plan to add this feature to delete the {{site.data.keyword.bpshort}} service instance.
{: note}
