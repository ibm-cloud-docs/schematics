---

copyright:
  years: 2017, 2022
lastupdated: "2022-06-18"

keywords: blueprint create init failure, blueprint init error, create init fails 

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do Blueprint create fails in the Blueprint create_init step?
{: #bp-create_init-fails}

When you create a Blueprint in {{site.data.keyword.bpshort}}, the create fails during initialization of the {{site.data.keyword.bpshort}} Workspaces for the modules. 
{: tsSymptoms}

Blueprints are created in two steps. The first step retrieves and validates the Blueprint definition. The second step creates the required module Workspaces based on the module statements in the definition. This step clones the specified module source repos. An incorrectly specified repo URL will result in an initialisation failure.
{: tsCauses}

Sample error

```text
Created Blueprint   blueprints-starter-example   eu-gb.BLUEPRINT.blueprints-starter-example.deee1f6f

Modules to be created
SNO   Type        Name   
1     Terraform   terraform_module2   
2     Terraform   terraform_module1   
      
Blueprint job running eu-gb.JOB.blueprints-starter-example.e44adbab

Waiting:2    Draft:0    Connecting:0    InProgress:0    Inactive:0    Active:0    Failed:0   

Type        Name                                         Status                     Job ID   
Blueprint   blueprints-starter-example   CREATE_FAILED   eu-gb.JOB.blueprints-starter-example.e44adbab   
Terraform   terraform_module2            NA                 
Terraform   terraform_module1            NA                 
            
Blueprint create_failed at Tue Jun 14 13:21:49 BST 2022
OK
```
{: screen} 

- Review the error messages from the log of the failing Blueprint job to determine the cause of the failure.  
{: tsResolve}

- Correct definition of the module source repository in the Blueprint definition.
- Push the changes to the Blueprint definition to Git repository. 
- Use the `ibmcloud schematics blueprint update` command to update the Blueprint configuration settings and complete the Workspace creation.

Example

```sh
ibmcloud schematics blueprint update -id <blueprint_id> 
```
{: pre}

