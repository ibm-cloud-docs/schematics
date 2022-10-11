---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-11"

keywords: blueprint create failure, blueprint download error, create fails,

subcollection: schematics

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the beta release.
{: beta}

# Blueprint create fails 
{: #bp-create-fails}

Review the following sections to help debugging blueprint install failures. 

## Blueprint create fails with an invalid blueprint template: failed to clone Git repository error
{: #bp-create-fails1}

Before you create a blueprint in {{site.data.keyword.bpshort}}, create fails with an error that the Blueprint, or input repositories cannot be cloned or found. 
{: tsSymptoms}

When you create the Blueprint, {{site.data.keyword.bpshort}} attempts to download the input files and blueprint template from the Git repositories that are specified on the create command and validate the YAML schema. 
{: tsCauses}

Sample error

```text
FAILED
Could not create the blueprint. Please verify that your request is correct. If the problem persists, contact IBM Cloud support.

Invalid blueprint templates. Error - Failed to clone git repository, repository not found (check url, also check the scope 'repo' of the personal access token if SCHEMATICSGITTOKEN is used)
```
{: screen}

Check that the source repositories for the Blueprints and input files are correctly specified, the referenced files and repository exists and is accessible with Git tokens specified whether necessary.
{: tsResolve} 

Rerun the Blueprints create operation with the correct repository reference.

## Blueprint create fails with an invalid blueprint template: unable to find file error
{: #bp-create-fails2}

When you create a blueprint in {{site.data.keyword.bpshort}}, the create fails before the blueprint is created with an error that the Blueprint, or input files cannot be found.
{: tsSymptoms}

Before you create the Blueprint, {{site.data.keyword.bpshort}} attempts to download the input files, and blueprint template from the Git repositories that are specified on the create command and validate the YAML schema. The repository was located, but the definition or input files cannot be found. 
{: tsCauses}

Sample error

```text
FAILED
Could not create the blueprint. Please verify that your request is correct. If the problem persists, contact IBM Cloud support.

Invalid blueprint templates. Error - Unable to find basic-blueprint1.yaml in the target repo
```
{: screen}

Check that the blueprint template file and input files that are identified in the error message exist in the target repository and are correctly specified on the create command.  
{: tsResolve} 

Rerun the Blueprints create operation with the correct file name.

## Blueprint create fails with the requested resource group as invalid
{: #bp-create-fails3}

When you create a blueprint in {{site.data.keyword.bpshort}}, the create fails before the blueprint is created with an error that the requested resource group ID is invalid or needed permissions.  
{: tsSymptoms}

The Blueprints are assigned to the {{site.data.keyword.bpshort}} management resource group passed on the create command. If the group is invalid or the user does not have the correct {{site.data.keyword.bpshort}} IAM permissions for the group the create operation fails.
{: tsCauses}

Sample error

```text
FAILED
Could not create the blueprint. Please verify that your request is correct. If the problem persists, contact IBM Cloud support.

The requested resource group id is invalid or required permissions for performing the action are not present on resource group. Please check the resource group ID and permissions.
```
{: screen}

Check that the resource group that is specified on the `--resource_group` option is valid and that the user has the correct {{site.data.keyword.bpshort}} [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create Blueprints.
{: tsResolve} 

Rerun the Blueprints create operation with the correct group name or permissions.

## Blueprint create fails with the error blueprint JSON validation failed: field missing or invalid in config
{: #bp-create-fails4}

When you create a blueprint in the {{site.data.keyword.bpshort}}. The create fails before the blueprint is created with the error that the blueprint JSON validation failed due to an invalid or missing field in the config.
{: tsSymptoms}

If the create {{site.data.keyword.bpshort}} validates that all the blueprint inputs are satisfied. If inputs are missing the validation fails, and the create stops.
{: tsCauses}

Sample error

```text
FAILED
Could not create the blueprint. Please verify that your request is correct. If the problem persists, contact IBM Cloud support.

Invalid blueprint templates. Error - Blueprint json validation failed - Field `config[resource_group_name]` missing or invalid - Value for resource_group_name not defined in config
```
{: screen}

The blueprint template is expecting more input values that are not specified in any of the input files or passed as dynamic inputs. The error output lists name of the expected missing or invalid input. Check the blueprint readme file to determine the needed dynamic inputs or add the input to an input file.
{: tsResolve} 

Rerun the blueprint create operation with all the needed inputs. 

## Blueprint create fails with the error blueprint JSON validation failed - field missing or invalid
{: #bp-create-fails5}

When you create a blueprint in {{site.data.keyword.bpshort}}, the create fails before the Blueprints is created with an error that the blueprint contains invalid definitions.  
{: tsSymptoms}

The {{site.data.keyword.bpshort}} blueprint create validates the syntax of the YAML blueprint template file. If the syntax is specified incorrectly the create fails. 
{: tsCauses}

Sample error 

```text
Could not create the blueprint. Please verify that your request is correct. If the problem exists, contact IBM Cloud support.

Invalid blueprint templates. Error - blueprint json validation failed - Field `blueprint.modules` missing or invalid
```
{: screen}

Correct the errors as identified in the validation messages. For more information about the syntax, see [Blueprint YAML schema definition](/docs/schematics?topic=schematics-bp-definition-schema-yaml). 
{: tsResolve}

Push the changes to the blueprint and input files to the Git repositories.

Rerun the blueprint create operation with the corrected syntax.
