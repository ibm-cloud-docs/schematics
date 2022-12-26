---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-23"

keywords: blueprint create init failure, blueprint init error, create init fails,

subcollection: schematics

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# blueprint create fails in the create_init step
{: #bp-create-init-fails}

When you create a blueprint config, it fails during initialization of the blueprint modules. 
{: tsSymptoms}

Blueprint environments are created in two steps. The first step retrieves, validates the blueprint template, and creates the configuration in {{site.data.keyword.bpshort}}. The second step creates the needed modules based on the module definitions in the template. This step clones the specified module source repos. An incorrectly specified repo URL results in a workspace initialization failure. Other possible causes are the repository is private and an access token is not specified, or the access token is invalid. 
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

Type        Name                         Status                     Job ID   
Blueprint   blueprints-starter-example   CREATE_FAILED   eu-gb.JOB.blueprints-starter-example.e44adbab   
Terraform   terraform_module2            NA                 
Terraform   terraform_module1            NA                 
            
Blueprint create_failed at Tue Jun 14 13:21:49 BST 2022
OK
```
{: screen} 

The job status shows as `create_failed`. Additionally the Terraform job status shows as `NA` indicating that {{site.data.keyword.bpshort}} failed to create the modules and no logs are available. 


Review the error messages from the log of the failing blueprint job, the blueprint module definitions, and Git repositories to determine the cause of the initialization failure.  
{: tsResolve}

Typical issues are:
- An incorrectly specified module repo URL 
- Repo is private and an access token is not specified
- Invalid Git access token 

Correct the cause of the failure. Most typically it is an invalid module repo URL and the blueprint module statement require updating.
- Update the blueprint module statements to specify the correct Git repo URL
- Push the new release of the blueprint template to its Git source repository. With an updated release tag if needed.

If you use the blueprint template current version, run the `ibmcloud schematics blueprint update` command to refresh the blueprint configuration that is stored by {{site.data.keyword.bpshort}} with the update to the blueprint template. 


```sh
ibmcloud schematics blueprint update -id <blueprint_ID> 
```
{: pre}

If explicit blueprint template versioning is used with release tags for each template release, the blueprint configuration must be updated in {{site.data.keyword.bpshort}} with the new release tag.  

```sh
ibmcloud schematics blueprint update --id <blueprint_ID> --bp-git-release x.y.z  
```
{: pre}


Verify that the blueprint config is updated successfully. When you update the config using the CLI. The command displays details of the modules to be updated, and continuously updating status of the progress of the {{site.data.keyword.bpshort}} jobs initializing the modules. The command returns on completion.

```text
Update blueprint ID: eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936
Modules to be updated
SNO   Type        Name   
1     Workspace   basic-resource-group   
2     Workspace   basic-cos-storage   
      
Blueprint job running eu-de.JOB.Blueprint-Basic-Example.da1b13ca
Waiting:0    Draft:0    Connecting:0    In Progress:0    Inactive:2    Active:0    Failed:0   

Type        Name                      Status           Job ID   
Blueprint   Blueprint Basic Example   CREATE_SUCCESS   eu-de.JOB.Blueprint-Basic-Example.da1b13ca   
Workspace   basic-resource-group      INACTIVE            
Workspace   basic-cos-storage         INACTIVE            
            
Blueprint ID eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936 `update_success` at Mon Jun 27 16:18:47 BST 2022
OK
```
{: screen}

On successful completion the config update returns **`update_success`**. All modules must be initialized to `Inactive` state and deployment of the environment can continue with [blueprint apply](/docs/schematics?topic=schematics-apply-blueprint). 

If the update job fails, repeat problem diagnosis, and resolution until update completes successfully and all modules are in an `Inactive` state.
