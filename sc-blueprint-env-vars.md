---

copyright:
  years: 2017, 2024
lastupdated: "2024-02-28"

keywords: parallelism, schematics parallelism, environment variables, command-line configuration, env vars

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Using environment variables with blueprints
{: #bp-env-vars}

Environment variables are used to modify the behavior and execution of workspaces and actions without modifying the IaC code itself. They are set independently of the IaC code and environment inputs. Terraform uses environment variables to customize several aspects of its behavior. For example environment variables are used to increase the output verbosity for debugging or to set rarely used runtime options. 

Refer to the section [Using environment variables with workspaces](/docs/schematics?topic=schematics-set-parallelism) for more information about the environment variables that can be passed to configure Terraform runtime behavior in Schematics. 

## Blueprints usage
{: #usage}

When using Blueprints, environment variables to modify workspace and action execution are configured using the `settings` parameters in the blueprint YAML template. Refer to the Blueprint template YAML schema specification for details on setting env-vars at the global blueprint level or the module level. 
