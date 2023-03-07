---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-28"

keywords: about schematics open source projects, open source projects, why use schematics, terraform template, schematics workspace

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Choosing your IaC tool  
{: #schematics-open-projects}

The choice of IaC tool matters. Some tools are a better fit for the task in hand. Often the use of different tools for configuration management and provisioning is the most effective choice. The section below, [Understanding {{site.data.keyword.bpshort}} features and IaC tools](/docs/schematics?topic=schematics-schematics-open-projects#sc-iac-mapping) identifies the mapping of Ansible and Terraform, along with operators and Helm to related {{site.data.keyword.bpshort}} features. 

But what is provisioning and configuration management? 

## What is Provisioning?
{: #sc-iac-provisioning}

Provisioning is the process of setting up IT infrastructure. It can also refer to the steps required to manage access to data and resources, and make them available to users and systems. Once something has been provisioned, the next step is configuration. [Red Hat](https://www.redhat.com/en/topics/automation/what-is-provisioning){: external} 

Provisioning tools (including Terraform and Ansible) provision infrastructure such as servers (VMs), load balancers, databases, networking configuration, and so on. They leave configuration to configuration tools.

“Provisioning” often implies it is an initial task. 

## What is Configuration Management?
{: #sc-iac-cm}

Configuration management is a systems engineering process for establishing and maintaining computer systems, servers, and software in a desired, consistent performance — state. Managing IT system configurations involves defining a system’s desired state — like server configuration — then building and maintaining those systems. [Wikipedia](https://en.wikipedia.org/wiki/Configuration_management){: external}

Config management tools install packages or software, manage software and configurations on existing provisioned servers, clusters and infrastructure. Terraform and Ansible can be used for configuration management, along with Helm and Operators. 

Config management usually happens repeatedly.


## How to choose your IaC tool
{: #sc-iac-choosing}

Some tools are a better fit for the task in hand for provisioning or configuration management. The blog [Infrastructure as Code: Chef, Ansible, Puppet, or Terraform?](https://www.ibm.com/cloud/blog/chef-ansible-puppet-terraform) provides an overview of several popular open-source IaC tools and summarizes their capabilities and relative strengths. 

{{site.data.keyword.cloud_notm}} leverages Terraform and Ansible, and other open-source tools including {{site.data.keyword.openshiftshort}}, Operators and Helm to deliver IaC as a managed service. Rather than limiting you to a single tool, {{site.data.keyword.bpshort}} allows you to use the tool and approach best suited to the task. You declare the tasks that you want to run and {{site.data.keyword.bpshort}} will run the tasks for you.

### Understanding Schematics features and IaC tools
{: #sc-iac-mapping}

 Review the tool descriptions below to identify the {{site.data.keyword.bpshort}} feature that maps to the IaC capability you would like to use. 
{: shortdesc}

|Logo|Open source project &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  | Actions|  Agents| Workspaces| Blueprints | IBM&nbsp;Catalog|
|---|---|:--:|:--:|:--:|:--:|:--:|
|<img src="images/ansible.png" alt="Ansible" width="10" style="width: 50px; border-style: none"/>|[Ansible](https://www.ansible.com/){: external} is a configuration management and provisioning tool, similar to Chef and Puppet. It is designed to automate the configuration and management of environments, and deploy app workloads in the cloud. |<img src="images/checkmark.svg" alt="Check mark" width="30" style="width: 30px; border-style: none"/>| | | |<img src="images/checkmark.svg" alt="Check mark" width="30" style="width: 30px; border-style: none"/>|
|<img src="images/helm.svg" alt="Helm" width="10" style="width: 50px; border-style: none"/>|[Helm](https://helm.sh/){: external} is a Kubernetes package manager that uses Helm charts to define, install, and upgrade complex Kubernetes apps in an {{site.data.keyword.containerlong_notm}} cluster.|| | ||<img src="images/checkmark.svg" alt="Check mark" width="30" style="width: 30px; border-style: none"/>|
|<img src="images/operator.png" alt="Operators" width="10" style="width: 50px; border-style: none"/>|[OpenShift Operators](https://www.redhat.com/en/technologies/cloud-computing/openshift/what-are-openshift-operators){: external} are a convenient way to add and run community, Third party, and other services in a {{site.data.keyword.openshiftlong_notm}} cluster. ||| ||<img src="images/checkmark.svg" alt="Check mark" width="30" style="width: 30px; border-style: none"/>|
|<img src="images/terraform.png" alt="Terraform" width="10" style="width: 50px; border-style: none"/>|[Terraform](https://www.terraform.io/){: external} is an Open Source project that specifies your cloud infrastructure resources and services using a high-level scripting language.||<img src="images/checkmark.svg" alt="Check mark" width="30" style="width: 30px; border-style: none"/>|<img src="images/checkmark.svg" alt="Check mark" width="30" style="width: 30px; border-style: none"/>|<img src="images/checkmark.svg" alt="Check mark" width="30" style="width: 30px; border-style: none"/>|<img src="images/checkmark.svg" alt="Check mark" width="30" style="width: 30px; border-style: none"/>|
{: caption="Open Source Projects" caption-side="bottom"}

## Next steps
{: #nextsteps-technologies}

- Learn more about the [terminology](/docs/schematics?topic=schematics-glossary) used in {{site.data.keyword.bpshort}}.
- Do you want to know how these open-source tools are used in {{site.data.keyword.bpshort}}? Explore these [use cases](/docs/schematics?topic=schematics-how-it-works).
- Visit [What is Infrastructure as Code?](/docs/schematics?topic=schematics-infrastructure-as-code) to understand more about Infrastructure as code and its best practices. 
