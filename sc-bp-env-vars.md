---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-19"

keywords: parallelism, schematics parallelism, environment variables, command-line configuration, env vars

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Using environment variables with blueprints
{: #bp-env-vars}

Terraform uses various environment variables to customize different aspects of its behavior. These environment variables are used to increase the output verbosity for debugging or rarely used runtime options. 

Refer to the section [Using environment variables with workspaces](/docs/schematics?topic=schematics-set-parallelism) for more information about the environment variables that can be passed to configure Terraform runtime behavior in Schematics. 

## Blueprints usage
{: #usage}

When using Blueprints, environment variables are set using the `settings` parameters in the blueprint YAML template. Refer to the [Blueprint template YAML schema](docs/schematics?topic=schematics-bp-template-schema-yaml#bp-settings) specification for details on setting both at the global blueprint level and the module level. 





