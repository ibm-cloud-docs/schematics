---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-22"

keywords: schematics CLI, schematics command-line, schematics commands, terraform commands, terraform CLI, setting up schematics CLI, cli

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Setting up the CLI 
{: #setup-cli}

Use the {{site.data.keyword.bplong_notm}} command-line plug-in to automate the infrastructure provisioning process, the configuration of your {{site.data.keyword.cloud_notm}} resources, and the deployment of app workloads in {{site.data.keyword.cloud_notm}}. The {{site.data.keyword.bpfull}} command-line extends the support of following multiple platform architectures inline with the {{site.data.keyword.cloud}} command line interface (CLI).

- Mac OS X 64-bit
- Mac OS arm64
- Windows&trade; 64-bit
- Windows&trade; 32-bit
- Linux&trade; 64-bit
- Linux&trade; arm64
- Linux&trade; 32-bit
- PowerLinux&trade; 64-bit
- System/390 Linux&trade; 64-bit


## Installing the {{site.data.keyword.cloud_notm}} command-line 
{: #install-schematics-cli}

Install the required command-line to start using {{site.data.keyword.bplong_notm}}. 
{: shortdesc}

1. Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started). 

    Plan to use the command-line often? Try [Enabling shell auto completion for {{site.data.keyword.cloud_notm}} command-line (Linux/MacOS only)](/docs/cli?topic=cli-shell-autocomplete#shell-autocomplete-linux).
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

    Plugin Name                             Version   Status             Private endpoints supported   
    schematics                              1.12.5                        true  
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
    ibmcloud schematics - 

    USAGE:
    ibmcloud schematics command [arguments...] [command options]

    COMMANDS:
    action, ac           Create and manage Schematics actions. Action let you define the source control repository that contains your playbook yamls etc. and pass environment-specific variables.
    agents, ag           [Beta] Agents extends Schematics ability to reach your private, or on-premises, infrastructure.
    apply                Apply a plan to an workspace to deploy the latest version of your configuration.
    blueprint, bp        Deploy and manage large scale environments using reusable blueprint templates.
    destroy              Destroy resources in an existing workspace. This action cannot be reversed.
    inventory, iv        Create and manage Schematics Inventories. Inventory let you define host group that can contain INI or Resource Query id's
    job, j               Create and manage Schematics jobs. Job let you manage all the jobs like creating/deleting/updating/retrieving.
    kms                  listing and enabling IBM Cloud Schematics kms instances and root keys .
    logs                 Show details about actions that ran against an workspace.
    output               Get all the output values from your workspace; (ex. result of terraform output command
    plan                 Create a plan for an workspace. Plans show how resources would change if you applied the latest version of your workspace configuration.
    refresh              Refresh the workspace with latest version of your workspace configuration.
    resource-query, rq   Create and manage Schematics Resource Query. Resource query let you define conditions to fetch host group that can be used to perform actions
    state                Advanced state management
    version              Report version information about the IBM Cloud Schematics CLI.
    workspace, ws        Create and manage workspaces. Workspaces let you define the source control repository that contains your Terraform configuration and pass workspace-specific variables.
    help, h              Show help

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
    schematics                      1.12.5        Update Available           false
    ```
    {: screen}

4. Update the {{site.data.keyword.bpshort}} command-line plug-in.

    For {{site.data.keyword.bpshort}} Blueprints, the {{site.data.keyword.bpshort}} plug-in version must be greater than the `1.12.5` version.
    {: note}

    For {{site.data.keyword.bpshort}} Agents, the {{site.data.keyword.bpshort}} plug-in version must be greater than the `1.12.0` version.
    {: note}

    ```sh
    ibmcloud plugin update schematics
    ```
    {: pre}

    ```sh
    ibmcloud plugin list | grep schematics
    ```
    {: pre}

   Example output for blueprints support: 

    ```text
    schematics                      1.12.5        true
    ```
    {: screen}

5. Show the {{site.data.keyword.bpshort}} plug-in.
    ```sh
    ibmcloud plugin show schematics
    ```
    {: pre}

    Example output:

    ```text
    Plugin Name                              schematics   
    Plugin Version                           1.12.5   
    Plugin SDK Version                       0.8.0   
    Minimal IBM Cloud CLI version required   0.15.1   
    Private endpoints supported              true   

    Commands:
    schematics,sch                     Automate the deployment and management of IBM Cloud resources using Infrastructure as Code   
    schematics,sch workspace,ws        Create and manage workspaces. Workspaces let you define the source control repository that contains your Terraform configuration and pass workspace-specific variables.  
    schematics,sch action,ac           Create and manage Schematics actions. Action let you define the source control repository that contains your playbook yamls etc. and pass environment-specific variables.  
    schematics,sch job,j               Create and manage Schematics jobs. Job let you manage all the jobs like creating/deleting/updating/retrieving.  
    schematics,sch plan                Create a plan for an workspace. Plans show how resources would change if you applied the latest version of your workspace configuration.  
    schematics,sch apply               Apply a plan to an workspace to deploy the latest version of your configuration.  
    schematics,sch refresh             Refresh the workspace with latest version of your workspace configuration.  
    schematics,sch destroy             Destroy resources in an existing workspace. This action cannot be reversed.  
    schematics,sch output              Get all the output values from your workspace; (ex. result of terraform output command   
    schematics,sch version             Report version information about the IBM Cloud Schematics CLI.  
    schematics,sch logs                Show details about actions that ran against an workspace.  
    schematics,sch state               Advanced state management   
    schematics,sch kms                 listing and enabling IBM Cloud Schematics kms instances and root keys .  
    schematics,sch inventory,iv        Create and manage Schematics Inventories. Inventory let you define host group that can contain INI or Resource Query id's   
    schematics,sch resource-query,rq   Create and manage Schematics Resource Query. Resource query let you define conditions to fetch host group that can be used to perform actions   
    schematics,sch agents,ag           [Beta] Agents extends Schematics ability to reach your private, or on-premises, infrastructure.  
    schematics,sch blueprint,bp        Deploy and manage large scale environments using reusable blueprint templates. 
    ```
    {: screen}

## Uninstalling the {{site.data.keyword.bplong_notm}} command-line plug-in
{: #uninstall-schematics-plugin}

Uninstall the {{site.data.keyword.bplong_notm}} plug-in to remove the {{site.data.keyword.bpshort}} plugin from your local system.
{: shortdesc}

1. Uninstall the {{site.data.keyword.cloud_notm}} command-line plug-in for {{site.data.keyword.bpshort}}.
    ```sh
    ibmcloud plugin uninstall schematics
    ```
    {: pre}

    Example output:

    ```text
    Uninstalling plug-in 'schematics'...
    OK
    Plug-in 'schematics' was successfully uninstalled.
    ```
    {: screen}

2. Verify that the {{site.data.keyword.bplong_notm}} command-line plug-in is uninstalled successfully.
    ```sh
    ibmcloud plugin list | grep schematics
    ```
    {: pre}
