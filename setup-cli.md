---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-19"

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

## Installing the IBM Cloud CLI and IBM Cloud Schematics plug-in
{: #install-schematics-cli}

Install the required CLIs to automate the provisioning of {{site.data.keyword.cloud_notm}} resources across environments. 
{:shortdesc}

1.  Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cloud-cli-getting-started#idt-prereq). This installation includes:
    -   {{site.data.keyword.cloud_notm}} CLI (`ibmcloud`)
    -   Several CLI plug-ins, such as for {{site.data.keyword.containerlong_notm}} (`ibmcloud ks`), {{site.data.keyword.registryshort_notm}} plug-in (`ibmcloud cr`), and {{site.data.keyword.cloud_notm}} Functions (`ibmcloud fn`)
    -   Other CLIs, such as Helm (`helm`), Docker (`docker`), Kubernetes (`kubectl`) Git (`git`), and Homebrew (`brew`)

    Plan to use the CLI often? Try [Enabling shell autocompletion for {{site.data.keyword.cloud_notm}} CLI (Linux/MacOS only)](/docs/cli/reference/ibmcloud?topic=cloud-cli-shell-autocomplete#shell-autocomplete-linux).
    {: tip}

2.  Log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your {{site.data.keyword.cloud_notm}} credentials when prompted.
    ```
    ibmcloud login
    ```
    {: pre}

    If you have a federated ID, use `ibmcloud login --sso` to log in to the {{site.data.keyword.cloud_notm}} CLI. Enter your user name and use the provided URL in your CLI output to retrieve your one-time passcode. You know that you have a federated ID when the login fails without the `--sso` and succeeds with the `--sso` option.
    {: tip}
    
3.  Install the {{site.data.keyword.bplong_notm}} CLI plug-in. 
    ```
    ibmcloud plugin install terraform -r 'IBM Cloud'
    ```
    {: pre}
    
4.  Verify that the {{site.data.keyword.bplong_notm}} CLI plug-in is installed correctly.
    ```
    ibmcloud plugin list
    ```
    {: pre}

    Example output:
    ```
    Listing installed plug-ins...

    Plugin Name                            Version   Status        
    terraform                              0.1.373     
    ```
    {: screen}
