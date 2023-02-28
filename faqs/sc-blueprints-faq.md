---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-28"

keywords: schematics faqs, infrastructure as code, iac, schematics blueprints faq, blueprints faq, 

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Blueprints
{: #blueprints-faq}

Answers to common questions about working with blueprints are classified under the following sections.
{: shortdesc}

## What are the Git repositories that are supported by Blueprints?
{: #faqs-bp-repos}
{: faq}
{: support}

|  Repository </br>  | Template </br> Public repo | Template </br>Private repo | Module </br>Public repo | Module </br>private repo | Comment </br>  |
| --- |--- | --- | --- | --- | --- |
| GitHub | Yes | Git token | Yes | Git token | 
| GitLab | Yes | Git token | Yes | Git token | 
| IBM GitLab | Yes | Git token | Yes | Git token | IAM token planned Dec 2022 
| Terraform.io | No | No | Yes | NA |
{: caption="Supported Git repositories" caption-side="top"}}

## Are variable operators and functions supported in blueprint templates?
{: #faqs-bp-values}
{: faq}
{: support}

The 1.0.0 release of the Blueprints schema supports a minimal set of variable operators. It is intentionally simple. The values passed between inputs and outputs are passed as is. Functions and variable operators are not supported in this release. 

A future release intends to implement functions and operators. 

## How are blueprint module dependencies and execution order determined?
{: #faqs-bp-dependencies}
{: faq}
{: support}

Dependencies between blueprint modules are created using the value references between module inputs and outputs. Similar to Terraform, a DAG (Directed Acyclic Graph) is generated from the dependencies and relationships to determine execution order. Dependencies are created using '$module' references. 

## How do I edit and validate blueprint templates?
{: #faqs-bp-editing}
{: faq}
{: support}

Blueprint templates can be edited in any editor or IDE. Follow the instructions on how to use and configure VSCode to [edit templates and input files](/docs/schematics?topic=schematics-edit-blueprints). The [Red Hat YAML VSCode extension](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml){: external}  provides a framework for editing blueprint YAML files, using a [blueprint schema](https://github.com/Cloud-Schematics/vscode-blueprint-schema){: external} defined using [JSON-Schema](https://json-schema.org){: external} . 

## Why do blueprints get the error 'Length for variable `variable name` greater than the given length'?  
{: #faqs-bp-length}
{: faq}
{: support}

The default size of values that can be passed to Blueprints as inputs is set to 1KB. If the expected size is greater than 1KB, the  [`max_length` inputs meta-data](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-inputs-max-len) can be defined to set the length that Blueprints should accept.  

 A value larger than 1KB or the specified length will result in the error `Length for variable <variable name> greater than the given length`   


## Why do blueprint operations require a `blueprint ID`?
{: #faqs-bp-install}
{: faq}
{: support}

In {{site.data.keyword.bpshort}} Blueprints, the displayed name is not a unique identifier. Only the blueprint ID that is generated at create time is unique. This unique blueprint ID is needed by the command line to identify the specific environment to be worked on. Blueprint IDs are unique globally and generated from the user supplied name at create time.  

## What URL format is used for referencing blueprint templates and input files?
{: #faqs-bp-url}
{: faq}
{: support}

In the Blueprints UI, the URL specification for the template and input files uses the Git object path syntax as used by the GitHub API and UI to access and retrieve files. This specifies both the repository URL and file name as a single string. 

All information to reference the file in the Git repo, its branch and sub-folder must be specified as part of the URL string. This includes the template or input file name. For example, `https://github.com/Cloud-Schematics/blueprint-basic-example/blob/main/basic-blueprint.yaml`. 

The link can point to the template file in the main branch, any other branch, or a subdirectory. The URL must include the template file name and **must use** the `blob/branch/` pattern for the full path. 
    
- Example for **main blueprint.yaml** - `https://github.com/myorg/myrepo/blob/main/blueprint.yaml`     
- Example for **blueprint.yaml in branches** - `https://github.com/myorg/myrepo/blob/mybranch/blueprint.yaml`
- Example for **blueprint.yaml in subdirectory** - `https://github.com/mnorg/myrepo/blob/mybranch/mysubdirectory/blueprint.yaml` 

The required URLs to the files can be copied directly from the Github or Gitlab UIs. For example with Github, on the `Code` tab hover over the template or input file you require the URL for. Right click with your mouse to bring up the context menu and select `Copy Link`, or `Copy Link Address`. The copied URL link can be pasted into the blueprint URL entry field. 

## How is resource provisioning performed?
{: #faqs-bp-resource}
{: faq}
{: support}

Cloud resource deployment with {{site.data.keyword.bpshort}} Blueprints is a two-step process, user driven process. Config create, Plan and Apply. Refer to [deploying blueprints](/docs/schematics?topic=schematics-deploy-blueprints) for an overview of the deployment and resource provisioning process.  

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

Sensitive input variables like API Keys or SSH Keys should not be saved in blueprint input files due to the risk of security exposure from a Git repository. To avoid accidental exposure, pass sensitive values as dynamic inputs at blueprint create time. 

- Through the UI as dynamic inputs

   In the UI enter sensitive values as override inputs on the inputs definition page.  

- Through the CLI passed as environment variables
   
   Dynamic inputs can be specified via the CLI using `--inputs` flag to pass string values. Sensitive values can be exported as environment variables and shell variable substitution is used to insert the variable. The example here shows the env-var `user_ssh_key` is exported with the value `ssh xxx`. Shell substitution is used to insert this value into the `blueprint create` command by using `--inputs sshkey=$user_ssh_key`

    ```sh
    export user_ssh_key="ssh xxx"
    ibmcloud schematics blueprint create  ......................  --inputs sshkey=$user_ssh_key
    ```
    {: pre}

- Through the CLI using an input file

   Dynamic inputs can be passed via the CLI using `-input-file` flag to pass values stored in a local YAML file. Refer to the [create CLI documentation](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) for more details.  

## Why does the blueprint template, basic example, fail in the apply step?
{: #faqs-bp-basic-example}
{: faq}
{: support}

The [blueprints-basic-example](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) as used in the [tutorial](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) demonstrates the principles of deploying cloud resources by using two modules, which reference and create cloud resources. If the user has insufficient IAM access permissions to these resources, the Terraform Apply operations can fail, resulting in an Apply failure.  

This example assumes, the user has IAM access permissions to create resources in the default resource group and also [Cloud Object Storage](/docs/cloud-object-storage?topic=cloud-object-storage-iam) instances. Additionally the user must also have permissions to create {{site.data.keyword.bpshort}} [workspaces and blueprints](/docs/schematics?topic=schematics-access). 


- Creating the Cloud Object Storage instance:
    - You can have one instance of a `Lite plan` per service. To create a new instance, either delete your existing `Lite plan` COS instance or select a paid plan. 
- Creating the Cloud Object Storage instance, the following error can occur if the user does not have the correct IAM permissions: 
    - `AccessDenied: Access Denied 	status code: 403`.


## What is the time taken to create an IBM Kubernetes Service cluster and other resources?
{: #faqs-bp-time}
{: faq}
{: support}

{{site.data.keyword.bpshort}} Blueprint utilizes IaC automation code modules that are written in Terraform HCL. The time taken to create resources is dependent on the resource type. Terraform automation modules are written not to return on initial resource creation, but to return only when the resource is fully initialized and available to be used by subsequent automation modules. This is different compared to the experience of creating resources through the UI. In the UI control is returned immediately and initialization continues in the background. With automation modules the perception is that they can take significantly longer to run. 

## How do you configure the version of Terraform to used?
{: #faqs-bp-tf-version}
{: faq}
{: support}

The version of Terraform used for an environment is user determined by the blueprint template `TF_VERSION` parameter. This parameter can be  used to control when the Terraform version for environment is updated to the next release or version.  

The Terraform version in use and allowable for modules is constrained by the value of `required_ version` in the Terraform module code.  

During Apply operations Terraform programmatically determines the Terraform version that is supported by the automation modules, looking for a `terraform` block with a [`required_version`](https://developer.hashicorp.com/terraform/language/settings#specifying-a-required-terraform-version){: external} parameter. If the module `required_version` constraint does not support the desired template version, the operation will be failed. 

## Is it possible to specify the CLI parameters as a file?
{: #faqs-bp-cli-file}
{: faq}
{: support}

A number of ways to create a blueprint configuration using the CLI.

For {{site.data.keyword.bpshort}} Blueprints, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.12.5`.
{: important}

1. Using a local file in JSON format.

   Example

   ```sh
   ibmcloud schematics blueprint create --file payload_complex.json
   ```
   {: pre}

2. Using command-line arguments.

   Example

   ```sh
   ibmcloud schematics blueprint create --name bp-useast-basic1 --resource-group Default --bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example --bp-git-file basic-blueprint.yaml --bp-git-branch main --input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example --input-git-file basic-input.yaml --input-git-branch main --inputs provision_rg=true,resource_group_name=bp-useast-rg1,cos_instance_name=bp-useast-cos1
   ```
   {: pre}

   The `--inputs provision_rg=true,resource_group_name=bp-useast-rg1,cos_instance_name=bp-useast-cos1` overrides the values from GitHub config file.

## When you create a blueprint config in the `us-south` target region, why is the blueprint job ID indicating `us-east`?
{: #faqs-bp-target-region}
{: faq}
{: support}

The CLI uses geo specific API endpoints which direct job requests to the first available region within a geo. `us.schematics.cloud.ibm.com` is called irrespective of the target `us-south` or `us-east` region and similarly, `eu.schematics.cloud.ibm.com` is called irrespective of the target `eu-gb` or `eu-de` region. {{site.data.keyword.bpshort}} dynamically determines which region to send the request based on region availability. Config's targeted to `us-south` during creation, will be automatically run on `us-east` if `us-south` is not available. 

This behavior is similar in the UI. For example, in the {{site.data.keyword.bpshort}} Workspace creation page, you select `North America` region from the list.

## Is it possible to delete the {{site.data.keyword.bpshort}} service instance by using the Resource Controller API or CLI?
{: #faqs-bp-schematics-instance}
{: faq}
{: support}

You cannot delete the {{site.data.keyword.bpshort}} service instance. Instead you can `destroy` the cloud resources that are created by using {{site.data.keyword.bpshort}} Terraform and `delete` the {{site.data.keyword.bpshort}} objects such as workspace, or an action.
{: shortdesc}

{{site.data.keyword.bpshort}} service is provisioned for the following reasons.

- Whenever {{site.data.keyword.bpshort}} service is used for the first time, you do not explicitly provision the {{site.data.keyword.bpshort}}.
- {{site.data.keyword.bpshort}} is a free service, not charged for the service instance.

There is no plan to add this feature to delete the {{site.data.keyword.bpshort}} service instance.
{: note}
