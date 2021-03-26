---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-26"

keywords: about schematics, schematics overview, infrastructure as code, iac, differences schematics and terraform, schematics vs terraform, how does schematics work, schematics benefits, why use schematics, terraform template, schematics workspace

subcollection: schematics

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:beta: .beta}
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

Learn about what {{site.data.keyword.bpshort}} is, how the service works, and the key features and benefits that you can leverage to achieve Infrastructure as Code (IaC) 
{: shortdesc}

## What is {{site.data.keyword.bpshort}}?
{: #what-is-schematics}
{: help}
{: support}

{{site.data.keyword.bplong_notm}} is a managed {{site.data.keyword.cloud_notm}} offering that provides powerful tools to automate your cloud infrastructure provisioning process, the configuration and operation management for your cloud resources, and app deployments. 
{: shortdesc}

To do so, {{site.data.keyword.bpshort}} leverages open source projects, such as Terraform, Ansible, Operators, and Helm, and delivers these capabilities to you as a managed service. Rather than installing each open source project on your machine, and learning the API or CLI, you declare the tasks that you want to run in {{site.data.keyword.cloud_notm}} and watch {{site.data.keyword.bpshort}} run these tasks for you. Your infrastructure code is treated the same way as your app code so that you can apply DevOps core practices, such as version control, testing, and continuous monitoring.

## How does {{site.data.keyword.bpshort}} work?
{: #how-it-works}
{: help}
{: support}

Choose among the following use cases to learn how {{site.data.keyword.bpshort}} automates your infrastructure, service, and app stack in {{site.data.keyword.cloud_notm}}.
{: shortdesc}

- [Infrastructure deployments with {{site.data.keyword.bpshort}} workspaces](#how-to-workspaces)
- [Configuration management with {{site.data.keyword.bpshort}} actions](#how-to-actions)
- [Software deployments with IBM-provided templates](#how-to-software)

### Infrastructure deployment with {{site.data.keyword.bpshort}} workspaces
{: #how-to-workspaces}

{{site.data.keyword.bpshort}} workspaces deliver Terraform-as-a-Service capabilities to you so that you can automate the provisioning and management of your {{site.data.keyword.cloud_notm}} resources, and rapidly build complex, multi-tier cloud environments. 
{: shortdesc}

To get started with infrastructure deployment in {{site.data.keyword.bpshort}}, see the [Getting started tutorial](/docs/schematics?topic=schematics-get-started-terraform). 
{: tip}

[Terraform](https://www.terraform.io/){: external} is an open source project that lets you specify your desired cloud infrastructure resources and services by using a high-level scripting language. Your specification is stored in a Terraform configuration file. In order to abstract the APIs and complexity of the cloud resource provisioning and management process to the user, cloud providers create a plug-in for Terraform that contains the information for how to connect to the cloud provider and what APIs to call to work with a certain cloud resource. IBM's plug-in is called the [{{site.data.keyword.cloud_notm}} Provider plug-in for Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-index-of-terraform-resources-and-data-sources).  

To use the capabilities of the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform, you create a {{site.data.keyword.bpshort}} workspace that points to the Terraform configuration files that you want to run. Review the following image to find detailed information about how to run Terraform configuration files with {{site.data.keyword.bpshort}} workspaces. 

<img src="images/schematics_flow.png" alt="Provisioning {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bplong_notm}} and Terraform" width="800" style="width: 800px; border-style: none"/>

1. **Codify your {{site.data.keyword.cloud_notm}} resources**. Use Terraform HashiCorp Configuration Language (HCL) or JSON format to specify the {{site.data.keyword.cloud_notm}} resources that you want to provision in your {{site.data.keyword.cloud_notm}} environment. If you are not familiar with Terraform, you can select one of the default Terraform templates that {{site.data.keyword.bpshort}} provides to provision the {{site.data.keyword.cloud_notm}} resources that you want. Terraform templates can be stored in a GitHub, GitLab, or Bitbucket repository to ensure source control and enable collaboration, review, and auditing in your organization. You can save usage information in `readme` files to make the template shareable and usable across multiple teams. You can also upload tape archive files (`.tar`) from your local machine to provide the template to {{site.data.keyword.bpshort}}.
2. **Create your workspace**. You can point your {{site.data.keyword.bpshort}} workspace to a GitHub, GitLab, or Bitbucket repository where you store your Terraform template, or provide your template by uploading a `.tar` file. Workspaces help to organize resources that belong to one {{site.data.keyword.cloud_notm}} environment. For example, use workspaces to separate your test, staging, and production environment. With {{site.data.keyword.cloud_notm}} Identity and Access Management, you can control who has access to your workspaces and who can run actions on your {{site.data.keyword.cloud_notm}} resources. 
3. **Create an execution plan**. Based on your configuration, Terraform creates an execution plan and describes the actions that need to be run to get to the state that is described in your Terraform configuration files. To determine the actions, {{site.data.keyword.bpshort}} analyzes the resources that are already provisioned in your {{site.data.keyword.cloud_notm}} account to give you a preview of whether resources must be added, modified, or removed. You can review the execution plan, change it, or simply execute the plan. 
4. **Provision your resources**. When you are ready to make changes to your cloud environment, you can apply your Terraform configuration files. To run the actions that are specified in your configuration files, {{site.data.keyword.bpshort}} uses the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. 

</br>

### Configuration management with {{site.data.keyword.bpshort}} actions
{: #how-to-actions}

{{site.data.keyword.bpshort}} actions deliver Ansible-as-a-Service capabilities to you so that you can automate the configuration and management of your {{site.data.keyword.cloud_notm}} environment, and deploy complex multi-tier apps to your  cloud infrastructure. 
{: shortdesc}

To get started with configuration management in {{site.data.keyword.bpshort}}, see the [Getting started tutorial](/docs/schematics?topic=schematics-get-started-ansible). 
{: tip}

[Ansible](https://www.ansible.com/){: external} is a configuration management and provisioning tool, similar to Chef and Puppet, and is designed to automate the configuration and management of cloud environements, and to deploy multi-tier app workloads in the cloud. Ansible uses YAML syntax to describe the tasks that must be run against a single host or a group of hosts, and stores these tasks in an Ansible playbook. 

Ansible does not use agents or a custom security infrastructure that must be present on a target machine to work properly. Instead, Ansible securely connects to compute hosts over the public network by using SSH keys or an optional bastion host. To bring a resource to the required state, Ansible pushes modules to the managed host that run the tasks in your Ansible playbook. After the tasks are executed, the result is returned to the Ansible server and the module is removed from the managed host. Ansible modules are idempotent such that executing the same playbook or operation multiple times returns the same result as resources are changed only if required. For more information about Ansible, check out this [video](https://www.youtube.com/watch?v=fHO1X93e4WA){: external}. 

To use Ansible capabilities in {{site.data.keyword.bpshort}}, you create a {{site.data.keyword.bpshort}} action that points to the Ansible playbook that you want to run. Review the following image to find detailed information about how to run Ansible playbooks with {{site.data.keyword.bpshort}} actions. 

<img src="images/ansible_flow.png" alt="Configuring {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bplong_notm}} and Ansible" width="700" style="width: 700px; border-style: none"/>

1. **Add tasks to your playbook**: Use Ansible YAML syntax to describe the configuration tasks that you want to run on your cloud infrastructure, such as installing software or starting, stopping, and rebooting a virtual server. You add these tasks to an Ansible playbook and store the playbook in a GitHub, GitLab, or Bitbucket repository to ensure source control and enable collaboration, review, and auditing in your organization. If you are not familiar with Ansible, you can use one of the [IBM-provided playbooks](https://github.com/Cloud-Schematics){: external}, or browse the [Ansible Galaxy library](https://galaxy.ansible.com/){: external}.
2. **Create a {{site.data.keyword.bpshort}} action**: When you create a {{site.data.keyword.bpshort}} action, you point your action to the repository that stores your Ansible playbook. Then, you select the cloud resources where you want to run the tasks that are defined in you Ansible playbook. To protect your cloud resources, you can further set up a bastion host in front of your target hosts that proxies all Ansible SSH connections to the target hosts. 
3. **Run your action**: When you are ready to configure your cloud resources, you can run your action. {{site.data.keyword.bpshort}} uses the built-in Ansible capabilities to connect to your target hosts via SSH, and execute the tasks that are defined in your Ansible playbook. You can monitor the progress by reviewing the logs.  

</br>

### Software deployments with IBM-provided templates
{: #how-to-software}

Browse the [IBM software solutions catalog](https://cloud.ibm.com/catalog#software){: external} and choose among a wide range of software and infrastructure templates that you can use to set up cloud resources, and to install IBM and 3rd party software in your {{site.data.keyword.containerlong_notm}} cluster, your {{site.data.keyword.openshiftlong_notm}} cluster, or a classic or VPC virtual server instance. 
{: shortdesc}

Software templates are installed by using the built-in Terraform, Ansible, Helm, OpenShift Operator, and CloudPak capabilities in {{site.data.keyword.bpshort}}. When you choose to install one of the provided templates, you create a {{site.data.keyword.bpshort}} workspace and choose the target service or host where you want run the installation. You can review which of the integrated technologies in {{site.data.keyword.bpshort}} is used to install your template. 





## What open source projects does {{site.data.keyword.bpshort}} integrate with?
{: #open-source-ov}

{{site.data.keyword.bplong_notm}} integrates with the following open source projects to provide powerful automation and configuration management capabilities. 
{: shortdesc}

|Logo|Open source project description|{{site.data.keyword.bpshort}} actions|{{site.data.keyword.bpshort}} workspaces|IBM software solutions catalog|
|---|---|--|--|--|
|<img src="images/ansible.png" alt="Ansible" width="50" style="width: 50px; border-style: none"/>|[Ansible](https://www.ansible.com/){: external} is a configuration management and provisioning tool, similar to Chef and Puppet, and is designed to automate the configuration and management of cloud environements, and to deploy multi-tier app workloads in the cloud. |X||X|
| <img src="images/cloudpak.png" alt="CloudPaks" width="50" style="width: 50px; border-style: none"/>||IBM software solutions catalog|
|<img src="images/helm.svg" alt="Helm" width="50" style="width: 50px; border-style: none"/>|[Helm](https://helm.sh/){: external} is a Kubernetes package manager that uses Helm charts to define, install, and upgrade complex Kubernetes apps in an {{site.data.keyword.containerlong_notm}} cluster.|IBM software solutions catalog|
|<img src="images/operator.png" alt="Operators" width="50" style="width: 50px; border-style: none"/>|[OpenShift Operators](https://www.openshift.com/learn/topics/operators){: external} are a convenient way to add and run community, 3rd party, and other services in a {{site.data.keyword.openshiftlong_notm}} cluster. |[{{site.data.keyword.bpshort}} software deployments](#how-to-software)|
|<img src="images/terraform.png" alt="Terraform" width="50" style="width: 50px; border-style: none"/>|[Terraform](https://www.terraform.io/){: external} is an open source project that lets you specify your desired cloud infrastructure resources and services by using a high-level scripting language.|[{{site.data.keyword.bpshort}} workspaces](#how-to-workspaces)|


## What are the benefits of using {{site.data.keyword.bpshort}}?
{: #schematics-benefits}

Review the benefits of using {{site.data.keyword.bpshort}}. 
{: shortdesc}

|Feature|Description|
|--------|-------------------------------|
|Model your {{site.data.keyword.cloud_notm}} stacks| Use high-level scripting languages to declare all the resources that you want to include in your {{site.data.keyword.cloud_notm}} infrastructure, service, and app stack. Instead of learning the API or command line to work with a specific resource, you use Ansible playbooks and Terraform configuration files to specify the required state and configuration of an {{site.data.keyword.cloud_notm}} resource. Then, you use {{site.data.keyword.bpshort}} to rapidly build, configure, and replicate the resources in your cloud environments.|
|Leverage native capabilities of integrated open source projects | Because {{site.data.keyword.bpshort}} integrates with open source projects, such as Terraform and Ansible, you can use their native capabilities to automate the provisioning, configuration, and management of your {{site.data.keyword.cloud_notm}} stacks. You do not neet to install the open source projects on your machine or learn their API and CLI. Simply point {{site.data.keyword.bpshort}} to your repository and let {{site.data.keyword.bpshort}} run the specified tasks. |
|Automate infrastructure deployments|Create Terraform templates to codify and configure the {{site.data.keyword.cloud_notm}} resources that you want, and use {{site.data.keyword.bpshort}} workspaces to enable predictable and consistent resource provisioning and management across cloud environments. Terraform templates help you standardize your {{site.data.keyword.cloud_notm}} stacks, automate the lifecycle of the individual resource, and apply access and version control so that you can achieve resource compliance and troubleshoot issues faster. |
|Automate config management of cloud resources and app deployments| With {{site.data.keyword.bpshort}} actions, you can use Ansible playbooks to create complex, reliable, and consistent configurations for your {{site.data.keyword.cloud_notm}} resources. Whether you want to deploy multi-tier apps, set up firewalls rules, take down virtual server instances, or lock down users, simply specify the tasks that you want to run in your playbook, and let {{site.data.keyword.bpshort}} securely connect and complete the tasks on your {{site.data.keyword.cloud_notm}} resource. |
|Software catalog|Choose among IBM-provided software templates to easily install IBM and 3rd party software in your {{site.data.keyword.containerlong_notm}} cluster, your {{site.data.keyword.openshiftlong_notm}} cluster, or a classic or VPC virtual server instance. Software packages are installed by using the built-in Helm and OpenShift Operator capabilities. |
|Treat your stack configuration as code| By codifying your infrastructure, service and app stacks, you can treat your Terraform templates and Ansible playbooks the same way as you treat your app code. You can author your templates and playbooks in any code editor, check them into a version control system such as GitHub, and let your team review and monitor updates before you apply these changes in your cloud environment. By applying these DevOps core practices, you can enable Infrastructure as Code (IaC) for your cloud environments.|
|Control access to cloud environments and configurations|{{site.data.keyword.bpshort}} is fully integrated with {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) so that you can control who can access your cloud environment and app configurations, and roll out changes. |
|Full support for integrated open source projects|{{site.data.keyword.bpshort}} is fully integrated into the {{site.data.keyword.cloud_notm}} support system. If you run into an issue by using the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform or Ansible, you can [open an {{site.data.keyword.cloud_notm}} support case](/docs/get-support?topic=get-support-using-avatar#getting-support).|















