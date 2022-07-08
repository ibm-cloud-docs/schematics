---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-08"

keywords: get started with blueprints, infrastructure management, infrastructure as code, iac, schematics cloud environment, schematics infrastructure, schematics terraform, 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Getting started with large-scale cloud environments in {{site.data.keyword.bplong_notm}}
{: #get-started-blueprints}

Use one of the {{site.data.keyword.IBM}} provided [samples](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint) to deploy a {{site.data.keyword.bpshort}} Blueprint with multiple linked Workspaces from the command-line.
{: shortdesc}

{{site.data.keyword.bplong_notm}} Blueprints support deploying and managing large-scale application environments using solution architecture definitions created from building blocks of open source IaC Code. Building on open source Infrastructure as Code (IaC) automation, Blueprints scales infrastructure deployments by linking multiple {{site.data.keyword.bpshort}} Workspaces to create large-scale environments.  

## Before your begin
{: #get-started-blueprints-prereq}

- Install and login to the [{{site.data.keyword.cloud_notm}} command-line](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli).
- Select the {{site.data.keyword.cloud_notm}} region you wish to use to manage your {{site.data.keyword.bpshort}} Blueprints environment from. Set the region by running the command `ibmcloud target --region <us-south>`.
- Install the [{{site.data.keyword.bpshort}} command line](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) plug-in, or [update the command line plug-in](/docs/schematics?topic=schematics-setup-cli#schematics-cli-update) to access the {{site.data.keyword.bpshort}} Blueprints commands.
- Check that you have the right [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create Blueprints.


## Select a Blueprint definition
{: #get-started-blueprints-select}
{: step}

Use one of the [sample Blueprints](https://github.com/Cloud-Schematics/blueprint-complex-inputs){: external} to create a scalable Blueprint cloud environment containing multiple {{site.data.keyword.bpshort}} Workspaces. 
{: shortdesc}

This sample Blueprint is a simple scenario creating two linked Workspaces and resource data flows between the Workspaces. No cloud resources are created by this example.  

## Creating a Blueprint environment in {{site.data.keyword.bpshort}}
{: #get-started-blueprints-create}
{: step}

Create your Blueprints environment by using the [`ibmcloud schematics blueprint create`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-blueprint-create) command. 

### Syntax
{: #get-started-blueprints-syntax}

```sh
ibmcloud schematics blueprint create --name Blueprint_Complex --resource-group Default --bp-git-url https://github.com/Cloud-Schematics/blueprint-complex-inputs --bp-git-file complex-blueprint.yaml --bp-git-branch main --input-git-url https://github.com/Cloud-Schematics/blueprint-complex-inputs --input-git-file complex-input.yaml --input-git-branch main --inputs region=eu-de,sample_var=testconfig_input_demo
```
{: pre}

### Output
{: #get-started-bp-create-output}

```text
Created Blueprint ID: us-south.BLUEPRINT.blueprints-complex-inputs.17a2b552

Modules to be created
SNO   Type        Name   
1     Workspace   terraform_module1   
2     Workspace   terraform_module2   
      
Blueprint job running us-south.JOB.blueprints-complex-inputs.9dec1c46

Waiting:0    Draft:0    Connecting:0    In Progress:0    Inactive:2    Active:0    Failed:0   

Type        Name                        Status           Job ID   
Blueprint   blueprints-complex-inputs   CREATE_SUCCESS   us-south.JOB.blueprints-complex-inputs.9dec1c46   
Workspace   terraform_module1           INACTIVE            
Workspace   terraform_module2           INACTIVE            
            
Blueprint ID us-south.BLUEPRINT.blueprints-complex-inputs.17a2b552 create_success at Mon Jul  4 13:49:07 BST 2022
```
{: screen}

Record the ID of the Blueprint to use in the later commands.
{: important}

## Installing the Blueprint to create cloud resources
{: #get-started-blueprints-install}
{: step}

Use the [`ibmcloud schematics blueprint install`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-blueprint-install) command to perform Terraform apply operations by using the Terraform configurations specified in the Blueprint definition. This operation will create cloud resources. Insert the ID saved from the [output of the create](#create-schematics-blueprint) command.

```sh
ibmcloud schematics blueprint install -id <blueprint_id>
```
{: pre}

### Output
{: #get-started-bp-install-output}

```text
Modules to be installed
SNO   Type        Name                Status   
1     Workspace   terraform_module1   INACTIVE   
2     Workspace   terraform_module2   INACTIVE   
      
Blueprint job running eu-gb.JOB.blueprints-complex-inputs.44624c19

Waiting:0    Draft:0    Connecting:0    In Progress:0    Inactive:0    Active:2    Failed:0   

Type        Name                        Status               Job ID   
Blueprint   blueprints-complex-inputs   FULFILMENT_SUCCESS   eu-gb.JOB.blueprints-complex-inputs.44624c19   
Workspace   terraform_module1           ACTIVE                  
Workspace   terraform_module2           ACTIVE                  
            
Blueprint ID eu-gb.BLUEPRINT.blueprints-complex-inputs.7312c775 fulfilment_success at Mon Jul  4 14:01:58 BST 2022
OK
```
{: screen}

In the UI view the provisioned {{site.data.keyword.cos_full_notm}} Blueprint and Workspaces. 
- From the [{{site.data.keyword.bpshort}} Blueprints list](https://schematics.test.cloud.ibm.com/blueprints){: external}, select the provisioned Blueprint to view the created Workspaces and cloud resources. 

## Next steps
{: #get-started-blueprints-nextsteps}

You have now deployed a {{site.data.keyword.bpshort}} Blueprint and created a multi-workspace environment.

Clean up the deployed Blueprint with the following commands:

```sh
ibmcloud schematics blueprint destroy -id <blueprint_id>
ibmcloud schematics blueprint delete -id <blueprint_id>
```
{: pre}

- Learn [about {{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-blueprint-intro&interface=ui).
- Looking for more samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/Cloud-Schematics/?q=topic:blueprint). 
