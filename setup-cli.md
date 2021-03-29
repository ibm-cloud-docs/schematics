---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-29"

keywords: schematics CLI, schematics command line, schematics commands, terraform commands, terraform CLI, setting up schematics CLI, cli

subcollection: schematics

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Setting up the CLI 
{: #setup-cli}

Use the {{site.data.keyword.bplong_notm}} command line plug-in to automate the infrastructure provisioning process, the configuration of your {{site.data.keyword.cloud}} resources, and the deployment of app workloads in {{site.data.keyword.cloud}}. 
{: shortdesc}


## Installing the IBM Cloud command line 
{: #install-schematics-cli}

Install the required command line to start using {{site.data.keyword.bplong_notm}}.  
{:shortdesc}

1. Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started). 

   Plan to use the command line often? Try [Enabling shell autocompletion for {{site.data.keyword.cloud_notm}} command line (Linux/MacOS only)](/docs/cli?topic=cli-shell-autocomplete#shell-autocomplete-linux).
   {: tip}

2. Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.
   ```
   ibmcloud login
   ```
   {: pre}

   If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your user name and use the provided URL in your command line output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
   {: tip}

## Installing the {{site.data.keyword.bplong_notm}} command line plug-in
{: #install-schematics-plugin}

Install the {{site.data.keyword.bplong_notm}} plug-in to automate cloud operations, configuration management, and infrastructure deployments in {{site.data.keyword.cloud_notm}}. 
{:shortdesc}
    
1. Install the {{site.data.keyword.cloud_notm}} command line plug-in for {{site.data.keyword.bpshort}}.

   ```
   ibmcloud plugin install schematics
   ```
   {: pre}
    
2. Verify that the {{site.data.keyword.bplong_notm}} command line plug-in is installed successfully. The plug-in is listed as `schematics`.

   ```
   ibmcloud plugin list
   ```
   {: pre}

   Example output:

   ```
   Listing installed plug-ins...

   Plugin Name         Version   Status        
   schematics          1.5.1     
   ```
   {: screen}
    
3. Verify that you can use the {{site.data.keyword.bpshort}} command line plug-in by listing all supported commands. The command prefix to work with the {{site.data.keyword.bpshort}} command line plug-in is `ibmcloud schematics`. 
   ```
   ibmcloud schematics help
   ```
   {: pre}
    
   Example output: 
   ```
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
{: schematics-cli-update}

Update the {{site.data.keyword.cloud_notm}} command line and the {{site.data.keyword.bpshort}} command line plug-in periodically to get access to new features. 
{: shortdesc}

1.  [Update the {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli#update-ibmcloud-cli). 

2. Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.

    ```
    ibmcloud login
    ```
    {: pre}

     If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your user name and use the provided URL in your command line output to retrieve your one-time passcode. You know you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
     {: tip}

3. Check if an update is available for the {{site.data.keyword.bpshort}} command line plug-in. If an update is available, you find an **Update available** notification in your command line output. 
   ```
   ibmcloud plugin list | grep schematics
   ```
   {: pre}
   
   Example output: 

   ```
   schematics                      1.5.0        Update Available
   ```
   {: screen}
   
4. Update the {{site.data.keyword.bpshort}} command line plug-in. 

   ```
   ibmcloud plugin update schematics
   ```
   {: pre}
