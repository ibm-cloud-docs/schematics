---

copyright:
  years: 2017,2022
lastupdated: "2022-07-04"

keywords: blueprint create init failure, blueprint init error, create init fails 

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Blueprints create fails in the Blueprints `create_init` step?
{: #bp-create-init-fails}

When you create a Blueprints in {{site.data.keyword.bpshort}}, the create fails during initialization of the {{site.data.keyword.bpshort}} Workspaces for the modules. 
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

- Review the error messages from the log of the failing Blueprints job to determine the cause of the failure.  
{: tsResolve}

- Correct definition of the module source repository in the Blueprint definition.
- Push the changes to the Blueprint definition to Git repository. 
- Use the `ibmcloud schematics blueprint update` command to update the Blueprints configuration settings and complete the Workspace creation.

Example

```sh
ibmcloud schematics blueprint update -id <blueprint_id> 
```
{: pre}

On command-line command failures, {{site.data.keyword.bpshort}} Blueprints identifies the failing Workspace and print a summary of the log.  

Sample log output

```text
Modules to be installed
SNO   Type        Name                   Status   
1     Terraform   basic-resource-group   INACTIVE   
2     Terraform   basic-cos-storage      INACTIVE   
      
Blueprint job running eu-gb.JOB.basic.f012ad25

Waiting:0    Draft:0    Connecting:0    InProgress:0    Inactive:0    Active:2    Failed:0   

Type        Name                   Status               Job ID   
Blueprint   basic                  FULFILMENT_SUCCESS   eu-gb.JOB.basic.f012ad25   
Terraform   basic-resource-group   FAILED                  
Terraform   basic-cos-storage      ACTIVE                  
            
Blueprint fulfilment_failure at Tue May 31 11:44:12 BST 2022
Module basic-resource-group failure

Log Summary 
 2022/02/17 11:10:40  
 2022/02/17 11:10:40    Terraform REFRESH error: Terraform REFRESH errorexit status 1
 2022/02/17 11:10:40    Could not execute action
----- 
OK

```
{: screen}

You are directed to the failing Workspace and Terraform automation code. From the provided log output you can follow the existing procedures to resolve the Terraform apply failure.  
