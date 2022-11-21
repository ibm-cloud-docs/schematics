---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-21"

keywords: get started with blueprints, infrastructure management, infrastructure as code, iac, schematics cloud environment, schematics infrastructure, schematics terraform, 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Getting started: Using blueprints to deploy large-scale cloud environments 
{: #get-started-blueprints}

Use one of the {{site.data.keyword.IBM}} provided [samples](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external} to deploy a blueprint with multiple linked modules.
{: shortdesc}

{{site.data.keyword.bplong_notm}} Blueprints support deploying and managing large-scale application environments using solution architecture definitions created from building blocks of open source IaC Code. Building on open source Infrastructure as Code (IaC) automation, blueprints scales infrastructure deployments by linking multiple workspaces to create large-scale environments.  

## Deploy blueprint through UI
{: #deploy-bp-ui}
{: ui}

### Before your begin
{: #get-started-blueprints-prereq-ui}

- Select the {{site.data.keyword.cloud_notm}} region you want to use to manage your blueprints. For example, to set the region use [`ibmcloud target -r <region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command.
- Check that you have the right [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create blueprints.

### Creating a blueprint in {{site.data.keyword.bpshort}}
{: #get-started-blueprints-create-ui}
{: step}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Click **Schematics** > **Blueprints** > **Create Blueprint**.
    - In **Blueprint Details** section:
        - **Name** `<Provide unique name for your blueprint>`.
        - **Location** as `North America` or other [region](/docs/schematics?topic=schematics-multi-region-deployment) for this blueprint.
        - **Resource group** as `Default` or other resource group for this blueprint. For more information, see [Creating a resource group](/docs/account?topic=account-rgs). Ensure you have right access permission for the resource group.
        - **Tags** as `<Provide the tag name for your blueprint>`.
        - **Description** for the blueprint. Supports maximum character range from `0 - 2048`.
        - Click **Next**.
    - In **Blueprint URL** section:
        - **Repository URL** - `<Provide your valid GitHub, GitLab or Bitbucket repository URL that hosts your blueprint configuration file>`. For example, `https://github.com/Cloud-Schematics/blueprint-basic-example`.
        - **Personal access token** - `<Provide your Git personal access token, only for private Git repos>`.
        - Check the information that is entered are correct to create a blueprint.
        - Click **Next and save as draft**. Observe the Blueprint ID is created and is in `Draft` Status.
           Validation takes few seconds to fetch the input variables from the blueprint configuration file.
           {: note}

    - In **Input Variables** section:
        - Observe that the input variables from the blueprint `.yaml` file are imported. Optionally, you can edit the variables.
           Enter variable values into the table by typing them in or by importing them. Prefilled values if any were pulled from the Blueprint definition, but can be changed. If there is a dropdown, select value from the dropdown.
           {: important}

        - Click **Done editing**, if the editing is done.
        - Click **Save draft** only if you need to edit the input variables.
        - Optional select **Import input file** drop down only when you want to import the new `.yaml` file for the blueprint.
            - In **Import input file (Optional)** section:
               -  **Input file GIT URL** - `<Provide your valid GitHub, GitLab or Bitbucket repository URL that hosts your blueprint configuration file>`. For example, `https://github.com/Cloud-Schematics/blueprint-basic-example/blob/main/basic-blueprint.yaml`.
               - **Source name** - Used to display from which source name the value is imported.
               - **Personal access token** - `<Provide your Git personal access token, only for private Git repos>`.
               - Click **Import values**.
3. Click **Create Blueprint** that redirects to your blueprint page. 

### Applying the blueprint to create cloud resources
{: #get-started-blueprints-apply-ui}
{: step}

You can follow these steps to generate plan and apply the {{site.data.keyword.bpshort}} Blueprints by using {{site.data.keyword.cloud_notm}} console.

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Click **Schematics** > **Blueprints**.
3. Select your `<Blueprint_ID>`.
4. Click **Generate plan** to initiate the modules that are configured in the blueprint.
    
    Generate plan execution takes few seconds. Once generated check if the plan is correct. You can see the **Resource summary**, and **Jobs history** that displays the `blueprint_create_init` and the respective module job details. If **Generate plan** fails, you can update the blueprint.
    {: note}

5. Click **Apply plan** to provision the resources configured in your modules. You can observe the `In progress` status.

     The apply plan exection takes few minutes based on the resources. The execution jobs show history of all blueprint, module activities, and the logs of the jobs. If **Apply plan** fails, you can update the blueprint.
     {: note}

### Listing the blueprint
{: #get-started-bp-list-ui}
{: step}

You can follow these steps to list the {{site.data.keyword.bpshort}} Blueprints by using {{site.data.keyword.cloud_notm}} console.

1. From the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/schematics/blueprints){: external}. Click your blueprint name to view the blueprint details.
2. Click **Overview** to view the blueprint summary, that includes `Modules status`, `Variables summary`, `Blueprint Details` and `Recent Job runs` of your blueprint. 
    - Optional: From **Modules status** section, Click **View details** to view the module details.
    - Optional: From **Variables summary** section, Click **View details** to view the variable summary.
3. Click **Modules** tab to see the list of modules and their current status. 
    - Optional: Click **Show details** to view the module details.
    - Optional: Click **Name** that takes to the modules `Workspace` page. 
4. Click **Resources** tab to view the list of resources provisioned status by the blueprint.
5. Click **Variables** tab to view your **Inputs** and **Outputs** variables and values. Optional: you can edit the input variable and click **Save variables**.
6. Click **Jobs history** tab view the job logs of the blueprint and module activities.
7. Click **Settings** tab to view the configuration settings of the blueprint.

### Next steps
{: #get-started-blueprints-nextsteps-ui}

You have now deployed a blueprint and created a multi-workspace environment. Optionally, you can clean up the deployed blueprint by using [destroy](/docs/schematics?topic=schematics-destroy-blueprint#destroy-blueprint-ui) the resources and [delete](/docs/schematics?topic=schematics-delete-blueprint#delete-blueprint-ui) the blueprint.

For more information about the difference between destroy and config delete, see [Deleting a blueprint](/docs/schematics?topic=schematics-delete-blueprints).
{: note}

- Learn [about {{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-blueprint-intro).
- Looking for template samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories?q=blueprint&type=all&language=&sort=){: external}. 

## Deploy blueprint through CLI
{: #deploy-bp-cli}
{: cli}

### Before your begin
{: #get-started-blueprints-prereq}

- Install or update the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version that is greater than the `1.12.3` version.
- Select the {{site.data.keyword.cloud_notm}} region you want to use to manage your blueprints. For example, to set the region use [`ibmcloud target -r <region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command.
- Check that you have the right [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create blueprints.

### Select a blueprint template
{: #get-started-blueprints-select}
{: step}

Use one of the [sample blueprints](https://github.com/Cloud-Schematics/blueprint-complex-inputs){: external} to create a scalable blueprint cloud environment, created from multiple blueprint modules. 
{: shortdesc}

This sample blueprint is a simple scenario using two modules to demonstrate linking and resource data flows between the modules. No cloud resources are created by this example.  

### Creating a blueprint in {{site.data.keyword.bpshort}}
{: #get-started-blueprints-create}
{: step}

Create your blueprint by using the [`ibmcloud schematics blueprint config create`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command. 

For {{site.data.keyword.bpshort}} Blueprints, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.12.3` version.
{: important}

#### Syntax
{: #get-started-blueprints-syntax}

The name of default resource group for the account is required for the blueprint creation. The blueprint config will be created in this resource group. 

```sh
ibmcloud resource groups --default

Retrieving default resource group under account 12345678901234567890123456789012 as anon@anon.com...
OK
Name      ID                                 Default Group   State
Default   aac37f57b20142dba1a435c70aeb12df   true            ACTIVE
```
{: pre}

In the `blueprint config create` command, replace `<default-resource-group-name>` with the name of the default resource group. 


```sh
ibmcloud schematics blueprint config create --name Blueprint_Complex --resource-group <default-resource-group-name> --bp-git-url https://github.com/Cloud-Schematics/blueprint-complex-inputs --bp-git-file complex-blueprint.yaml --bp-git-branch main --input-git-url https://github.com/Cloud-Schematics/blueprint-complex-inputs --input-git-file complex-input.yaml --input-git-branch main --inputs region=eu-de,sample_var=testconfig_input_demo
```
{: pre}

#### Output
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

### Applying the blueprint to create cloud resources
{: #get-started-blueprints-install}
{: step}

Use the [`ibmcloud schematics blueprint run apply`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-apply) command to perform Terraform apply operations by using the Terraform configurations specified in the blueprint template. This operation will create cloud resources. Insert the ID saved from the [output of the create](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli#create-schematics-blueprint-cli) command.

```sh
ibmcloud schematics blueprint run apply -id <blueprint_ID>
```
{: pre}

#### Output
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

In the UI view the provisioned blueprint and modules. 
- From the [Blueprints list](https://cloud.ibm.com/schematics/blueprints){: external}, select the provisioned blueprint to view the created  and cloud resources. 

### Next steps
{: #get-started-blueprints-nextsteps}

You have now deployed a Blueprint and created a multi-workspace environment.

Optionally, you can clean up the deployed blueprint with the following commands:

```sh
ibmcloud schematics blueprint run destroy -id <blueprint_ID>
```
{: pre}

You need to run the `blueprint run destroy` command and then run the  `blueprint config delete` command. For more information about the difference between destroy and config delete, see [Deleting a blueprint](/docs/schematics?topic=schematics-delete-blueprints).
{: note}

```sh
ibmcloud schematics blueprint config delete -id <blueprint_ID>
```
{: pre}

- Learn [about {{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-blueprint-intro).
- Looking for template samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories?q=blueprint&type=all&language=&sort=){: external}. 
