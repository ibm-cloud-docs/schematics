---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-28"

keywords: blueprint,  modules, terraform modules, root, child, injection 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Using Terraform modules with blueprint templates 
{: #blueprint-terraform}    

One of the use cases for blueprints is to compose infrastructure architectures directly from [Terraform modules](https://developer.hashicorp.com/terraform/language/modules#modules).
{: shortdesc}

Blueprint templates can reuse existing Terraform modules from the [Terraform registry](https://registry.terraform.io/namespaces/terraform-ibm-modules){: external}, along with {{site.data.keyword.IBM_notm}} and user created modules from public and private libraries. Best practice implementations for {{site.data.keyword.IBM_notm}} Cloud are available as reusable Terraform modules in the [terraform-ibm-modules](https://github.com/terraform-ibm-modules){: external} GitHub repo and the Terraform registry. 

The section [Orchestration and modules](/docs/schematics?topic=schematics-blueprint-scaling#blueprint-scaling-orchestration) describes the design philosophy of the design and usage of modules within the Blueprints orchestration framework. See the [IBM module authoring guidelines](https://terraform-ibm-modules.github.io/documentation/#/implementation-guidelines) for creating user modules compliant with {{site.data.keyword.bpshort}} Blueprints. 
 
The combination of {{site.data.keyword.IBM_notm}} and user modules from public and private repos to create a custom template is illustrated in the figure.  

![Custom templates with public and private modules](/images/new/bp-terraform-modules.svg){: caption="Custom templates with public and private modules" caption-side="bottom"}

The template determines the architecture, specifying the modules required for the implementation and their source repositories. As illustrated publicly available module functionality can be combined with third-party and user developed modules to create customized solutions. 

Templates support the use of both Terraform [root modules](https://developer.hashicorp.com/terraform/language/modules#the-root-module){: external} and Terraform [child](https://developer.hashicorp.com/terraform/language/modules#child-modules){: external} modules. Root modules can be used by referencing the source library and passing the required inputs. Reusable child modules require additional blueprint template `injector` parameters to enable usage with templates. Use of injector parameters is described in the next sections.    
{: shortdesc}

## Root modules and Terraform configurations
{: #blueprint-root-module-config}

A root module is the root of an executable Terraform configuration. A root module, also known as a Terraform config or Terraform template and can be run directly by the Terraform command line binary or {{site.data.keyword.bpshort}}. 

A Terraform configuration consists of a root module as the `main` directory that contains `.tf` files. From the root module, Terraform can call child modules [published](https://developer.hashicorp.com/terraform/language/modules#published-modules){: external} to public or private repositories. Configurations (root modules) written for {{site.data.keyword.cloud_notm}} must contain a `provider` block and  a `terraform>required_providers` block to identify the [{{site.data.keyword.IBM_notm}} Cloud provider](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs){: external} to Terraform.   

A root module can be used unchanged in a blueprint template, by referencing the module source repo and providing input values. It must contain an {{site.data.keyword.IBM_notm}} Cloud provider definition. 

## Child and reusable shared modules
{: #blueprint-reusable-module}

Sharing and reuse of child modules is at the heart of an effective IaC automation strategy and use of {{site.data.keyword.bpshort}} Blueprints. 

Child module, is the term given to any module that is called by another module or root module. Child modules cannot be executed stand-alone and the naming distinguishes it from root modules which can be executed directly. Sharing and reuse builds on the Terraform support for loading child modules from shared public and private repositories and the Terraform registry. Remote loading enables the sharing of content by [publishing modules](https://developer.hashicorp.com/terraform/language/modules#published-modules){: external} for others to use, and to reuse published modules. 

Examples of {{site.data.keyword.IBM_notm}} authored (child) modules designed for reuse can be found in the [terraform-ibm-modules](https://github.com/terraform-ibm-modules){: external} GitHub repository.

## Blueprints provider injection
{: #bp-provider-injection}  

To enable a reusable child module to be executed as a root module, {{site.data.keyword.bpshort}} Blueprints injects the {{site.data.keyword.IBM_notm}} Cloud provider config and version information as `.tf` files into the Terraform working directory at run time. The same approach is used by [Terragrunt](https://terragrunt.gruntwork.io/docs/reference/config-blocks-and-attributes/#a-note-about-using-modules-from-the-registry){: external} to allow shared (child) modules to be executed directly as root modules.

The contents of the Terraform working directory for a blueprint module with provider injection is illustrated. 

![Blueprint provider injection](/images/new/bp-injection.svg){: caption="Blueprint provider injection" caption-side="bottom"}
 
The `main.tf` and `variables.tf` files are loaded from the module repo. The provider config is defined by an `injectors` block in the template module definition. This block defines the templated `.tf` files that contain the additional config statements. It also defines the specification of the additional Terraform language constructs that will be injected into the Terraform working directory. 

The two files `ibm_tft_provider_override.tf` and `ibm_tft_versions_override.tf` contain the additional injected config parameters.

### Templating Terraform language statements
{: #blueprint-template-statement}

{{site.data.keyword.bpshort}}blueprint templates use [mustache templates](https://mustache.github.io/){: external} to create Terraform language statements. These are injected as `.tf` files in the Terraform working directory at run time. The templates are retrieved from the Git repo defined in the injectors block. Provider injection examples are provided in the [Cloud-Schematics/tf-templates](https://github.com/Cloud-Schematics/tf-templates){: external} GitHub repo. 

The repo folder contains the files to be injected, which contain the mustache templates. The `ibm` folder contains the two template files for injecting and configuring the {{site.data.keyword.IBM_notm}} Cloud provider and setting the `provider_version` to be used.  

- tf_ibm_provider_override.tft 
- tf_ibm_versions_override.tft

### Configuring provider injection
{: #blueprint-provider-injection}

To configure provider injection, an `injectors` block is added to the template module definition. See the [module.injectors](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-modules-injector) for the syntax for the injectors block. An example is shown below. 

```yaml
modules:
  - name: accounts
     module_type 
    ....
     injectors:
      - tft_git_url: "https://github.com/Cloud-Schematics/tf-templates"
        tft_name: "ibm"
        injection_type: override
        tft_parameters:
          - name: provider_version
            value: 1.42.3
          - name: provider_source
            value: IBM-Cloud/ibm
          - name: region
            value: us-south
```

Two injection options are supported, inject or override. Definitions can be injected as additional files if its believed there is no conflict with any existing HCL statements. Alternatively they can be injected as [HCL override files](https://developer.hashicorp.com/terraform/language/files/override){: external}. Overrides is a feature that allows Terraform to override portions of an existing configuration. It is intended for use in those rare circumstances where it is necessary to override the original authors intent. With these provider definitions, override is used to ensure that if there is an existing `required_providers` statement in the module, it can be replaced without conflict. 

The inputs to the mustache templates are defined in the `[tft_parameters](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-modules-tft-parameters)` section. Refer to the tft files in the [Cloud-Schematics/tf-templates/ibm](https://github.com/Cloud-Schematics/tf-templates/tree/main/ibm){: external} folder to determine the supported input parameters for the Terraform language definitions. For the {{site.data.keyword.IBM}} Terraform provider the example required inputs are: 

```yaml
          - name: provider_version
            value: 1.38.2
          - name: provider_source
            value: IBM-Cloud/ibm
          - name: region
            value: us-south
```
{: pre}

With the example inputs, the generated output files are: 

tf_ibm_versions_override.tf

```yaml
terraform {
   required_providers {
     ibm = {
       source  = "IBM-Cloud/ibm"
       version = "1.38.2"
     }
   }
 }
```
{: pre}

tf_ibm_provider_override.tf

```yaml
provider "ibm" {
   region = "us-south"
}
```


## Next steps
{: #bp-terraform-nextsteps}

In this section you have learned about using Terraform modules with blueprints. 
- Explore [understanding blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates) to create templates containing modules. 

- Looking for blueprint samples using modules? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external}. Check the `Readme` files of the examples for further customization and usage for each sample. 
