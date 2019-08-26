---

copyright:
  years: 2017, 2019
lastupdated: "2019-08-26"

keywords: Schematics, automation, Terraform

subcollection: schematics

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}

# Setting up the CLI 
{: #setup-cli}

Use the {{site.data.keyword.cloud_notm}} Schematics CLI plug-in to create and manage your Schematics workspaces, and to provision and mange your resources in {{site.data.keyword.cloud_notm}}. 
{: shortdesc}


## Installing the IBM Cloud CLI and the IBM Cloud Schematics CLI plugin
{: #install-schematics-cli}

Install the required CLIs to automate the provisioning of {{site.data.keyword.cloud_notm}} resources across environments. 
{:shortdesc}

1. Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started#idt-prereq). This installation includes: 
   -   {{site.data.keyword.cloud_notm}} CLI (`ibmcloud`)
   -   Several CLI plug-ins, such as for {{site.data.keyword.containerlong_notm}} (`ibmcloud ks`), {{site.data.keyword.registryshort_notm}} plug-in (`ibmcloud cr`), and {{site.data.keyword.cloud_notm}} Functions (`ibmcloud fn`)
   -   Other CLIs, such as Helm (`helm`), Docker (`docker`), Kubernetes (`kubectl`), Git (`git`), and Homebrew (`brew`)

   Plan to use the CLI often? Try [Enabling shell autocompletion for {{site.data.keyword.cloud_notm}} CLI (Linux/MacOS only)](/docs/cli/reference/ibmcloud?topic=cloud-cli-shell-autocomplete#shell-autocomplete-linux).
   {: tip}

2. Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.
   ```
   ibmcloud login
   ```
   {: pre}

   If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
   {: tip}
    
3. Install the {{site.data.keyword.cloud_notm}} CLI plug-in for Schematics. 
   ```
   ibmcloud plugin install schematics
   ```
   {: pre}
    
4. Verify that the {{site.data.keyword.cloud_notm}} Schematics CLI plug-in is installed successfully. The plug-in is listed as `schematics`. 
   ```
   ibmcloud plugin list
   ```
   {: pre}

   Example output:
   ```
   Listing installed plug-ins...

   Plugin Name         Version   Status        
   schematics          1.4.0     
   ```
   {: screen}
    
5. Verify that you can use the {{site.data.keyword.cloud_notm}} Schematics CLI plug-in by listing all supported commands. The command prefix to work with the Schematics CLI plug-in is `ibmcloud terraform`. 
   ```
   ibmcloud terraform help
   ```
   {: pre}
    
   Example output: 
   ```
   NAME:
   ibmcloud terraform - IBM Cloud Terraform plug-in

   USAGE:
   ibmcloud terraform command [arguments...] [command options]

   COMMANDS:
   --------------------------------------------------------------------------------------------------------------------------    ---------------------------------------------------------------------------------------

   apply       Apply a plan to an workspace to deploy the latest version of your configuration.
   destroy     Destroy resources in an existing workspace. This action cannot be reversed.
   info        Show information about the CLI and IBM Cloud Terraform service.
   plan        Create a plan for an workspace. Plans show how resources would change if you applied the latest version of your workspace configuration.
   version     Report version information about the IBM Cloud Terraform CLI.
   workspace   Create and manage workspaces. workspaces let you define the source control repository that contains your Terraform configuration and pass workspace-specific variables.
   help, h     Show help

   Enter 'ibmcloud terraform help [command]' for more information about a command.
   ```
   {: screen}
   
## Updating the CLI
{: schematics-cli-update}

Update the {{site.data.keyword.cloud_notm}} CLI and the {{site.data.keyword.cloud_notm}} Schematics CLI plug-in periodically to get access to new features. 
{: shortdesc}

1.  Update the {{site.data.keyword.cloud_notm}} CLI. Download the [latest version ![External link icon](../icons/launch-glyph.svg "External link icon")](/docs/cli?topic=cloud-cli-getting-started) and run the installer.

2. Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.

    ```
    ibmcloud login
    ```
    {: pre}

     If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
     {: tip}

3. Check if an update is available for the {{site.data.keyword.cloud_notm}} Schematics CLI plug-in. If an update is available, you find an **Update available** notification in your CLI output. 
   ```
   ibmcloud plugin list | grep schematics
   ```
   {: pre}
   
   Example output: 
   ```
   schematics                      1.4.0        Update available
   ```
   {: screen}
   
4. Update the {{site.data.keyword.cloud_notm}} Schematics CLI plug-in. 
   ```
   ibmcloud plugin update schematics
   ```
   {: pre}
