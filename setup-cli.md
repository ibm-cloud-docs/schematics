---

copyright:
  years: 2017, 2022
lastupdated: "2022-01-31"

keywords: schematics CLI, schematics command-line, schematics commands, terraform commands, terraform CLI, setting up schematics CLI, cli

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Setting up the CLI 
{: #setup-cli}

Use the {{site.data.keyword.bplong_notm}} command-line plug-in to automate the infrastructure provisioning process, the configuration of your {{site.data.keyword.cloud_notm}} resources, and the deployment of app workloads in {{site.data.keyword.cloud_notm}}. The {{site.data.keyword.bpfull}} command-line extends the support of following multiple platform architectures inline with the {{site.data.keyword.cloud}} command line interface (CLI).

- Mac OS X 64-bit
- Windows&trade; 64-bit
- Windows&trade; 32-bit
- Linux&trade; 64-bit
- Linux&trade; 32-bit
- PowerLinux&trade; 64-bit
- System/390 Linux&trade; 64-bit

## Installing the IBM Cloud command-line 
{: #install-schematics-cli}

Install the required command-line to start using {{site.data.keyword.bplong_notm}}.  
{: shortdesc}

1. Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started). 

    Plan to use the command-line often? Try [Enabling shell autocompletion for {{site.data.keyword.cloud_notm}} command-line (Linux/MacOS only)](/docs/cli?topic=cli-shell-autocomplete#shell-autocomplete-linux).
    {: tip}

2. Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.
    ```sh
    ibmcloud login
    ```
    {: pre}

    If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your user name and use the provided URL in your command-line output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
    {: tip}

## Installing the {{site.data.keyword.bplong_notm}} command-line plug-in
{: #install-schematics-plugin}

Install the {{site.data.keyword.bplong_notm}} plug-in to automate cloud operations, configuration management, and infrastructure deployments in {{site.data.keyword.cloud_notm}}. 
{: shortdesc}

1. Install the {{site.data.keyword.cloud_notm}} command-line plug-in for {{site.data.keyword.bpshort}}.
    ```sh
    ibmcloud plugin install schematics
    ```
    {: pre}

2. Verify that the {{site.data.keyword.bplong_notm}} command-line plug-in is installed successfully. The plug-in is listed as `schematics`.
    ```sh
    ibmcloud plugin list
    ```
    {: pre}

    Example output:

    ```text
    Listing installed plug-ins...

    Plugin Name         Version   Status        
    schematics          1.5.1     
    ```
    {: screen}

3. Verify that you can use the {{site.data.keyword.bpshort}} command-line plug-in by listing all supported commands. The command prefix to work with the {{site.data.keyword.bpshort}} command-line plug-in is `ibmcloud schematics`. 
    ```sh
    ibmcloud schematics help
    ```
    {: pre}

    Example output: 
    ```text
    NAME:
        ibmcloud schematics - IBM Cloud Schematics plug-in

    USAGE:
        ibmcloud schematics command [arguments...] [command options]

    COMMANDS:
        action          Create and manage Schematics actions. Action let you define the source control repository that contains your playbook yamls etc. and pass environment-specific variables.
        apply           Apply a plan to an workspace to deploy the latest version of your configuration.
        destroy         Destroy resources in an existing workspace. This action cannot be reversed.
        job             Create and manage Schematics jobs. Job let you manage all the jobs like creating/deleting/updating/retrieving.
        kms             listing and enabling ibmcloud schematics kms instances and root keys .
        logs            Show details about actions that ran against an workspace.
        output          Get all the output values from your workspace; (ex. result of terraform output command.
        plan            Create a plan for an workspace. Plans show how resources would change if you applied the latest version of your workspace configuration.
        refresh         Refresh the workspace with latest version of your workspace configuration.
        shareddataset   Create and manage shared datas. Shared datas let you define the source control pre defined variables values for Terraform configuration and pass workspace-specific variables.
        state           Advanced state management
        version         Report version information about the IBM Cloud Schematics CLI.
        workspace       Create and manage workspaces. workspaces let you define the source control repository that contains your Terraform configuration and pass workspace-specific variables.
        help, h         Show help

    Enter 'ibmcloud schematics help [command]' for more information about a command.

    ```
    {: screen}

## Updating the CLI
{: #schematics-cli-update}

Update the {{site.data.keyword.cloud_notm}} command-line and the {{site.data.keyword.bpshort}} command-line plug-in periodically to get access to new features. 
{: shortdesc}

1. [Update the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli#update-ibmcloud-cli). 

2. Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.

    ```sh
    ibmcloud login
    ```
    {: pre}

    If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your user name and use the provided URL in your command-line output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
    {: tip}

3. Check if an update is available for the {{site.data.keyword.bpshort}} command-line plug-in. If an update is available, you find an **Update available** notification in your command-line output. 
    ```sh
    ibmcloud plugin list | grep schematics
    ```
    {: pre}

    Example output: 

    ```text
    schematics                      1.5.9        Update Available           false
    ```
    {: screen}

4. Update the {{site.data.keyword.bpshort}} command-line plug-in. 
    ```sh
    ibmcloud plugin update schematics
    ```
    {: pre}


