---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-11"

keywords: blueprint create failure, blueprint download error, create fails,

subcollection: schematics

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Blueprint create fails 
{: #bp-create-fails}

Review the following sections to assist in debugging Blueprint install failures. 

## Blueprint create fails with an invalid blueprint definitions: failed to clone Git repository error
{: #bp-create-fails1}

When you create a Blueprint in {{site.data.keyword.bpshort}}, the create fails before the Blueprint is created with an error that the Blueprint or input repositories cannot be cloned or found. 
{: tsSymptoms}

Before creating the Blueprint, {{site.data.keyword.bpshort}} attempts to download the input files and Blueprint definition from the Git repositories specified on the create command and validate the YAML schema. 
{: tsCauses}

Sample error

```text
FAILED
Could not create the blueprint. Please verify that your request is correct. If the problem persists, contact IBM Cloud support.

Invalid blueprint definitions. Error - Failed to clone git repository, repository not found (check url, also check the scope 'repo' of the personal access token if SCHEMATICSGITTOKEN is used)
```
{: screen}

Check that the source repository for the Blueprints and input files are correctly specified, the referenced files and repository exists and is accessible with Git tokens specified if necessary.
{: tsResolve} 

Rerun the Blueprints create operation with the correct repository reference.

## Blueprint create fails with an invalid blueprint definitions: unable to find file error
{: #bp-create-fails2}

When you create a Blueprint in {{site.data.keyword.bpshort}}, the create fails before the Blueprint is created with an error that the Blueprint or input files cannot be found.
{: tsSymptoms}

Before creating the Blueprint, {{site.data.keyword.bpshort}} attempts to download the input files and Blueprint definition from the Git repositories specified on the create command and validate the YAML schema. The repository was located, but the definition or input files cannot be found. 
{: tsCauses}

Sample error

```text
FAILED
Could not create the blueprint. Please verify that your request is correct. If the problem persists, contact IBM Cloud support.

Invalid blueprint definitions. Error - Unable to find basic-blueprint1.yaml in the target repo
```
{: screen}

Check that the Blueprint definition file and input files identified in the error message exist in the target repo and are correctly specified on the create command.  
{: tsResolve} 

Rerun the Blueprints create operation with the correct file name.

## Blueprint create fails with the requested resource group is invalid
{: #bp-create-fails3}

When you create a Blueprint in {{site.data.keyword.bpshort}}, the create fails before the Blueprint is created with an error that the requested resource group ID is invalid or required permissions.  
{: tsSymptoms}

During creation the Blueprint is assigned to the {{site.data.keyword.bpshort}} management resource group passed on the create command. If the group is invalid or the user does not have the correct {{site.data.keyword.bpshort}} IAM permissions for the group the create operation will fail. 
{: tsCauses}

Sample error

```text
FAILED
Could not create the blueprint. Please verify that your request is correct. If the problem persists, contact IBM Cloud support.

The requested resource group id is invalid or required permissions for performing the action are not present on resource group. Please check the resource group ID and permissions.
```
{: screen}

Check that the resource group specified on the `--resource_group` option is valid and that the user has the correct  {{site.data.keyword.bpshort}} [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create Blueprints.
{: tsResolve} 

Rerun the Blueprints create operation with the correct group name or permissions.

## Blueprint create fails with the error Blueprint JSON validation failed: field missing or invalid in config
{: #bp-create-fails4}

When you create a Blueprint in {{site.data.keyword.bpshort}}, the create fails before the Blueprint is created with the error that the Blueprint JSON validation failed due to an invalid or missing field in the config. 
{: tsSymptoms}

During creation {{site.data.keyword.bpshort}} validates that all the Blueprint inputs are satisfied. If there are missing inputs the validation will fail and the create will be terminated.  
{: tsCauses}

Sample error

```text
FAILED
Could not create the blueprint. Please verify that your request is correct. If the problem persists, contact IBM Cloud support.

Invalid blueprint definitions. Error - Blueprint json validation failed - Field `config[resource_group_name]` missing or invalid - Value for resource_group_name not defined in config
```
{: screen}

The Blueprint definition is expecting additional input values that are not specified in any of the input files or passed as dynamic inputs. The error output will list name of the expected missing or invalid input. Check the Blueprint Readme file to determine the required dynamic inputs or add the input to an input file.  
{: tsResolve} 

Rerun the Blueprints create operation with all the required inputs. 

## Blueprint create fails with the error Blueprint JSON validation failed - field missing or invalid 
{: #bp-create-fails5}

When you create a Blueprint in {{site.data.keyword.bpshort}}, the create fails before the Blueprints is created with an error that the Blueprint contains invalid definitions.  
{: tsSymptoms}

Before creating the Blueprint, {{site.data.keyword.bpshort}} validates the syntax of the YAML Blueprint definition file. If the syntax is specified incorrectly the create will fail. 
{: tsCauses}

Sample error 

```text
Could not create the blueprint. Please verify that your request is correct. If the problem exists, contact IBM Cloud support.

Invalid blueprint definitions. Error - Blueprint json validation failed - Field `blueprint.modules` missing or invalid
```
{: screen}

Correct the errors as identified in the validation messages. Refer to the [Blueprint YAML schema definition](/docs/schematics?topic=schematics-bp-definition-schema-yaml) for additional information on the required syntax. 
{: tsResolve}

Push the changes to the Blueprint and input files to the Git repositories.

Rerun the Blueprints create operation with the corrected syntax.
