---

copyright:
  years: 2017, 2022
lastupdated: "2022-05-17"

keywords: schematics capabilities, schematics benefits, why use schematics, capabilities

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} Capabilities 
{: #learn-about-schematics} 

{{site.data.keyword.bplong}} provides powerful tools to automate your cloud infrastructure provisioning and management process, the configuration and operation of your cloud resources, and the deployment of your app workloads.
{: shortdesc}

To do so, {{site.data.keyword.bpshort}} leverages open source projects, such as Terraform, Ansible, {{site.data.keyword.openshiftshort}}, Operators, Helm, and delivers these capabilities to you as a managed service. Rather than installing each open source project on your machine, and learning the API or command-line, you declare the tasks that you want to run in {{site.data.keyword.cloud_notm}} and watch {{site.data.keyword.bpshort}} run these tasks for you.

## {{site.data.keyword.bpshort}} benefits
{: #learn-benefits}

Review the capabilities that {{site.data.keyword.bpshort}} provides you to set up Infrastructure as Code (IaC).
{: shortdesc}

| Capability | Description |
|--------|-------------------------------|
|Model your {{site.data.keyword.cloud_notm}} stacks| Use high-level scripting languages to declare all the resources that you want to include in your {{site.data.keyword.cloud_notm}} infrastructure, service, and app stack. Instead of learning the API or command-line to work with a specific resource, you use Ansible playbooks and Terraform configuration files to specify the required state and configuration of an {{site.data.keyword.cloud_notm}} resource. Then, you use {{site.data.keyword.bpshort}} to rapidly build, configure, and replicate the resources in your cloud environments.|
|Leverage native capabilities of integrated open source projects | Because {{site.data.keyword.bpshort}} integrates with open source projects, such as Terraform and Ansible, you can use their native capabilities to automate the provisioning, configuration, and management of your {{site.data.keyword.cloud_notm}} stacks. You do not need to install the open source projects on your machine or learn their API and CLI. Simply point {{site.data.keyword.bpshort}} to your repository and let {{site.data.keyword.bpshort}} run the specified tasks. |
|Automate infrastructure deployments|Create Terraform templates to codify and configure the {{site.data.keyword.cloud_notm}} resources that you want, and use {{site.data.keyword.bpshort}} workspaces to enable predictable and consistent resource provisioning and management across cloud environments. Terraform templates help you standardize your {{site.data.keyword.cloud_notm}} stacks, automate the lifecycle of the individual resource, and apply access and version control so that you can achieve resource compliance and troubleshoot issues faster. |
|Automate config management of cloud resources and app deployments| With {{site.data.keyword.bpshort}} actions, you can use Ansible playbooks to create complex, reliable, and consistent configurations for your {{site.data.keyword.cloud_notm}} resources. Whether you want to deploy multitiered apps, set up firewalls rules, take down virtual server instances, or lock down users, simply specify the tasks that you want to run in your playbook, and let {{site.data.keyword.bpshort}} securely connect and complete the tasks on your {{site.data.keyword.cloud_notm}} resource. |
|Software catalog|Choose among IBM-provided software templates to easily install IBM and 3rd party software in your {{site.data.keyword.containerlong_notm}} cluster, your {{site.data.keyword.openshiftlong_notm}} cluster, or a classic or {{site.data.keyword.vsi_is_short}}. Software packages are installed by using the built-in Terraform, Ansible, Helm, OpenShift Operator, and Cloud Pak capabilities. |
|Automatic security and version updates |The versions of built-in open source projects, the {{site.data.keyword.bpshort}} provisioning engine, and execution platform are tested, maintained, and monitored by IBM. IBM automatically applies the latest security standards and patches to {{site.data.keyword.bpshort}} to ensure reliability and availability of the service. Other than in a traditional Ansible Tower deployment, you do not need to manually apply updates to the {{site.data.keyword.bpshort}} platform. Instead, you can leverage the built-in features of {{site.data.keyword.bpshort}} to securely run operations in the cloud. |
|Streamline version configuration|When you create {{site.data.keyword.bpshort}} workspaces and actions, you can decide on the Terraform and Ansible version that you want to use to run your Terraform templates and Ansible playbooks. {{site.data.keyword.bpshort}} concurrently supports multiple Terraform versions that you can choose from and the latest version of Ansible, version 2.9. All versions are tested by IBM. As new versions of Terraform and Ansible become available, IBM begins of hardening and testing these versions so that they can be supported in the {{site.data.keyword.bpshort}} platform. For more information, see [When are new Terraform and Ansible versions added to {{site.data.keyword.bpshort}}?](/docs/schematics?topic=schematics-faqs#new-versions). |
|Treat your stack configuration as code| By codifying your infrastructure, service and app stacks, you can treat your Terraform templates and Ansible playbooks the same way as you treat your app code. You can author your templates and playbooks in any code editor, check them into a version control system such as GitHub, and let your team review and monitor updates before you apply these changes in your cloud environment. By applying these DevOps core practices, you can enable Infrastructure as Code (IaC) for your cloud environments.|
|Control access to cloud environments and configurations|{{site.data.keyword.bpshort}} is fully integrated with IAM so that you can use service access roles to control who can access and collaborate on your workspaces and actions, or roll out changes. You can invite {{site.data.keyword.cloud_notm}} users to your account and leverage IAM access groups to streamline the access assignment process in your organization. As a multi-tenant solution, {{site.data.keyword.bpshort}} creates all resources in your personal account. Resources are not shared or reused by other {{site.data.keyword.cloud_notm}} tenants. Because {{site.data.keyword.bpshort}} is built on Kubernetes, IAM service access roles are mapped to role-based access controls (RBAC) in Kubernetes to enforce resource isolation within your account.|  
|Full support for integrated open source projects|{{site.data.keyword.bpshort}} is fully integrated into the {{site.data.keyword.cloud_notm}} support system. If you run into an issue by using the {{site.data.keyword.terraform-provider_full_notm}} or the Ansible support in {{site.data.keyword.bpshort}} actions, you can [open an {{site.data.keyword.cloud_notm}} support case](/docs/get-support?topic=get-support-using-avatar#getting-support).|
{: caption="{{site.data.keyword.bpshort}} benefits" caption-side="bottom"}
