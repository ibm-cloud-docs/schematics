---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-19"

keywords: get started with blueprints, infrastructure management, infrastructure as code, iac, schematics cloud environment, schematics infrastructure, schematics terraform, 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Getting started: Large-scale cloud environments in {{site.data.keyword.bplong_notm}}
{: #get-started-blueprints}

Use one of the {{site.data.keyword.IBM}} provided [samples](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external} to deploy a {{site.data.keyword.bpshort}} blueprint with multiple linked Workspaces from the command-line.
{: shortdesc}

{{site.data.keyword.bplong_notm}} blueprints support deploying and managing large-scale application environments using solution architecture definitions created from building blocks of open source IaC Code. Building on open source Infrastructure as Code (IaC) automation, blueprints scales infrastructure deployments by linking multiple {{site.data.keyword.bpshort}} Workspaces to create large-scale environments.  

## Before your begin
{: #get-started-blueprints-prereq}

- Install or update the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version that is greater than the `1.11.0` version.
- Select the {{site.data.keyword.cloud_notm}} region you want to use to manage your {{site.data.keyword.bpshort}}. For example, to set the region use [`ibmcloud target -r <region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command.
- Check that you have the right [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create blueprints.

## Select a blueprint template
{: #get-started-blueprints-select}
{: step}

Use one of the [sample blueprints](https://github.com/Cloud-Schematics/blueprint-complex-inputs){: external} to create a scalable blueprint cloud environment, created from multiple blueprint modules. 
{: shortdesc}

This sample blueprint is a simple scenario using two modules to demonstrate linking and resource data flows between the modules. No cloud resources are created by this example.  

## Creating a blueprint in {{site.data.keyword.bpshort}}
{: #get-started-blueprints-create}
{: step}

Create your blueprint by using the [`ibmcloud schematics blueprint config create`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command. 

For {{site.data.keyword.bpshort}} blueprints, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.11.0` version.
{: important}

### Syntax
{: #get-started-blueprints-syntax}

```sh
ibmcloud schematics blueprint config create --name Blueprint_Complex --resource-group Default --bp-git-url https://github.com/Cloud-Schematics/blueprint-complex-inputs --bp-git-file complex-blueprint.yaml --bp-git-branch main --input-git-url https://github.com/Cloud-Schematics/blueprint-complex-inputs --input-git-file complex-input.yaml --input-git-branch main --inputs region=eu-de,sample_var=testconfig_input_demo
```
{: pre}

### Output
{: #get-started-bp-create-output}

```text
Created blueprint ID: us-south.BLUEPRINT.blueprints-complex-inputs.17a2b552

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

Record the ID of the blueprint to use in the later commands.
{: important}

## Installing the blueprint to create cloud resources
{: #get-started-blueprints-install}
{: step}

Use the [`ibmcloud schematics blueprint run apply`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-apply) command to perform Terraform apply operations by using the Terraform configurations specified in the blueprint template. This operation will create cloud resources. Insert the ID saved from the [output of the create](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli#create-schematics-blueprint-cli) command.

```sh
ibmcloud schematics blueprint run apply -id <blueprint_ID>
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

In the UI view the provisioned {{site.data.keyword.cos_full_notm}} blueprint and Workspaces. 
- From the [{{site.data.keyword.bpshort}} blueprints list](https://cloud.ibm.com/schematics/blueprints){: external}, select the provisioned blueprint to view the created Workspaces and cloud resources. 

## Next steps
{: #get-started-blueprints-nextsteps}

You have now deployed a {{site.data.keyword.bpshort}} blueprint and created a multi-workspace environment.

Optionally, you can clean up the deployed blueprint with the following commands:

```sh
ibmcloud schematics blueprint run destroy -id <blueprint_ID>
```
{: pre}

You need to run the `blueprint run destroy` command and then run the  `blueprint config delete` command. For more information about the difference between destroy and config delete, see [Deleting a blueprint](/docs/schematics?topic=schematics-blueprint-delete).
{: note}

```sh
ibmcloud schematics blueprint config delete -id <blueprint_ID>
```
{: pre}

- Learn [about {{site.data.keyword.bpshort}} blueprints](/docs/schematics?topic=schematics-blueprint-intro).
- Looking for template samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/Cloud-Schematics/?q=topic:blueprint). 
