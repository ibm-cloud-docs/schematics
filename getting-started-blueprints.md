---

copyright:
  years: 2017, 2023
lastupdated: "2023-01-04"

keywords: get started with blueprints, infrastructure management, infrastructure as code, iac, schematics cloud environment, schematics infrastructure, schematics terraform, 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Using Blueprints to deploy large-scale cloud environments 
{: #get-started-blueprints}

Use one of the {{site.data.keyword.IBM}} provided [samples](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external} to deploy a blueprint linking multiple module environments. 
{: shortdesc}

{{site.data.keyword.bplong_notm}} Blueprints supports deploying and managing large-scale application environments using solution architecture definitions composed from building blocks of open source IaC Code. Building on open source Infrastructure as Code (IaC) automation, Blueprints scales infrastructure deployments by linking multiple environments to create large-scale application architectures.   

## Deploy blueprint through UI
{: #deploy-bp-ui}
{: ui}


### Creating a blueprint in {{site.data.keyword.bpshort}}
{: #get-started-blueprints-create-ui}
{: step}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Click on **Schematics** in the left hand navigator pane, then click **Blueprints** 
3. On the Blueprints dashboard page, click the **Create Blueprint** button
    - In **Blueprint Details** section:
        - **Name** `<Provide unique name for your blueprint>`.
        - **Location** as `North America` or other [region](/docs/schematics?topic=schematics-multi-region-deployment) for this blueprint.
        - **Resource group** as `Default` or other resource group for this blueprint. For more information, see [Creating a resource group](/docs/account?topic=account-rgs). Ensure you have right access permission for the resource group.
        - **Tags** as `<Provide the tag name for your blueprint>`.
        - **Description** for the blueprint. Supports maximum character range from `0 - 2048`.
        - Click **Next**.
    - In **Blueprint URL** section:
        - **Repository URL** - `<Provide your valid GitHub or GitLab repository URL that hosts your blueprint configuration file>`. For example, `https://github.com/Cloud-Schematics/blueprint-basic-example/blob/main/basic-blueprint.yaml`. Review the [blueprint URL FAQ](/docs/schematics?topic=schematics-blueprints-faq#faqs-bp-url) for details of the URL format. 
        - **Personal access token** - `<Provide your Git personal access token, only for private Git repos>`.
        - Check the information that is entered are correct to create a blueprint.
        - Click **Next and save as draft**. Observe that a blueprint is created with a Blueprint ID and is in `Draft` Status.
           Validation takes a few seconds to fetch the template details from the Git repo. 
           {: note}

    - In **Input Variables** section:
        - Select **Import input file** drop down only when you want to import the new `inputs` YAML file for the blueprint.
            - In **Import input file (Optional)** section:
               -  **Input file GIT URL** - `<Provide your valid GitHub or GitLab repository URL that hosts your blueprint configuration file>`. For example, `https://github.com/Cloud-Schematics/blueprint-basic-example/blob/main/basic-input.yaml`. Review the [blueprint URL FAQ](/docs/schematics?topic=schematics-blueprints-faq#faqs-bp-url) for details of the URL format. 
               - **Source name** - Used to set the source name the input file values are identified by in the UI. 
               - **Personal access token** - `<Provide your Git personal access token, only for private Git repos>`.
               - Click **Import values**.
        - Observe that the input variables from the `inputs.yaml` file are imported. Optionally, you can edit the variables.
           Enter variable values into the table by typing them in or by importing them. Prefilled default values, if any, were pulled from the blueprint template, but can be changed. If there is a dropdown, select a value from the dropdown.
           {: important}

        - Click **Done editing**, if the editing is done.
        - Click **Save draft** only if you need to save the draft to continue edit the input variables again later.
4. Click **Create Blueprint** that will redirect to the blueprint overview page. 

### Applying the blueprint configuration to create cloud resources
{: #get-started-blueprints-apply-ui}
{: step}

You can follow these steps to generate a plan and apply a blueprint using the {{site.data.keyword.cloud_notm}} console.

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Click **Schematics** > **Blueprints**.
3. Click your blueprint that is listed in the [Blueprints dashboard](https://cloud.ibm.com/schematics/blueprints){: external} 
4. Click **Generate plan** to initialize the modules that are defined in the blueprint template
    
    Generate plan execution can take few a minutes to execute. Once generated check if the plan is correct. You can see the **Resource summary**, and **Jobs history** that displays the `blueprint_create_init` and the respective module job details. If **Generate plan** fails, review the job logs to identify the cause of the failure. As required to resolve the failure, modify the template and update the blueprint configuration with the revised template and any input values. Then re-run Generate Plan.
    {: note}

5. Click **Apply plan** to provision the resources configured in your modules. The blueprint will show an `In progress` status.
    
    The apply plan execution takes a few minutes based on the resources. The execution jobs show the history of all blueprint, module activities, and the logs of the jobs. If **Apply plan** fails, review the module job logs for information relating to the Terraform execution errors. As required to resolve the failure, modify the template and update the blueprint configuration with the revised template and any input values. Then re-run Generate Plan.
    {: note}

### Displaying the blueprint
{: #get-started-bp-list-ui}
{: step}

1. From the [Blueprints dashboard](https://cloud.ibm.com/schematics/blueprints){: external}. Click your blueprint name to view the blueprint details.
2. Click **Overview** to view the blueprint summary, that includes `Modules status`, `Variables summary`, `Blueprint Details` and `Recent Job runs` of your blueprint. 
    - Optional: From the **Modules status** card, Click **View details** to navigate to the module details page.  
    - Optional: From **Variables summary** card, Click **View details** to navigate to the variable summary page.
3. Click **Modules** tab to see the list of blueprint modules and their current status. 
    - Optional: Click **Show details** to view more details about the blueprint module definition. 
    - Optional: Click **Name** that will take you to the modules related Schematics `Workspace` page. 
4. Click **Resources** tab to view the list of resources provisioned by the blueprint.
5. Click **Variables** tab to view the blueprint **Inputs** and **Outputs** variables and values. Optional: on this page you can edit the input variable values and save. 
6. Click **Jobs history** tab view the job logs of the blueprint and module jobs.
7. Click **Settings** tab to view and edit the configuration settings of the blueprint.

### Next steps
{: #get-started-blueprints-nextsteps-ui}

You have now deployed a blueprint and created a multi-module environment. Optionally, you can clean up the deployed blueprint by using [destroy](/docs/schematics?topic=schematics-destroy-blueprint#destroy-blueprint-ui) to remove the cloud resources and [delete](/docs/schematics?topic=schematics-delete-blueprint#delete-blueprint-ui) to remove the blueprint config.

For more information about the difference between destroy and config delete, see [Deleting a blueprint](/docs/schematics?topic=schematics-delete-blueprints).
{: note}

- Learn [about {{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-blueprint-intro).
- Looking for template samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories?q=blueprint&type=all&language=&sort=){: external}. 

## Deploy a blueprint using the CLI
{: #deploy-bp-cli}
{: cli}

### Before your begin
{: #get-started-blueprints-prereq}

- Install or update the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version that is greater than the `1.12.5` version.
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

Create your blueprint by using the [`ibmcloud schematics blueprint create`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-create) command. 

For {{site.data.keyword.bpshort}} Blueprints, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.12.5` version.
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

In the `blueprint create` command, replace `DEFAULT-RESOURCE-GROUP-NAME` with the name of the default resource group. 


```sh
ibmcloud schematics blueprint create --name Blueprint_Complex --resource-group DEFAULT-RESOURCE-GROUP-NAME --bp-git-url https://github.com/Cloud-Schematics/blueprint-complex-inputs --bp-git-file complex-blueprint.yaml --bp-git-branch main --input-git-url https://github.com/Cloud-Schematics/blueprint-complex-inputs --input-git-file complex-input.yaml --input-git-branch main --inputs region=eu-de,sample_var=testconfig_input_demo
```
{: pre}

#### Output
{: #get-started-bp-create-output}

```text
Created blueprint ID: Blueprint_Complex.eaB.5670

Modules to be created
SNO   Module Type   Name   
1     Workspace     terraform_module1   
2     Workspace     terraform_module2   
      
Blueprint job us-east.JOB.Blueprint_Complex.eeb09213 started at 2022-11-24 09:58:46

Module job execution status
Waiting:0    In Progress:0    Success:2    Failed:0   

Blueprint job us-east.JOB.Blueprint_Complex.eeb09213 completed at 2022-11-24 10:00:14

Module Type   Name                Status           Job ID   
Blueprint     Blueprint_Complex   CREATE_SUCCESS   us-east.JOB.Blueprint_Complex.eeb09213   
Workspace     terraform_module1   INITIALISED         
Workspace     terraform_module2   INITIALISED         
              
Blueprint ID Blueprint_Complex.eaB.5670 create_success at 2022-11-24 10:00:15
OK
```
{: screen}

Record the ID of the blueprint to use in the later commands.
{: important}

### Applying the blueprint to create cloud resources
{: #get-started-blueprints-install}
{: step}

Use the [`ibmcloud schematics blueprint apply`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-blueprint-apply) command to perform Terraform apply operations by using the Terraform configurations specified in the blueprint template. This operation will create cloud resources. Insert the ID saved from the [output of the create](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli#create-schematics-blueprint-cli) command.

```sh
ibmcloud schematics blueprint apply -id <blueprint_ID>
```
{: pre}

#### Output
{: #get-started-bp-install-output}

```text
Modules to be applied
SNO   Module Type   Name                Status   
1     Workspace     terraform_module1   INITIALISED   
2     Workspace     terraform_module2   INITIALISED   
      
Blueprint job us-east.JOB.Blueprint_Complex.23a60fdb started at 2022-11-24 10:03:19

Module job execution status
Waiting:0    In Progress:0    Success:2    Failed:0   

Blueprint job us-east.JOB.Blueprint_Complex.23a60fdb completed at 2022-11-24 10:05:57

Module Type   Name                Status               Job ID   
Blueprint     Blueprint_Complex   job_finished   us-east.JOB.Blueprint_Complex.23a60fdb   
Workspace     terraform_module1   APPLIED                 
Workspace     terraform_module2   APPLIED                 
              
Blueprint ID Blueprint_Complex.eaB.5670 job_finished at 2022-11-24 10:05:58
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
ibmcloud schematics blueprint destroy -id <blueprint_ID>
```
{: pre}

You need to run the `blueprint destroy` command and then run the  `blueprint delete` command. For more information about the difference between destroy and config delete, see [Deleting a blueprint](/docs/schematics?topic=schematics-delete-blueprints).
{: note}

```sh
ibmcloud schematics blueprint delete -id <blueprint_ID>
```
{: pre}

- Learn [about {{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-blueprint-intro).
- Looking for template samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories?q=blueprint&type=all&language=&sort=){: external}. 
