---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-15"

keywords: schematics capabilities, schematics benefits, why use schematics, capabilities

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} capabilities 
{: #learn-about-schematics} 

Building on open-source Ansible, Terraform and related technologies, {{site.data.keyword.bplong}} provides a powerful set of **Infrastructure as Code** (IaC) tools as a service to program your cloud infrastructure. {{site.data.keyword.bpshort}} can run your end-to-end automation to build one or more stacks of cloud resources, manage their lifecycle, manage changes in their configurations, deploy your app workloads, and perform day-2 operations.
{: shortdesc}

## {{site.data.keyword.bpshort}} benefits
{: #learn-benefits}

Review the [IaC capabilities](/docs/schematics?topic=schematics-infrastructure-as-code) that {{site.data.keyword.bpshort}} provides for you as you explore {{site.data.keyword.cloud_notm}} automation.
{: shortdesc}

| Capability | Description |
|------|-------|
|Model your {{site.data.keyword.cloud_notm}} stacks| Use high-level scripting languages to declare all the cloud resources in your stack, and define their configurations. Instead of learning the cloud API or command-line to work with a specific resource, you can use Ansible playbooks and Terraform configuration files to specify the required state and configuration of a cloud resource. Then, you use {{site.data.keyword.bpshort}} to rapidly build, configure, and replicate the resources in your cloud environments.|
|Use open technologies | {{site.data.keyword.bpshort}} integrates open source projects, such as Terraform and Ansible, for you to natively automate the provisioning, configuration, and management of your cloud stacks. You do not need to install the open source projects on your machine or learn their API and command-line. You need to simply point {{site.data.keyword.bpshort}} to your IaC code repository and let {{site.data.keyword.bpshort}} run the specified tasks. |
|Automate cloud infrastructure deployments| Use Terraform and blueprint templates to codify and configure your cloud resources, and use {{site.data.keyword.bpshort}} Workspaces to reliably, predictably and consistently provision your resources across cloud environments. Terraform help you standardize your Cloud stacks, automate the lifecycle of the individual resource, and apply access and version control to the template, so that you can achieve compliance and rapidly troubleshoot the issues. |
|Automate day-2 operations of the cloud infrastructure| Use Ansible playbooks to perform complex day-2 operations on your cloud resources, cloud environment, and app workloads. Whether you want to deploy multitiered apps, start or stop virtual servers or clusters, rotate keys, backup and restore app data, perform security scans, manage database schemas, or manage users, simply specify the tasks that you want to run in your playbook, and let {{site.data.keyword.bpshort}} securely connect and complete the tasks.|
|{{site.data.keyword.IBM_notm}} software catalog| A catalog of {{site.data.keyword.IBM_notm}} provided and third party automation templates to deploy software in your {{site.data.keyword.containerlong_notm}} cluster, your {{site.data.keyword.redhat_openshift_notm}} on {{site.data.keyword.cloud_notm}} cluster, or a classic or {{site.data.keyword.vsi_is_short}}. Software packages are installed by using the built-in Terraform, Helm, {{site.data.keyword.containershort}} operator, and {{site.data.keyword.cpd_short}} capabilities. {{site.data.keyword.bpshort}} is used as the fulfillment engine for provisioning these softwares from the {{site.data.keyword.cloud_notm}} Catalog.|
| Secure and update IaC runtime engines | The open source projects used by {{site.data.keyword.bpshort}}, namely  - Terraform, Ansible, Helm provisioning engine, and execution platform are tested, maintained, and monitored by {{site.data.keyword.IBM_notm}}. {{site.data.keyword.IBM_notm}} automatically applies the latest security standards and patches to {{site.data.keyword.bpshort}} to ensure reliability and availability of the service. You do not need to manually apply updates to the {{site.data.keyword.bpshort}} platform. Instead you can leverage the built-in features of {{site.data.keyword.bpshort}} to securely run operations in the cloud.|
| Streamline version configuration | When you create blueprints, workspaces and actions, you can decide on the Terraform and Ansible version that you want to use to run your Terraform templates and Ansible playbooks. {{site.data.keyword.bpshort}} concurrently supports multiple versions of Terraform and Ansible that you can choose from. All versions are tested by {{site.data.keyword.IBM_notm}}. As new versions of Terraform and Ansible become available, {{site.data.keyword.IBM_notm}} begins with hardening and testing these versions, so that they can be supported in the {{site.data.keyword.bpshort}} platform. For more information, see [when are new Terraform and Ansible versions added to {{site.data.keyword.bpshort}}?](/docs/schematics?topic=schematics-actions-faq#new-versions)|
| Treat your stack configuration as code | By codifying your infrastructure, service and app stacks, you can treat your Terraform templates and Ansible playbooks the same way as you treat your app code. You can author your templates and playbooks in any code editor. Check them into a version control system such as GitHub, and let your team review and monitor updates before you apply these changes in your cloud environment. By applying these DevOps core practices, you can enable IaC for your cloud environments. |
| Control access to cloud environments and configurations | {{site.data.keyword.bpshort}} is fully integrated with IAM so that you can use service access roles to control who can access and collaborate on your workspaces and actions, or roll out changes. You can invite {{site.data.keyword.cloud_notm}} users to your account and leverage IAM access groups to streamline the access assignment process in your organization. As a multi-tenant solution, {{site.data.keyword.bpshort}} creates all resources in your personal account. Resources are not shared or reused by other {{site.data.keyword.cloud_notm}} tenants. Because {{site.data.keyword.bpshort}} is built on Kubernetes, IAM service access roles are mapped to role-based access controls (RBAC) in Kubernetes to enforce resource isolation within your account.|
| Full {{site.data.keyword.IBM_notm}} support for the open-source tools and plug-ins related to {{site.data.keyword.cloud_notm}} | {{site.data.keyword.bpshort}} is fully integrated into the {{site.data.keyword.cloud_notm}} support system. If you run into an issue by using the `IBM Cloud Provider plug-in for Terraform` or the Ansible modules for {{site.data.keyword.cloud_notm}}, you can [open an {{site.data.keyword.cloud_notm}} support case](/docs/get-support?topic=get-support-using-avatar#getting-support).|
{: caption="{{site.data.keyword.bpshort}} benefits" caption-side="bottom"}

## Next steps about {{site.data.keyword.bpshort}}
{: #nextstep-capabilities}

- Learn more about the {{site.data.keyword.bpshort}} [terminologies](/docs/schematics?topic=schematics-learn-schematics-term) and the [Open Source technologies](/docs/schematics?topic=schematics-schematics-open-projects) used in {{site.data.keyword.bpshort}}.
- Click [here](/docs/schematics?topic=schematics-about-schematics) to revisit the {{site.data.keyword.bpshort}} capabilities.
