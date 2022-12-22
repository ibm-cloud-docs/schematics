---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-22"

keywords: schematics blueprints infrastructure, blueprints schema, schema definitions, templates, yaml,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Blueprints input file YAML Schema
{: #bp-input-schema-yaml}

This document is the reference of the YAML schema used to describe a blueprint input YAML file containing the blueprint input values required by a template. 

Blueprint inputs play an important role in the customization of a blueprint template to deploy a target environment: dev, stage, prod or regions. They provide configuration for scaling, region, networking and additional parameters. Review the section on [Blueprint customization](/docs/schematics?topic=schematics-blueprint-reuse#blueprint-customization) for more detail on defining and using inputs.  


A blueprint input file defines the [input values](/docs/schematics?topic=schematics-blueprint-templates#blueprint-input-statements) required to customize the template to a specific use case. If a value is not defined in the template, it is assumed that the input is satisfied by a user defined input value at blueprint create time. These must be satisfied at create time by an input file (as described here) or dynamic inputs. Review the section on [input precedence order](/docs/schematics?topic=schematics-blueprint-reuse#blueprint-input-precedence)
{: shortdesc}  

The type of an input variable is defined in the [input block](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-modules-inputs-options) of the template. The supported types are the same as the [Terraform variable types](https://developer.hashicorp.com/terraform/language/expressions/types). If the type is omitted the default is `string`.

## Defining input values
{: #define-input-value}

Blueprint input variables are represented as YAML `key: value` pairs. With one input value per line. The example here represents input values where the type is `string`. 

``` yaml
resource_group: default
region: us-south
```
{: codeblock}

## Complex input values
{: #complex-input-value}

{{site.data.keyword.bpshort}} Blueprints has full support for all Terraform HCL complex data types. To retain compatibility with Terraform HCL and readability for Terraform users, complex variables are not represented directly in YAML as collections or lists, but retain their original HCL (string) representation. Complex variables are represented as single or multi-line strings, as flow or block scalars. Several resources exist documenting the styles of YAML value representation. For example [https://yaml-multiline.info/](https://yaml-multiline.info/). 

The use of block scalars is recommended, over the use of flow scalars. Block scalars maintain single and double quotes, flow scalars require attention to be paid to escaping single and double quotes for the string to be passed as is to Terraform.   

The example input file, [blueprint complex inputs](https://github.com/Cloud-Schematics/blueprint-complex-inputs){: external} contains several complex data types represented as YAML scalars. 
{: shortdesc} 

The use of 


Example inputs

```yaml
resource_group: default
region: us-south
sample_var: testconfig

# Complex vars - https://yaml-multiline.info/
# Flow scalar literal 
list_any_flow_scalar:  "[\"36\", \"mqm-grand\", \"madison-circle-garden\"]"

# Block scalar literal 
list_any_block_scalar: |
    [
        "36", 
        "mqm-grand", 
        "madison-circle-garden"
    ]

# Block scalar literal - multi line       
docker_ports: | 
  [
    {
      internal = 9900,
      external = 9900,
      protocol = "tcp"
    },
    {
      internal = 9901,
      external = 9901,
      protocol = "ldp"
    }
  ]

boolean_var: false  
```
{: codeblock}

The input values `list_any_flow_scalar` and `list_any_block_scalar` illustrate the two different forms of representing the same HCL complex variable value. 
- Flow scalar: Value specified on a single line without using new line characters. Double quotes must be escaped to pass the string as is to Terraform.  
- Block scalar: Value is specified across multiple lines in `literal` style using the `|` symbol to maintain new lines in the value. No escaping required. 

