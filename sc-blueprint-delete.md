---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-08"

keywords: blueprint delete, delete blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Deleting a Blueprint
{: #delete-blueprint}

Blueprint delete is the second step required to completely delete a Blueprint from {{site.data.keyword.bpshort}}. To protect from accidental deletion, a Blueprint can only be deleted when cloud resources in all the linked Workspaces have been deleted and the Workspaces are in `Inactive` state.  

This behaviour can be modified by using the `-force-delete` flag to allow deletion when Workspaces cannot be returned to an Inactive state. 


```sh
ibmcloud schematics blueprint delete -id us-east.ENVIRONMENT.Blueprints-Starter-Sample.c579f31d
```
{: pre}

For more information, about the command options, see the [delete command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-delete).


```text
Modules to be deleted
SNO   Type        Name                   Status   
1     Workspace   basic-resource-group   INACTIVE   
2     Workspace   basic-cos-storage      INACTIVE   
      
Do you really want to delete the blueprint ? [y/N]> y
Job : eu-gb.JOB.Blueprint-Basic-Example.f2d388d3 Created

Job Type: BLUEPRINT DELETE

OK
```
{: screen}


## Verify Blueprint delete success 
{: #bp-verify-delete}

During the beta, the delete CLI command does not wait for successful job completion and returns immediately. 

The status of the delete operation can be monitored using the `blueprint job get` command. The following command performs a Blueprint `job get` for the JOB ID `eu-gb.JOB.Blueprint-Basic-Example.f2d388d3`. The job ID will be displayed in the delete command output. 

```sh
ibmcloud schematics blueprint job get -id eu-gb.JOB.Blueprint-Basic-Example.f2d388d3
```
{: pre}


```text
ID                      eu-gb.JOB.Blueprint-Basic-Example.f2d388d3   
Blueprint ID            eu-gb.ENVIRONMENT.Blueprint-Basic-Example.d39591ae   
Job Type                blueprint_delete   
Location                eu-gb   
Start Time              2022-07-02 09:33:40   
End Time                0001-01-01 00:00:00   
Status                  Normal   
                           
OK
```
{: screen}

The Blueprint and all of its cloud resources are now deleted. 



