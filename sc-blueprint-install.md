---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-08"

keywords: blueprint install, install blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Installing a Blueprint
{: #install-blueprint}

Blueprint install is the second step required to create or modify cloud resources when using Blueprints. Install runs the IaC automation code for each workspace. For each module, {{site.data.keyword.bpshort}} performs a Terraform apply or Ansible playbook run to create or configure the specified cloud resources. 
{: shortdesc}

## Installing a Blueprint from the CLI 
{: #install-blueprint-cli}
{: cli}

To install and deploy your Blueprint with the CLI, use the `ibmcloud schematics blueprint install` command. This command requires a name and the Git URL of a Blueprint definition and other optional arguments. For a complete listing of options, see the [ibmcloud schematics blueprint install](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command.
{: shortdesc}

Before your begin

- Install and log in to the [{{site.data.keyword.cloud_notm}} command-line](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli).

The following command performs a Blueprint install for the Blueprint with the ID `eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936`

**Syntax:**

```sh
ibmcloud schematics blueprint install -id eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936
```
{: pre}

On successful completion the install command returns **fullfilment_success**. 

## Verify Blueprint install success 
{: #bp-verify-install}

Verify that the Blueprint has been installed successfully. When you install the Blueprint from the CLI, the command displays details of the Workspaces being installed and a continuously updating status of the progress of the {{site.data.keyword.bpshort}} jobs executing the IaC automation code. The command only returns on completion.

```text
Modules to be installed
SNO   Type        Name                   Status   
1     Terraform   basic-resource-group   INACTIVE   
2     Terraform   basic-cos-storage      INACTIVE   
      
Blueprint job running eu-gb.JOB.basic.f012ad25

Waiting:0    Draft:0    Connecting:0    InProgress:0    Inactive:0    Active:2    Failed:0   

Type        Name                   Status               Job ID   
Blueprint   basic                  FULFILMENT_SUCCESS   eu-gb.JOB.basic.f012ad25   
Terraform   basic-resource-group   ACTIVE                  
Terraform   basic-cos-storage      ACTIVE                  
            
Blueprint fulfilment_success at Tue May 31 11:44:12 BST 2022
OK
```
{: screen}

On successful completion the install command returns **fullfillment_success**.  

For more information, about how to diagnose and resolve issues if the install fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-install-fails).

## Next steps
{: #bp-install-nextsteps}

After installing the Blueprint, the cloud resources are now deployed. The resources can be located on the Blueprint `Resources` tab in the [Blueprints UI](https://cloud.ibm.com/schematics/blueprints){: external}, or through [Resource list](https://cloud.ibm.com/resources){: external}. 
{: shortdesc}

The configuration of the Blueprint and outputs can be reviewed using the `blueprint get` command. See section [Displaying Blueprints](/docs/schematics?topic=schematics-display-blueprint). 
