---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-05"

keywords: automate continuous deployment using Schematics, automate continuous deployment of resource using Schematics and DevOps toolchain, continuous deployment of resources

subcollection: schematics

content-type: tutorial
services: schematics, continuous-deployment
account-plan: lite
completion-time: 60m

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
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
{:java: #java .ph data-hd-programlang='java'}
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
{:swift: #swift .ph data-hd-programlang='swift'}
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
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}


# Automate continuous deployment by using {{site.data.keyword.bpfull_notm}} and DevOps toolchain
{: #schematics-continuous-deployment}
{: toc-content-type="tutorial"}
{: toc-services="schematics, continuous-deployment"}
{: toc-completion-time="60m"}

## Description
{: #schematics-desc}

In this tutorial, you can learn to use your credentials and an API key to use a Terraform template of {{site.data.keyword.cos_full_notm}} in the Schematics workspace. Then, you also learn to automate the continuous deployment by using DevOps delivery pipeline. As part of the tutorial, you will use `ibm_cos_bucket` Terraform template example.
The ibm_cos_bucket example creates an instance of {{site.data.keyword.cos_full_notm}}, {{site.data.keyword.cloud_notm}} Activity Tracker and {{site.data.keyword.cloud_notm}} monitoring with Sysdig. 
{: shortdesc}

As per your resource usage, the cost is incurred. For more information, about the pricing, refer to [Pricing](/docs/billing-usage?topic=billing-usage-charges). About the support and help, refer to [Schematics help](/docs/schematics?topic=schematics-schematics-help).
{: important}
   

## Objectives
{: #schematics-obj}

In this tutorial, you can:
- Explore an IBM provided Terraform template to create an {{site.data.keyword.cloud_notm}} Object Storage instance that binds with the IBM resource instance, and IBM resource group.
- Learn how to create an {{site.data.keyword.bplong_notm}} workspace.
- Learn to automate continuous  deployment of a resource by using {{site.data.keyword.bplong_notm}} and DevOps toolchain.
- Review the {{site.data.keyword.cloud_notm}} resources that you create.

## Time required
{: #schematics-timereq}

1 hour

## Audience
{: #schematicsd-tut-audience}

This tutorial is intended for developer and system administrators who want to learn how to use Terraform templates to create and automate the continuous deployment of resource by using {{site.data.keyword.bplong_notm}} and DevOps toolchain.

## Prerequisites
{: #schematics-prereq}

**About {{site.data.keyword.bplong_notm}}**

[{{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-getting-started) is an {{site.data.keyword.cloud_notm}} automation tool. It provides simplified provisioning, orchestrating Infrastructure as  Code (IaC), templates, and managing {{site.data.keyword.cloud_notm}} resources in your {{site.data.keyword.cloud_notm}} environment by using various resources tools such as Terraform, Helm, etc.
IaC helps you codify your cloud environment to automate the provisioning, speeds deployment and managing your resources. The infrastructure is treated the same way as your app code, so that you can automate the DevOps core practices such as version control, testing, continuous integration and deployment.
{: shortdesc}

**About DevOps toolchain**

A DevOps toolchain is a set of tools that automates the tasks of developing and deploying your app. A toolchain is a set of tool integrations that support development, deployment, and operations tasks. The collective power of a toolchain is greater than the sum of its individual tool integrations.

For the information, about the importance of using an {{site.data.keyword.cloud_notm}} toolchain and continuous delivery of your app, refer to [DevOps toolchain](/docs/apps?topic=apps-devops-toolchains).
Schematics provides option to enable the continuous delivery of your infrastructure configurations as well with {{site.data.keyword.cloud_notm}} toolchain.

Complete the following prerequisites for the tutorial:

- If you do not have {{site.data.keyword.cloud_notm}} account, create an {{site.data.keyword.cloud_notm}} account and pay as you use. For more information, about managing {{site.data.keyword.cloud_notm}} account, refer to [Managing IBM Cloud account](https://cloud.ibm.com/registration).
- Install the IBM Cloud CLI and the Schematics CLI plug-in. For more information, about CLI setup, see [Schematics CLI setup](/docs/schematics?topic=schematics-setup-cli).
- Ensure you are assigned the required permissions in {{site.data.keyword.iamlong}} to create and work with {{site.data.keyword.bplong_notm}} workspace. Refer to [Schematic access](/docs/schematics?topic=schematics-access#access-roles) and to create an {{site.data.keyword.cos_full_notm}} service instance. 
- Follow the instructions to ensure you are assigned the required permissions in Identity and Access Management to create resources. For more information, about create Cloud Object Storage, refer to [Cloud Object Storage](https://cloud.ibm.com/docs/cloud-object-storage?topic=cloud-object-storage-provision).

## Accessing the {{site.data.keyword.cloud_notm}} and GitHub
{: #access-automate-template}
{: step}

Complete the following steps to access the {{site.data.keyword.cloud_notm}} and the Terraform templates from the GitHub:
{: shortdesc}

1. If you do not have one, create an [IBM Cloud Pay-As-You-Go or Subscription {{site.data.keyword.cloud_notm}} account](https://cloud.ibm.com/registration){: external}. 
2. Log in to your [GitHub](https://github.com/) account. 
3. Open the Terraform template to create an {{site.data.keyword.cos_full_notm}}. (https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cos-bucket) 
4. From the right corner of the GitHub page, click `Fork` icon to create your own fork of the shared repository.
   You need to copy the URL of the Terraform template of the GitHub or the GitLab Repository URL to create your Schematics workspace.
   {: note}

## Creating your {{site.data.keyword.bplong_notm}} workspace
{: #create-workspace}
{: step}

Complete the following steps to create the {{site.data.keyword.bplong_notm}} and the Terraform template URL.
{: shortdesc}

1. Click `Navigation Menu > Schematics > Create a Schematics workspace` from the {{site.data.keyword.cloud_notm}} page.
2. Provide a unique name for the `Workspace name` parameter and add values for the other parameters as required. The default values are maintained in this tutorial.
3. Click `Create` to view your workspace page.
4. Scroll to view import your Terraform template to fill the GitHub repository URL, Personal access token and Terraform version details.
  From the import your Terraform template, click sample templates to view the sample templates. In the tutorial, [ibm-cos-bucket](https://github.com/nibhart1/toolchain_template/tree/master/ibm-cos-bucket) example repository is used.
5. Copy and paste the URL in the GitHub or GitLab repository URL parameter. Optionally, You can add your Personal access token of the template from your private repository.
6. Select Terraform version as `terraform_v0.12` from the drop-down list.
7. Click `Save template information` to view the Variables page.

## Configuring the variables
{: #configure-variables}
{: step}

Configure the variables as described in the table to authenticate the api keys and user credentials and save the changes.
{: shortdesc}

|Name|Value|
|-----|-----|
|`iaas_classic_username`|Enter the username to access IBM Cloud classic infrastructure. |
|`iaas_classic_api_key`|Enter the API key to access IBM Cloud classic infrastructure. For more information, to create an API key, refer to [Classic infrastructure API keys](/docs/account?topic=account-classic_keys#create-classic-infrastructure-key).|
|`ibmcloud_api_key`|Enter your IBM Cloud API Key, for more information on API key, refer to [IBM Cloud API key](https://cloud.ibm.com/iam#/apikeys(https://cloud.ibm.com/iam#/apikeys).|
|`resource_group_name`| Keep as default.|

1. On the variable page, click `Enable continuous delivery` hyperlink option to view Schematics Infrastructure as Code (IaC) Toolchain page.
2. Click `Delivery Pipeline Required` tab.

  The GitHub Server type parameter expects the authorization, you can provide GitHub credentials and confirm the authorization.
  {: note}

## Automating and analyzing the continuous deployment process
{: #schematics-continuous-deployment}
{: step}

The `Enable continuous delivery` option has the capability of automating the different Terraform actions to the Schematics workspace. Complete the following steps to observe the automation of the end to end {{site.data.keyword.bplong_notm}} workspace deployment.

1. Provide the IBM Cloud API Key in the `Tool Integrations panel`. Click `Create` to view the Toolchains page.
2. Click `Deliver Pipeline` pane to view Schematics Pipeline | Delivery Pipeline page. 
  Observe the UPDATE is in STAGE RUNNING state, without using the button click.
  {: note}
3. During the update stage process, from the example of ibm-cos-bucket repository observe the `main.tf` file configuration with the cos_instance name and bucket_name. These details are updated in the Schematics workspace after the APPLY stage is passed.
4. Once the `UPDATE` stage is completed, PLAN stage is in running state.
5. Click the View logs and history to view the status of the job from the `PLAN` pane.
6. Observe that the PLAN stage is passed, and APPLY stage is in running state.
7. From the Schematics workspace check the resource name and bucket name are created successfully.
8. Observe that the `APPLY` stage is passed and TEST stage is in Running state.
9. Observe that the `TEST` stage is passed successfully.
10. Now, you can edit your template in the configured repository and observe that there is an automatic pull of your workspace by the continuous delivery toolchain.

**Analyzing the deployment**

Alternatively, through the {{site.data.keyword.cloud_notm}} dashboard, you can view the status of the workspace.
{: shortdesc}

1. From the {{site.data.keyword.cloud_notm}}, select `Navigation Menu > Schematics > Workspaces > Resources` to observe the apply state of the resources in your workspace.
2. You can view the output from your working directory, or from the {{site.data.keyword.cloud_notm}} dashboard plan logs to view the workspace status.

## What's next?
{: #automate-what's next}

Congratulations! You successfully created the {{site.data.keyword.bplong_notm}} workspace and automated the end to end deployment by using the DevOps toolchain. You can now learn how to set up a continuous delivery pipeline for an IBM cluster. For more information, refer to [Setting up a continuous delivery pipeline for an IBM cluster](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cluster).
