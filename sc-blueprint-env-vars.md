---

copyright:
  years: 2017, 2023
lastupdated: "2023-01-25"

keywords: parallelism, schematics parallelism, environment variables, command-line configuration, env vars

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Using environment variables with blueprints
{: #bp-env-vars}

Environment variables are used to modify the behavior and execution of Terraform and Ansible without modifying the IaC code itself. They are set independently of the IaC code and environment inputs. Terraform uses environment variables to customize several aspects of its behavior. For example environment variables are used to increase the output verbosity for debugging or to set rarely used runtime options. 

Refer to the section [Using environment variables with workspaces](/docs/schematics?topic=schematics-set-parallelism) for more information about the environment variables that can be passed to configure Terraform runtime behavior in Schematics. 

## Blueprints usage
{: #usage}

When using Blueprints, environment variables to modify Terraform and Ansible execution are configured using the `settings` parameters in the blueprint YAML template. Refer to the [Blueprint template YAML schema](/docs/schematics?topic=schematics-bp-template-schema-yaml#bp-settings) specification for details on setting env-vars at the global blueprint level or the module level. 
