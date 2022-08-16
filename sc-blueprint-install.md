---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-16"

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

To install and deploy your Blueprint with the CLI, use the `ibmcloud schematics blueprint install` command. 
{: shortdesc}

Before your begin

- Install and log in to the [{{site.data.keyword.cloud_notm}} command-line](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli).

The following command performs a Blueprint install for the Blueprint with the ID `eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936`

For all the Blueprints commands, syntax, and detailed option flags, refer to, [Blueprints commands](/docs/schematics?topic=schematics-schematics-cli-reference#blueprints-cmd).
{: important}

**Syntax:**

```sh
ibmcloud schematics blueprint install -id eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936
```
{: pre}

On successful completion the install command returns **`fullfilment_success`**. 

### Verify Blueprint install success 
{: #bp-verify-install-cli}

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


## Creating a Blueprint from the UI 
{: #create-blueprint-ui}
{: ui}

Currently, you can only install a Blueprint from command-line by using the [install command](#install-blueprint-cli) to deploy cloud resources.

### Verify Blueprint install from the UI 
{: #bp-verify-install-ui}

1. Click your Blueprint that is listed in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external} to view the results of the install operation. 
2. Click **Overview** tab to see the Blueprint summary, including `Modules`, `Variables`, `Details`. The `Recent Job runs` should show the summary details of the Blueprint install job. 
3. Click **Modules** tab to see the status of the resource modules that are now in `Active` status.
4. Click **Resource** tab to view your provisioned resources list.
5. Click **Jobs history** tab view the result of the Blueprint install job and operations performed against the resource modules to deploy the resources.  


## Next steps
{: #bp-install-nextsteps}

After installing the Blueprint, the cloud resources are now deployed. The resources can be located on the Blueprint `Resources` tab in the [Blueprints UI](https://cloud.ibm.com/schematics/blueprints){: external}, or through [Resource list](https://cloud.ibm.com/resources){: external}. 
{: shortdesc}

The configuration of the Blueprint and outputs can be reviewed using the `blueprint get` command. See section [Displaying Blueprints](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-blueprint-get). 

Looking for additional Blueprint samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint). Check the example `Readme` files for further Blueprint customization and usage scenarios for each sample. 
