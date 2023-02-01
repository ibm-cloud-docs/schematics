---

copyright:
  years: 2017, 2023
lastupdated: "2023-01-31"

keywords: terraform template guidelines, terraform config file guidelines, sample terraform files, terraform provider, terraform variables, terraform input variables, terraform template

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Creating Terraform templates 
{: #create-tf-config}

Learn how to create Terraform templates that are well-structured, reusable, and comprehensive.
{: shortdesc}

A Terraform template consists of one or more Terraform configuration files that declare the state that you want to achieve for your {{site.data.keyword.cloud}} resources. To successfully work with your resources, you must [configure IBM as your cloud provider](/docs/schematics?topic=schematics-create-tf-config#configure-provider) and [add resources to your Terraform configuration file](/docs/schematics?topic=schematics-create-tf-config#configure-resources). Optionally, you can use [input variables](/docs/schematics?topic=schematics-create-tf-config#configure-variables) to customize your resources.

You can write your Terraform configuration file by using HashiCorp Configuration Language (HCL) or JSON format. 

Before you start creating your Terraform template, make sure to review the [{{site.data.keyword.bplong_notm}} limitations](/docs/schematics?topic=schematics-schematics-limitations). 
{: tip}

## Configuring the `provider` block 
{: #configure-provider}
{: help}
{: support}

Specify the cloud provider that you want to use in the `provider` block of your Terraform configuration file. The `provider` block includes all the input variables that the {{site.data.keyword.cloud}} Provider plug-in for Terraform requires to provision your resources.
{: shortdesc}

**Do I need to provide the {{site.data.keyword.cloud_notm}} API key?**

The {{site.data.keyword.cloud_notm}} API key is essential to authenticate with the {{site.data.keyword.cloud_notm}} platform. Also the IAM token and IAM refresh token that {{site.data.keyword.bpshort}} requires to work with the resource's API, and to determine the permissions that you were granted. When you use native Terraform, always you must provide the {{site.data.keyword.cloud_notm}} API key. In {{site.data.keyword.bpshort}}, the IAM token is retrieved for all IAM-enabled resources, including {{site.data.keyword.containerlong_notm}} clusters, and VPC infrastructure resources. However, the IAM token is not retrieved for {{site.data.keyword.ibmcf_notm}} and classic infrastructure resources and the API key must be provided in the `provider` block. 

**Can I specify a different {{site.data.keyword.cloud_notm}} API key in the `provider` block?**

If you want to use a different API key than the one that is associated with your {{site.data.keyword.cloud_notm}} account, you can provide this API key in the `provider` block. If an API key is configured in the `provider` block, this key takes precedence over the API key that is stored in {{site.data.keyword.cloud_notm}}. 

**Can I provide an API key for a service ID?**

You can provide an API key for a service ID for all IAM-enabled services, including VPC infrastructure resources. You cannot use a service ID for classic infrastructure or {{site.data.keyword.ibmcf_notm}} resources. 

Follow the instructions to configure the `provider` block.

1. Choose how you want to configure the `provider` block. 
    - **Option 1 Create a separate `provider.tf` file.** The information in this file is loaded by Terraform and {{site.data.keyword.bplong_notm}}, and applied to all Terraform configuration files that exist in the same GitHub directory or tape archive file `.tar`. This approach is useful if you split out your infrastructure code across multiple files. 
    - **Option 2 Add a `provider` block to your Terraform configuration file.** You might choose this option if you prefer to, specify the provider alongside with your variables and resources in one Terraform configuration file. 

2. Review what [credentials and information that you must provide in the `provider` block to work with your resources](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-reference#required-parameters). {{site.data.keyword.bpshort}} automatically retrieves your {{site.data.keyword.cloud_notm}} API key so that you do not need to specify this information in your `provider` block. 

3. Create a `provider.tf` file or add the following code to your Terraform configuration file. For a full list of supported parameters that you can set in the `provider` block, see the [{{site.data.keyword.cloud_notm}} provider reference](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-provider-reference#provider-parameter-ov).

    Example for VPC infrastructure resources.
    ```terraform
    provider "ibm" {
        generation = 1
        region = "<region_name>"
    }
    ```
    {: codeblock}

    Example for classic infrastructure resources.
    ```terraform
    variable "iaas_classic_username" {
        type = "string"
    }
    variable "iaas_classic_api_key" {
        type = "string"
    }

    provider "ibm" {
        region = "<region_name>"
        iaas_classic_username = var.iaas_classic_username
        iaas_classic_api_key  = var.iaas_classic_api_key
    }
    ```
    {: codeblock}

    Example for all {{site.data.keyword.containerlong_notm}} resources.
    ```terraform
    provider "ibm" {
    }
    ```
    {: codeblock}

    Example for all other resources.
    ```terraform
    provider "ibm" {
        region = "<region_name>"
    }
    ```
    {: codeblock}

## Adding {{site.data.keyword.cloud_notm}} resources to the `resource` block
{: #configure-resources}

Use `resource` blocks to define the {{site.data.keyword.cloud_notm}} resources that you want to manage with {{site.data.keyword.bplong_notm}}. 
{: shortdesc}

To support a multi-cloud approach, Terraform works with multiple cloud providers. A cloud provider is responsible for understanding the resources that you can provision, their API, and the methods to expose these resources in the cloud. To make this knowledge available to users, every supported cloud provider must provide a command-line plug-in for Terraform that users can use to work with the resources. To find an overview of the resources that you can provision in {{site.data.keyword.cloud_notm}}, see the [{{site.data.keyword.terraform-provider_full_notm}} reference](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-resources-datasource-list). 

Example infrastructure code for provisioning a VPC. 
```terraform
resource ibm_is_vpc "vpc" {
    name = "myvpc"
}
```
{: codeblock}

### Referencing resources in other resource blocks
{: #reference-resource-info}

Review the options that you have to reference existing resources in other resource blocks of your Terraform configuration file. 
{: shortdesc}

The {{site.data.keyword.cloud_notm}} Provider plug-in reference includes two types of objects, data sources and resources. You can use both objects to reference resources in other resource blocks. 

- **Resources**: To create a resource, you use the resource definition in the {{site.data.keyword.cloud_notm}} Provider plug-in reference. A resource definition includes the syntax for configuring your {{site.data.keyword.cloud_notm}} resources and an **Attributes reference** that shows the properties that you can reference as input parameters in other resource blocks. For example, when you create a VPC, the ID of the VPC is made available after the creation. You can use the ID as an input parameter when you create a subnet for your VPC. Use this option if you combine multiple resources in one Terraform configuration file. </br>

    Example of an infrastructure code. 
    ```terraform
    resource ibm_is_vpc "vpc" {
        name = "myvpc"
    }

    resource ibm_is_security_group "sg1" {
        name = "mysecuritygroup"
    vpc  = ibm_is_vpc.vpc.id
    }
    ```
    {: codeblock}

- **Data sources**: You can also use the data sources from the {{site.data.keyword.cloud_notm}} Provider plug-in reference to retrieve information about an existing {{site.data.keyword.cloud_notm}} resource. Review the **Argument reference** section in the {{site.data.keyword.cloud_notm}} Provider plug-in reference to see what input parameters you must provide to retrieve an existing resource. Then, review the **Attributes reference** section to find an overview of parameters that are made available to you and that you can reference in your `resource` blocks. Use this option if you want to access the details of a resource that is configured in another Terraform configuration file. 

    Example of an infrastructure code.
    ```terraform
    data ibm_is_image "ubuntu" {
        name = "ubuntu-18.04-amd64"
    }

    resource ibm_is_instance "vsi1" {
        name    = "$mysi"
    vpc     = ibm_is_vpc.vpc.id
    zone    = "us-south1"
    keys    = [data.ibm_is_ssh_key.ssh_key_id.id]
    image   = data.ibm_is_image.ubuntu.id
    profile = "cc1-2x4"

    primary_network_interface {
        subnet          = ibm_is_subnet.subnet1.id
        security_groups = [ibm_is_security_group.sg1.id]
    }
    }
    ```
    {: codeblock}

## Managing resources in other account
{: #manage-resource-account}

You can create workspaces in the {{site.data.keyword.cloud_notm}} source account and execute Terraform providing resources in the target account. You can provision a resource in the target account only through command-line and API calls by using the target account's service ID with authentication, appropriate cross account authorization, or API key. To provision in the target account, you need to have a right permission of the source account.

Whereas in UI, workspaces use a user who are logged in identity for executing operations. Hence, you cannot provision the Terraform providing resources in the target account.
{: important}

## Using `variable` blocks to customize resources
{: #configure-variables}

You can use `variable` blocks to create template for your infrastructure code. For example, instead of creating multiple Terraform configuration files for a resource that you want to deploy in multiple data centers. You simply reuse the same configuration with an input variable to define the data center.
{: shortdesc}

**Where do I store my variable declarations?**

You can decide to declare your variables within the same Terraform configuration file where you specify the resources that you want to provision, or to create a separate `variables.tf` file that includes all your variable declarations. When you create a workspace, {{site.data.keyword.bplong_notm}} automatically parses through your Terraform configuration files to find variable declarations.

**What information do I need to include in my variable declaration?**

When you declare an input variable, you must provide a name for your variable and the data type according to the Terraform version. You can optionally provide default value for your variable. When input variables are imported into {{site.data.keyword.bpshort}} and a default value is specified, you can choose to overwrite the default value. \n {{site.data.keyword.bplong_notm}} accepts the values as a string for primitive types such as `bool`, `number`, `string`, and `HCL` format for complex variables.
- `Terraform v0.12` supports **string, list, map, `bool`, number, and complex data types such as list(type), map(type), object({attribute name=type,.}), set(type), tuple([type])**.

**Is there a character limit for input variables?** 

Yes. If you define input variables in your Terraform configuration file, keep in mind that the value that you enter for these variables can be up to 2049 characters. If your input variable requires a value that exceeds this limit, the value is truncated after 2049 characters. 

Example variable declaration without a default value.
```terraform
variable "datacenter" {
    type        = "string"
    description = "The data center that you want to deploy your Kubernetes cluster in."
}
```
{: codeblock}

Example variable declaration with a default value. 
```terraform
variable "datacenter" {
    type        = "string"
    description = "The data center that you want to deploy your Kubernetes cluster in."
    default = "dal10"
}
```
{: codeblock}

### Referencing variables
{: #reference-variables}

You can reference the value of the variable in other blocks of your Terraform configuration files by using the `"${var.<variable_name>}"` syntax. 
{: shortdesc}

Example for referencing a `datacenter` variable.

```terraform
resource ibm_container_cluster "test_cluster" {
    name         = "test"
    datacenter   = var.datacenter
}
```
{: codeblock}


## Providing values to {{site.data.keyword.bplong_notm}} for the declared variables
{: #declare-variable}

You can provide the values after creating the workspace for the {{site.data.keyword.bplong_notm}} to use on Terraform actions, for the variables that are declared in the template. 
- For `UI`, you can provide the values on the **{{site.data.keyword.cloud_notm}} &gt; {{site.data.keyword.bpshort}} &gt; workspace &gt; Settings page**. The `value` field is the `HCL` format value as provided in the `.tfvars` file.
- For `CLI`, you can see create, or update the values for the [Complex data type](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-update). Then, the `value` field must contain escaped string for the variable store, as shown in the example.
- For `API`, you can see [create or update the values](/apidocs/schematics/schematics#createworkspace) in the field `template_data` &gt;  `variablestore`. The `value` field is the `HCL` format value as provided in the `.tfvars` file. It is always a JSON string for any type of the variable. 

    Example

    ```json
    "variablestore": [
                {
                    "value": "[\n    {\n      internal = 800\n      external = 83009\n      protocol = \"tcp\"\n    }\n  ]",
                    "description": "",
                    "name": "docker_ports",
                    "type": "list(object({\n    internal = number\n    external = number\n    protocol = string\n  }))"
                },
                {
                    "name": "worker_pool_labels",
                    "type": "map(string)",
                    "value": "{\n        \"label-name1\": \"label-value1\",\n        \"label-name2\": \"label-value2\"\n}"
                },
                {
                    "name": "docker_ports",
                    "type": "list(object({\n    internal = number\n    external = number\n    protocol = string\n  }))",
                    "value": "[\n    {\n      internal = 800\n      external = 83009\n      protocol = \"tcp\"\n    }\n  ]",
                    "description": ""
                }
        ]
    ```
    {: codeblock}


**Can I see how to declare complex variables in a file?**

Yes, when you declare and assign the value to the variables, you can view the tooltip in the UI. The table provides few examples of the complex data type that can be declared in the variable store.

| Type | Example |
| --- | --- |
| `number` | 4.56 |
| `string` | example value |
| `bool` | false |
| `map(string)` | {key1 = "value1", key2 = "value2"} |
| `set(string)` | ["hello", "he"] |
| `map(number)` | {internal = 8080, external = 2020} |
| `list(string)` | ["us-south", "eu-gb"] |
| `list` | ["value", 30] |
| `list(list(string))` | See [list of String example](/docs/schematics?topic=schematics-create-tf-config#example-list-strings). |
| `list(object({internal = number </br> external = number </br> protocol = string}))` | See [list of Object example](/docs/schematics?topic=schematics-create-tf-config#example-list-object). |
{: caption="Complex variable types with example" caption-side="bottom"}

### Example for list of Strings
{: #example-list-strings}

```yaml
[
    {
        internal = 8300
        external = 8300
        protocol = "tcp"
    },
    {
        internal = 8301
        external = 8301
        protocol = "ldp"
    }
]
```
{: pre}

### Example for list of Objects
{: #example-list-object}

```yaml
[
    {
        internal = 8300
        external = 8300
        protocol = "tcp"
    },
    {
        internal = 8301
        external = 8301
        protocol = "ldp"
    }
]
```
{: pre}

## Storing your Terraform templates
{: #store-template}

Your Terraform configuration files contain infrastructure code that you must treat as regular code. To support collaboration, source, and version control, store your files in a GitHub or GitLab repository. With version control, you can revert to previous versions, audit changes, and share code with multiple teams. If you do not want to store your files in GitHub, provide your template by uploading a [tape archive file or `.tar`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) from your local machine instead. If you want to clone, see [allowed and blocked file extensions](/docs/schematics?topic=schematics-workspaces-faq#clone-file-extension) for cloning.
{: shortdesc}

The directory structure of the Terraform template in the GitHub repository is listed in the table with the latest updated time.

| File | Description |
|----|-----|
| README.md | Create README.md |
| `main.tf` | Create `main.tf` |
| `output.tf` | Create `output.tf` |
| `provider.tf` | Create `provider.tf` |
| `variables.tf` | Create `variables.tf` |
{: caption="Terraform template directory structure" caption-side="bottom"}

