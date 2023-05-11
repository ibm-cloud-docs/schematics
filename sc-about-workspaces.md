---

copyright:
  years: 2017, 2023
lastupdated: "2023-05-09"

keywords: schematics workspaces, workspaces, schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} Workspaces
{: #sc-workspaces}

{{site.data.keyword.bpshort}} Workspaces delivers Terraform as a service capabilities to automate the [provisioning and configuration management](/docs/schematics?topic=schematics-schematics-open-projects) of your {{site.data.keyword.cloud_notm}} resources, and rapidly build, duplicate, and scale complex, multitiered cloud environments.

[Terraform](https://www.terraform.io){: external} is an open source project that allows you to specify your cloud infrastructure resources and services as code. It applies the concept of [Infrastructure as Code (IaC)](/docs/schematics?topic=schematics-infrastructure-as-code), using code to manage and provision infrastructure (networks, virtual machines, load-balancers, clusters, services and connection topology) in a descriptive model instead of manual processes.
{: shortdesc} 

With Terraform, configuration files define your infrastructure, which also makes it easier to edit, share, and reuse configurations. By codifying your infrastructure, you provision the same environment every time avoiding undocumented, ad hoc configuration changes.

The blog [Infrastructure as Code: Chef, Ansible, Puppet, or Terraform?](https://www.ibm.com/cloud/blog/chef-ansible-puppet-terraform){: external} provides an overview of several of the most popular open-source IaC tools and summarizes their capabilities and relative strengths.

## Architecture
{: #sch-wks-overview}

![Deploying infrastructure and services with Workspaces](/images/new/sc-workspaces.svg){: caption="Deploying infrastructure and services with Workspaces" caption-side="bottom"}

Using your supplied Terraform template (config), {{site.data.keyword.bpshort}} runs the Terraform engine to execute your config. 

## Features
{: #sc-wks-features}

{{site.data.keyword.cloud}} provides built in [remote-state](/docs/schematics?topic=schematics-remote-state) management for Terraform. Terraform state files are automatically preserved between runs and are accessible by {{site.data.keyword.cloud}} commands and operations. {{site.data.keyword.cloud}} remote-state management enables team work and workspace shared operations, with built in state locking preventing concurrent operations against the same state file. 

Workspaces is designed for teams. Terraform templates can be stored in a GitHub, GitLab, or `Bitbucket` repository to ensure source control and enable collaboration, review, and auditing in your organization.

Workspaces supports [drift detection](/docs/schematics?topic=schematics-drift-note), detecting when the configuration of your deployed infrastructure differs from the wanted state that is defined in your template configuration. Drift can occur for many reasons. The most frequent cause changes that are applied manually outside of Terraform automation.

## Next steps
{: #sc-wks-nextsteps}

So far you have learned about {{site.data.keyword.bpshort}} Workspaces. The following are some next steps to explore.
{: shortdesc}

- [Getting started usecase](/docs/schematics?topic=schematics-get-started-terraform) to understand how to use workspace to deploy and manage your Infrastructure as Code (IaC) in the cloud environments.
- [Sample Terraform templates](/docs/schematics?topic=schematics-create-tf-config) to provide you how to create well-structured, and reusable Terraform templates.
- See [Creating Workspaces](/docs/schematics?topic=schematics-sch-create-wks) to dig into how to create workspace using the Terraform templates. 
