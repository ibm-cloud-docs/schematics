---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-06"

keywords: blueprint destroy, destroy blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Destroying a Blueprint
{: #destroy-blueprint}

The cloud resources created by a Blueprint are destroyed using the `blueprint destroy` command. If it is then needed to remove the Blueprint from Schematics, this is performed after all resources have been destroyed using the [blueprint delete](/docs/schematics?topic=schematics-sc-blueprint-delete) command. Refer to [Blueprints lifecycle](https://test.cloud.ibm.com/docs/schematics?topic=schematics-blueprint-lifecycle-cmds) to understand the role of the Blueprint commands create, update and delete and the Blueprints lifecycle. 

For Terraform Workspaces, destroy runs a Terraform destroy operation against each Workspace in turn. This removes all cloud resources in reverse dependency order.    

The following command performs a Blueprint destroy for the Blueprint with the ID `eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936`

```sh
ibmcloud schematics blueprints destroy -id eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936
```
{: pre}

On successful completion the destroy command will return **fullfilment_success**. 

For more information, about the command options, see [Destroy command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-destroy).



## Verfiy Blueprint destroy success 
{: #bp-verify-destroy}

Verify that the Blueprint resources have been destroyed successfully. When you run destroy from the CLI, the command displays details of the Workspaces to be destroyed and the status of {{site.data.keyword.bpshort}} jobs executing the Terraform destroy operations. After prompting to confirm that the user intends to destory all resources, the command only returns on completion.

```text
Modules to be destroyed
SNO   Type        Name                   Status   
1     Workspace   basic-resource-group   ACTIVE   
2     Workspace   basic-cos-storage      ACTIVE   
      
Do you really want to destroy all the resources of blueprint eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936? [y/N]> y
Blueprint job running eu-de.JOB.Blueprint-Basic-Example.62942aa1

Waiting:0    Draft:0    Connecting:0    In Progress:0    Inactive:2    Active:0    Failed:0   
Type        Name                      Status               Job ID   
Blueprint   Blueprint Basic Example   FULFILMENT_SUCCESS   eu-de.JOB.Blueprint-Basic-Example.62942aa1   
Workspace   basic-resource-group      INACTIVE                
Workspace   basic-cos-storage         INACTIVE                
            
Blueprint ID eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936 fulfilment_success at Mon Jun 27 16:43:30 BST 2022
OK
```
{: screen}

On successful completion the destroy command will return **fullfillment_success**.

Successful command completion and the status of the Workspaces as `Inactive` indicates that resources in all linked Workspaces have been destroyed


## Next steps
{: #bp-destroy-nextsteps}

After the cloud resources are destroyed, the Blueprint can be [deleted](/docs/schematics?topic=schematics-sc-blueprint-delete) from {{site.data.keyword.bpshort}}. Alternatively the cloud environment can be re-constituted and the resources re-created by running [blueprint install](/docs/schematics?topic=schematics-sc-blueprint-install) again using the same Blueprint configuration. 

