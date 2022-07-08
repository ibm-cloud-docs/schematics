---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-06"

keywords: blueprint install failure, terraform error, terraform fails, install fails,

subcollection: schematics

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Blueprint install fails 
{: #bp-install-fails}

Review the following sections to assist in debugging Blueprint install failures. 


## Blueprint install fails with message "Install of module xyz Failed"
{: #bp-install-fails1}

When you run the Blueprint install command, it fails with message that the install of a module has failed.    
{: tsSymptoms}

When you install a Blueprint, {{site.data.keyword.bpshort}} runs Terraform Apply operations against all Workspaces which have pending updates.  Errors when running Terraform operations against Workspaces will result in a Workspace job failure and will cause the Blueprint install job to terminate (fail). 
{: tsCauses}

Example error message

```text
Modules to be installed
SNO   Type        Name                   Status   
1     Workspace   basic-resource-group   ACTIVE   
2     Workspace   basic-cos-storage      FAILED   
      
Blueprint job running eu-gb.JOB.Blueprint-Basic-Example.0cebdb53

Waiting:0    Draft:0    Connecting:0    In Progress:0    Inactive:0    Active:1    Failed:1   

Type        Name                      Status              Job ID   
Blueprint   Blueprint Basic Example   FULFILMENT_FAILED   eu-gb.JOB.Blueprint-Basic-Example.0cebdb53   
Workspace   basic-resource-group      ACTIVE                 
Workspace   basic-cos-storage         FAILED                 
            
Errors:
Install of module basic-cos-storage Failed

Error on Terraform Apply operation. Review log for cause of failure
Review job failure log yes/no [y/N]> y


Workspace Name       basic-cos-storage   
Workspace ID         eu-gb.workspace.basic-cos-storage.7378a905   
Log Summary          (last few lines)..........   
                     :12 Terraform apply |   with ibm_cos_bucket.standard-ams03,   
                      2022/07/02 17:57:12 Terraform apply |   on main.tf line 64, in resource "ibm_cos_bucket" "standard-ams03":   
                      2022/07/02 17:57:12 Terraform apply |   64: resource "ibm_cos_bucket" "standard-ams03" {   
                      2022/07/02 17:57:12 Terraform apply |    
                      2022/07/02 17:57:12 Terraform APPLY error: Terraform APPLY errorexit status 1   
                      2022/07/02 17:57:12 Could not execute job: Error : Terraform APPLY errorexit status 1   
                        
Status               FAILED   
Workspace Logs CLI   ibmcloud schematics logs --id eu-gb.workspace.basic-cos-storage.7378a905 --act-id a28612c58714c908a0d0c111df7cc74c   
                        
Attention! Job ID: eu-gb.JOB.Blueprint-Basic-Example.0cebdb53 FULFILMENT_FAILED
```
{: screen}

Install failures are related to Terraform execution and the specific Terraform config being executed. Debugging a Blueprint install failure follows the same approach as is followed for debugging a Terraform command failure for a {{site.data.keyword.bpshort}} Workspace or standalone Terraform usage. 
{: tsResolve} 

The Blueprint install command will indicate which module has failed. Then prompt to show a summary of the log for the failed Workspace.  

```text
Errors:
Install of module basic-cos-storage Failed

Error on Terraform Apply operation. Review log for cause of failure
Review job failure log yes/no [y/N]>
```
{: screen}

To assist in problem diagnosis reply `y` to the prompt to review the failure logs. The Blueprint Install CLI command output includes the last few lines of the Terraform Apply job log.  As Terraform terminates on an error, frequently the last few lines of the Terraform apply log will include sufficient information to identify the cause of the Terraform failure. 

If the log summary does not provide sufficient information, the `ibmcloud schematics logs` command to review the entire Terraform log for the failing Workspace is printed below the summary. 

```text
Status               FAILED   
Workspace Logs CLI   ibmcloud schematics logs --id eu-gb.workspace.basic-cos-storage.7378a905 --act-id a28612c58714c908a0d0c111df7cc74c 
```
{: screen}


See Troubleshooting {{site.data.keyword.bpshort}} apply errors for additional information on debugging Terraform Apply failures  

## Blueprint install failure due to Terraform config coding error  
{: #bp-install-fails2}

When you run the Blueprint install command, it fails with message that the install of module has failed. 
{: tsSymptoms}

The analysis of the Workspace logs indicates that the modules Terraform config has a coding error which caused the Terraform apply failure.  
{: tsCauses}

Correct the Terraform config error at source and push a new release to its Git source repository . 
{: tsResolve} 

If explicit versioning of Blueprint modules is used or specific branches, the Blueprint definition will also require updating in its Git repo to specify the new release tag or branch fon the module statement. 
- Update the Blueprint module statements to specify the new module version. 
- Push the new release of the Blueprint definition to its Git source repository. With an updated release tag for the Blueprint definition if required.

For modules, when no Git release is specified on the Blueprint module statements and relaxed module versioning is used, to always use the latest module release, no update to the Blueprint definition is required. The (latest) change to the module repo will be pulled in automatically by Schematics.  

Run the `ibmcloud schematics blueprint update` command to refresh the Blueprint configuration stored by {{site.data.keyword.bpshort}} with the update to the Blueprint definition. With 'latest' release, {{site.data.keyword.bpshort}} will identify the updated module Git repos and perform a Pull-Latest to update any Workspaces with the modified Terraform configs.  

```sh
ibmcloud schematics blueprint update -id <blueprint_id> 
```
{: pre}

If explicit Blueprint versioning is used with release tags for each Blueprint definition release, the Blueprint configuration must be updated in {{site.data.keyword.bpshort}} with the new Blueprint release tag.  

```sh
ibmcloud schematics blueprint update --id <blueprint_id> --bp-git-release x.y.z  
```
{: pre}

Finally, run the `ibmcloud schematics blueprint install` command to rerun the failed Terraform Apply operation and to complete all operations against all workspaces.  

```sh
ibmcloud schematics blueprint install -id <blueprint_id> 
```
{: pre}

## Blueprint install failure due to Terraform timeouts or transient failures 
{: #bp-install-fails3}

When you run the Blueprint install command, it fails with message that the install of module has failed. 
{: tsSymptoms}

Analysis of the logs indicates that the modules Terraform apply operation timed out or a transient failure occuted. 
{: tsCauses}


No user action should be necessary to recover from this and the Blueprint install operation can be retried. 
{: tsResolve} 

Run the `ibmcloud schematics blueprint install` command to rerun the failed Terraform Apply operation and complete all operations against all workspaces.  

```sh
ibmcloud schematics blueprint install -id <blueprint_id> 
```
{: pre}

## Blueprint install failures that require changes to values in input files  
{: #bp-install-fails4}

When you run the Blueprint install command, it fails with message that the install of module has failed. 
{: tsSymptoms}

Analysis of the Workspace logs indicates that the cause of the Terraform apply failure was due to an incorrect input value.  
{: tsCauses}

Update the input file source and push a new release to its Git source repository . 
{: tsResolve} 

If explicit input file versioning is used with release tags for each input file release, the Blueprint configuration must be updated in {{site.data.keyword.bpshort}} with the new input file release tag.  

```sh
ibmcloud schematics blueprint update --id <blueprint_id> --input-git-release x.y.z  
```
{: pre}

Where no Git release is specified and relaxed module versioning (latest) is used for input files in the Blueprint config, no change to the Blueprint config is required and the input file changes will be pulled in automatically by Schematics.  

Run the `ibmcloud schematics blueprint update` command to refresh the Blueprint configuration with the changes. This will update the Blueprint and Workspaces with the updated input values. 

```sh
ibmcloud schematics blueprint update -id <blueprint_id> 
```
{: pre}

Finally, run the `ibmcloud schematics blueprint install` command to rerun the failed Terraform Apply operation with the changed input values and complete all operations against all Workspaces.  

```sh
ibmcloud schematics blueprint install -id <blueprint_id> 
```
{: pre}

## Blueprint install failures that require changes to dynamic inputs
{: #bp-install-fails5}

When you run the Blueprint install command, it fails with message that the install of module has failed. 
{: tsSymptoms}

Analysis of the logs indicates that the cause of the Terraform apply failure was due to an incorrect dynamic input value  
{: tsCauses}

Identify the input value that must be updated and run the `ibmcloud schematics blueprint update` command to refresh the Blueprint configuration with the change. This will update the Blueprint and Workspaces with updated input value. Input values can be changed individually and all dynamic inputs do not have to be re-entered. 

```sh
ibmcloud schematics blueprint update --id <blueprint_id> --inputs <name>=<value>
```
{: pre}

Finally run the `ibmcloud schematics blueprint install` command to rerun the failed Terraform Apply operation with the updated dynamic inputs and complete all operations against all Workspaces.  

```sh
ibmcloud schematics blueprint install -id <blueprint_id> 
```
{: pre}