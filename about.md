---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-02"

keywords: about schematics, schematics overview, infrastructure as code, iac, differences schematics and terraform, schematics vs terraform, how does schematics work, schematics benefits, why use schematics, terraform template, schematics workspace

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
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# About {{site.data.keyword.bplong_notm}}
{: #about-schematics} 

{{site.data.keyword.bplong_notm}} delivers Terraform-as-a-Service so that you can use a high-level scripting language to model the resources that you want in your {{site.data.keyword.cloud_notm}} environment, and enable Infrastructure as Code (IaC). [Terraform](https://www.terraform.io/){: external} is an Open Source software that is developed by HashiCorp that enables predictable and consistent resource provisioning to rapidly build complex, multi-tier cloud environments.
{: shortdesc}

**What is Infrastructure as Code?** </br>
Infrastructure as Code (IaC) helps you codify your cloud environment so that you can automate the provisioning and management of your resources in the cloud. Rather than manually provisioning and configuring infrastructure resources or using scripts to adjust your cloud environment, you use a high-level scripting language to specify your resource and its configuration. Then, you use tools like Terraform to provision the resource in the cloud by leveraging its API. Your infrastructure code is treated the same way as your app code so that you can apply DevOps core practices such as version control, testing, and continuous monitoring. 

**How is {{site.data.keyword.bplong_notm}} different from Terraform?** </br>
With {{site.data.keyword.bplong_notm}}, you can organize your {{site.data.keyword.cloud_notm}} resources across environments by using workspaces. Every workspace points to a set of Terraform configuration files, which build a Terraform template. You can choose to create your own Terraform template, or use one of the pre-defined templates that are provided by IBM. Workspaces allow for the separation of concerns for cloud resources and can be individually managed with {{site.data.keyword.cloud_notm}} Identity and Access Management. To use {{site.data.keyword.bplong_notm}}, you don't need to install the Terraform CLI or the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. 

**I am not familiar with Terraform. Can I still use {{site.data.keyword.bplong_notm}}?** </br>
Yes. {{site.data.keyword.bplong_notm}} provides a set of pre-defined Terraform templates that you can choose from to get started with {{site.data.keyword.bpshort}}. Simply select the template that you want and create a workspace in {{site.data.keyword.bplong_notm}} from this template. Then, create a Terraform execution plan, apply this plan, and watch {{site.data.keyword.bplong_notm}} provision the resources for you. 

## How it works
{: #schematics-architecture}
{: help}
{: support}

Review how {{site.data.keyword.bplong_notm}} provisions and manages your {{site.data.keyword.cloud_notm}} resources. 
{: shortdesc}

<img src="images/schematics_flow.png" alt="Provisioning {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bplong_notm}}" width="800" style="width: 800px; border-style: none"/>

1. **Codify your {{site.data.keyword.cloud_notm}} resources**. Use Terraform HashiCorp Configuration Language (HCL) or JSON format to specify the {{site.data.keyword.cloud_notm}} resources that you want to provision in your {{site.data.keyword.cloud_notm}} environment. If you are not familiar with Terraform, you can select one of the default Terraform templates that {{site.data.keyword.bplong_notm}} provides to provision the {{site.data.keyword.cloud_notm}} resources that you want. Terraform templates can be stored in a GitHub or GitLab repository to ensure source control and enable collaboration, review, and auditing in your organization. You can save usage information in `readme` files to make the template shareable and usable across multiple teams. You can also upload tape archive files (`.tar`) from your local machine to provide {{site.data.keyword.bpshort}} with your template.
2. **Create your workspace**. You can point your {{site.data.keyword.bplong_notm}} workspace to a GitHub or GitLab repository where you store your Terraform template, or provide your template by uploading a `.tar` file. Workspaces help to organize resources that belong to one {{site.data.keyword.cloud_notm}} environment. For example, use workspaces to separate your test, staging, and production environment. With {{site.data.keyword.cloud_notm}} Identity and Access Management, you can control who has access to your resources and can provision or manage these resources in your {{site.data.keyword.cloud_notm}} account. 
3. **Create an execution plan**. {{site.data.keyword.bplong_notm}} uses the `terraform plan` command to parse the configuration files of the provided Terraform template, and to create a summary of actions that need to be performed to achieve the state that is described in your configuration files. To determine the actions, {{site.data.keyword.bplong_notm}} takes into account the resources that are already provisioned in your {{site.data.keyword.cloud_notm}} account to give you a preview whether resources must be added, modified, or removed. You can review the plan and any validation errors by reviewing the logs.  
4. **Provision your resources**. To create, modify, or remove resources from your {{site.data.keyword.cloud_notm}} account, {{site.data.keyword.bplong_notm}} uses the `terraform apply` command. This command calls the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform, which is aware of the API for each resource to provision, configure, or remove the resource. 

## Benefits
{: #schematics-benefits}

Review the capabilities that {{site.data.keyword.bplong_notm}} provides to templatize and organize your  {{site.data.keyword.cloud_notm}} resources. 
{: shortdesc}

| Benefit    | Description   |
| :------------- | :------------- |
| Enable Infrastructure as Code (IaC) | Use Terraform templates to model, codify, and configure the {{site.data.keyword.cloud_notm}} resources that you want, and build your own resource library that you can replicate or re-create across environments. If you want to change your environment, you state the outcome that you want and let {{site.data.keyword.bplong_notm}} determine the actions that must be performed to get to the described state. |
| Use native Terraform capabilities | Build your Terraform configuration files in HashiCorp Configuration Language (HCL) or JSON format and provision your specified resources with {{site.data.keyword.bplong_notm}}. {{site.data.keyword.bplong_notm}} supports all {{site.data.keyword.cloud_notm}} resources that are provided by the [{{site.data.keyword.cloud_notm}} Provider plug-in for Terraform](/docs/terraform?topic=terraform-tf-provider) with the advantage that you don't have to install the Terraform CLI and the {{site.data.keyword.cloud_notm}} Provider plug-in. Simply use the built-in Terraform capabilities in the {{site.data.keyword.cloud_notm}} console or CLI to connect {{site.data.keyword.bplong_notm}} to the GitHub repository that hosts your template or provide your template by uploading a `.tar` file. Then, create an execution plan, and watch {{site.data.keyword.bplong_notm}} spin up your resources.  |
| One language to describe resources | Every {{site.data.keyword.cloud_notm}} resource comes with a CLI or API that you can use to provision and work with the resource. By using {{site.data.keyword.bplong_notm}}, you don't need to learn each CLI or API to automate the provisioning of your resources. Instead, you use the Terraform language to model all your resources. |
| Organize {{site.data.keyword.cloud_notm}} resources in workspaces | With {{site.data.keyword.bplong_notm}}, you can organize your {{site.data.keyword.cloud_notm}} resources across environments by using workspaces. Every workspace is connected to a GitHub repository that stores a Terraform template. You can also provide the template by uploading a `.tar` file from your local machine. Use workspaces to distinguish between your test, staging, and prod environment, and to change resource configurations without affecting resources in other environments.  |
| Control access to your {{site.data.keyword.cloud_notm}} resources | Assign platform and services access permissions to your users in {{site.data.keyword.cloud_notm}} Identity and Access Management to control who can provision and manage resources in your {{site.data.keyword.cloud_notm}} account. |
| Leverage GitHub for version control | If you connect your workspace to a repository in GitHub, you can keep your Terraform template in source control and enable collaboration, review, and auditing of changes. You can also roll back to a previous version of your template and let {{site.data.keyword.bplong_notm}} deploy the change to your {{site.data.keyword.cloud_notm}} environment. |  
| Get {{site.data.keyword.cloud_notm}} help and support | {{site.data.keyword.bplong_notm}} is fully integrated into the {{site.data.keyword.cloud_notm}} support system. If you run into an issue with using {{site.data.keyword.bplong_notm}}, [open an {{site.data.keyword.cloud_notm}} support case](/docs/get-support?topic=get-support-using-avatar). |

## Key terms
{: #schematics-terms}

{{site.data.keyword.bplong_notm}} uses Terraform as the underlying tool to enable Infrastructure as Code (IaC) so that you can define and deploy your resources with simple text files. 
{:shortdesc}

Learn the basics about Terraform and {{site.data.keyword.bplong_notm}} by reviewing the following terms.

<dl>
  <dt>Resources</dt>
  <dd>Resources are {{site.data.keyword.cloud_notm}} Platform-as-a-Service, Infrastructure-as-a-Service, and Functions-as-a-Service components that you can provision and manage in {{site.data.keyword.cloud_notm}} with {{site.data.keyword.bplong_notm}}. Resources are specified and configured by using Terraform configuration files. The resources that are supported in {{site.data.keyword.bplong_notm}} are determined by the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. The plug-in provides the syntax to describe a supported resource, and understands the APIs of each supported {{site.data.keyword.cloud_notm}} resource and available configuration parameters. <br>To find a list of supported {{site.data.keyword.cloud_notm}} resources, see the [{{site.data.keyword.cloud_notm}} Provider plug-in reference](/docs/terraform?topic=terraform-index-of-terraform-resources-and-data-sources).</p></dd>
</dl>

<dl>
  <dt>Terraform configuration file</dt>
  <dd>A Terraform configuration file defines the {{site.data.keyword.cloud_notm}} resources that you want to create. You can configure one resource per file, or combine multiple resources in one file. Terraform configuration files can be written in HashiCorp Configuration Language (HCL) or JSON syntax, and are stored in a GitHub or GitLab repository or uploaded by using a `.tar` file.<br>For more information about how to write configuration files, see [creating Terraform configurations](/docs/schematics?topic=schematics-create-tf-config). </p></dd>
</dl>

<dl>
  <dt>Template</dt>
  <dd>A Terraform template includes one or a set of Terraform configuration files that combined can be used to build a specific {{site.data.keyword.cloud_notm}} solution. For example, you might have a template that creates a multizone cluster in {{site.data.keyword.containerlong_notm}}. This cluster consists of multiple {{site.data.keyword.cloud_notm}} resources in different zones, such as classic infrastructure virtual servers and VLANs. You can build your own template and import it into {{site.data.keyword.bplong_notm}} by storing all Terraform configuration files that build your configuration in one GitHub or GitLab repository. You can also provide your template by uploading a tape archive file (`.tar`) from your local machine. Templates are designed and constructed for reuse by using variables so that you can share these templates with other teams in your organization.  </dd>
</dl>

<dl>
  <dt>Workspace</dt>
  <dd>A workspace is used to organize your {{site.data.keyword.cloud_notm}} resources across environments. For example, use workspaces to separate your test, staging, and production environment. You can provide your Terraform template by connecting your workspace to a GitHub or GitLab repository or by uploading a tape archive file (`.tar`) from your local machine. To customize the {{site.data.keyword.cloud_notm}} resources to your needs, you specify user-defined variables in your workspace. With {{site.data.keyword.cloud_notm}} Identity and Access Management, you can control who has access to your resources and can provision or manage these resources in your {{site.data.keyword.cloud_notm}} account. 
  </dd>
</dl>

<dl>
  <dt>Execution plan</dt>
  <dd>An execution plan is a summary of actions that {{site.data.keyword.bplong_notm}} must perform to provision, modify, or remove the {{site.data.keyword.cloud_notm}} resources of your template. The plan is created by running the <code>terraform plan</code> command.
  </dd>
</dl>

<dl>
  <dt>{{site.data.keyword.cloud_notm}} Provider plug-in</dt>
  <dd>To support a multi-cloud approach, Terraform works with different cloud providers. A cloud provider is responsible for understanding the resources that you can provision, their API, and the methods to expose these resources in the cloud. To make this knowledge available to users, each cloud provider must provide a CLI plug-in for Terraform. The {{site.data.keyword.cloud_notm}} Provider plug-in is IBM's CLI plug-in for Terraform. {{site.data.keyword.bplong_notm}} uses the plug-in to provision your {{site.data.keyword.cloud_notm}} resources. To find a list of supported {{site.data.keyword.cloud_notm}} resources and how to describe them, see the [{{site.data.keyword.cloud_notm}} Provider plug-in reference](/docs/terraform?topic=terraform-index-of-terraform-resources-and-data-sources).</p>
  </dd>
</dl>











