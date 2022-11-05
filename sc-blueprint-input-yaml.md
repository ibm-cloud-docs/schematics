---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-05"

keywords: schematics blueprints infrastructure, blueprints schema, schema definitions, templates, yaml,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# blueprint input file YAML Schema
{: #bp-input-schema-yaml}

This document is the reference of the YAML schema used to describe a blueprint input YAML file containing the blueprint input variables required by a template. 

A blueprint input file defines the [input variables](/docs/schematics?topic=schematics-blueprint-templates#blueprint-input-statements) required to customize the template to a specific use case. If a value is not defined, it is assumed that the input is satisfied by a user defined input value at blueprint config create time. These must be satisfied at create time by an input file or dynamic inputs. 
{: shortdesc}  

The type of an input variable is defined in the [input block](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-modules-inputs-options) of the consuming Module in the template. The supported types are the same as the [Terraform variable types](https://developer.hashicorp.com/terraform/language/expressions/types). If the type is omitted the default is `string`.

## Defining input values
{: #define-input-value}

Blueprint input variables are represented as YAML `key: value` pairs. With one input value per line. The example here represents input values where the type defaults to a simple string. 

``` yaml
resource_group: default
region: us-south
```
{: codeblock}

## Complex input values
{: #complex-input-value}

{{site.data.keyword.bpshort}} Blueprints has full support for all Terraform HCL complex data types. To retain compatibility with Terraform HCL and readability for Terraform users, complex variables are not represented directly in YAML as collections or lists, but retain their original HCL representation. Complex variables are represented as single or multi-line strings, as flow or block scalars.  

See the sample [blueprint complex inputs](https://github.com/Cloud-Schematics/blueprint-complex-inputs){: external} contains several complex data types represented as YAML scalars. 
{: shortdesc} 

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
      internal = 9900
      external = 9900
      protocol = "tcp"
    },
    {
      internal = 9901
      external = 9901
      protocol = "ldp"
    }
  ]

boolean_var: false  
```
{: codeblock}
