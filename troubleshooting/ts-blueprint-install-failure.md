---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-11"

keywords: blueprint install failure, terraform error, terraform fails, install fails,

subcollection: schematics

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the beta release.
{: beta}

# Blueprint install fails 
{: #bp-install-fails}

Review the following sections to help debugging blueprint install failures. 


## Blueprint install fails with message "Install of module Failed"
{: #bp-install-fails1}

When you run the blueprint install command, it fails with message that the install of a module fails.    
{: tsSymptoms}

When you install a Blueprint, {{site.data.keyword.bpshort}} runs Terraform Apply operations against all Workspaces, which have pending updates. Errors when you run the Terraform operations against the Workspaces result in a Workspace job failure, and cause the blueprint install job to fail. 
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
Blueprint   blueprint Basic Example   FULFILMENT_FAILED   eu-gb.JOB.Blueprint-Basic-Example.0cebdb53   
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

Install failures are related to Terraform execution and the specific Terraform config being ran. Debugging a blueprint install failure follows the same approach as is followed for debugging a Terraform command failure for a {{site.data.keyword.bpshort}} Workspace or stand-alone Terraform usage. 
{: tsResolve} 

The blueprint install command indicates which module fails. Then, prompt to show a summary of the log for the failed Workspace.  

```text
Errors:
Install of module basic-cos-storage Failed

Error on Terraform Apply operation. Review log for cause of failure
Review job failure log yes/no [y/N]>
```
{: screen}

To help problem diagnosis reply `y` to the prompt to review the failure logs. The blueprint apply CLI command output includes the last few lines of the Terraform Apply job log.  As Terraform fails on an error, frequently the last few lines of the Terraform apply log includes sufficient information to identify the cause of the Terraform failure. 

If the log summary does not provide sufficient information, the `ibmcloud schematics logs` command to review the entire Terraform log for the failing Workspace is printed summary. 

```text
Status               FAILED   
Workspace Logs CLI   ibmcloud schematics logs --id eu-gb.workspace.basic-cos-storage.7378a905 --act-id a28612c58714c908a0d0c111df7cc74c 
```
{: screen}

See troubleshooting {{site.data.keyword.bpshort}} apply errors for additional information on debugging Terraform Apply failures  

## Blueprint install failure due to Terraform config coding error  
{: #bp-install-fails2}

When you run the blueprint install command, it fails with message that the install of module fails. 
{: tsSymptoms}

The analysis of the Workspace logs indicates that the modules Terraform config has a coding error, which caused the Terraform apply failure.  
{: tsCauses}

Correct the Terraform config error at source and push a new release to its Git source repository. 
{: tsResolve} 

If explicit version of the blueprint modules is used on specific branches. The blueprint template requires updating in its Git repository to specify the new release tag or branch for the module statement. 
- Update the blueprint module statements to specify the new module version. 
- Push the new release of the blueprint template to its Git source repository. With an updated release tag for the blueprint template if needed.

For modules, when no Git release is specified on the blueprint module statements and relaxed module version are used. No update to the blueprint template is needed. The current change to the module repository is pulled automatically by Schematics.  

Run the `ibmcloud schematics blueprint update` command to refresh the blueprint configuration that is stored by {{site.data.keyword.bpshort}} with the update to the blueprint template. With `latest` release, {{site.data.keyword.bpshort}} identifies the updated module Git repository and run the Pull-Latest to update any Workspaces with the modified Terraform configurations.  

```sh
ibmcloud schematics blueprint update -id <blueprint_id> 
```
{: pre}

If explicit blueprint version is used with release tags for each blueprint template release, the blueprint configuration must be updated in {{site.data.keyword.bpshort}} with the new blueprint release tag.  

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

When you run the blueprint install command, it fails with message that the install of module fails. 
{: tsSymptoms}

Analysis of the logs indicates that the modules Terraform apply operation that is timed out or a transient failure occurs. 
{: tsCauses}


No user action must be necessary to recover and the blueprint install operation can be retried. 
{: tsResolve} 

Run the `ibmcloud schematics blueprint install` command to rerun the failed Terraform Apply operation and complete all operations against all workspaces.  

```sh
ibmcloud schematics blueprint install -id <blueprint_id> 
```
{: pre}

## Blueprint install failures that require changes to values in input files  
{: #bp-install-fails4}

When you run the blueprint install command, it fails with message that the install of module fails. 
{: tsSymptoms}

Analysis of the Workspace logs indicates that the cause of the Terraform apply failure was due to an incorrect input value.  
{: tsCauses}

Update the input file source and push a new release to its Git source repository. 
{: tsResolve} 

If explicit input file version is used with release tags for each input file release, the blueprint configuration must be updated in {{site.data.keyword.bpshort}} with the new input file release tag.  

```sh
ibmcloud schematics blueprint update --id <blueprint_id> --input-git-release x.y.z  
```
{: pre}

Where no Git release is specified and relaxed module current version is used for input files in the blueprint config. No change to the blueprint config is needed and the input file changes are pulled in automatically by Schematics.  

Run the `ibmcloud schematics blueprint update` command to refresh the blueprint configuration with the changes. It updates the blueprint and Workspaces with the updated input values. 

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

When you run the blueprint install command, it fails with message that the install of module fails. 
{: tsSymptoms}

Analysis of the logs indicates that the cause of the Terraform apply failure was due to an incorrect dynamic input value  
{: tsCauses}

Identify the input value that must be updated and run the `ibmcloud schematics blueprint update` command to refresh the blueprint configuration with the change. It updates the blueprint and Workspaces with updated input value. Input values can be changed individually and all dynamic inputs do not have to be reentered. 

```sh
ibmcloud schematics blueprint update --id <blueprint_id> --inputs <name>=<value>
```
{: pre}

Finally, run the `ibmcloud schematics blueprint install` command to rerun the failed Terraform Apply operation with the updated dynamic inputs and complete all operations against all Workspaces.  

```sh
ibmcloud schematics blueprint install -id <blueprint_id> 
```
{: pre}
