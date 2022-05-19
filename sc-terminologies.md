---

copyright:
  years: 2017, 2022
lastupdated: "2022-05-17"

keywords: schematics terminology, infrastructure as code, iac, terminology, 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} terminology
{: #learn-schematics-term} 

{{site.data.keyword.bplong}} provides powerful tools to automate your cloud infrastructure provisioning and management process, the configuration and operation of your cloud resources, and the deployment of your app workloads.
{: shortdesc}

Learn the basic concepts of the technology behind {{site.data.keyword.bplong_notm}}.

| Term | Description |
| --- | --- |
| `Actions` | Specifies how you want to run your [Ansible playbook](/docs/schematics?topic=schematics-getting-started-ansible) in {{site.data.keyword.bpshort}}. When you [create an action](/docs/schematics?topic=schematics-action-setup#create-action), you point your action to the Ansible playbook in your GitHub or GitLab repository. Then, you define the {{site.data.keyword.cloud_notm}} resource inventory where you want to run your playbook. You can also add environment variables and schedule to your action to customize when your playbook runs. |
| `Blueprints` | Assembles multiple components such as Workspace, and Actions runnable templates. {{site.data.keyword.bpshort}} blueprints support deploying and managing large-scale solution infrastructures on {{site.data.keyword.cloud_notm}}. |
| `Cart` | Fulfills the orders from other {{site.data.keyword.cloud_notm}} service. Cart is also known as shopping cart.|
| `Inventory` | Defines a single {{site.data.keyword.cloud_notm}} resource or a group of resources where you want to run Ansible playbooks, modules, or roles by using {{site.data.keyword.bpshort}} actions. You can specify your resource inventory by using a [static inventory file](/docs/schematics?topic=schematics-inventories-setup#static-inv), or [dynamically retrieve](/docs/schematics?topic=schematics-inventories-setup#dynamic-inv) to your target {{site.data.keyword.cloud_notm}} resources from {{site.data.keyword.bpshort}} workspaces that you created.|
| `Workspaces` | Organizes your {{site.data.keyword.cloud_notm}} resources across environments. For example, you can create workspaces to separate your development, test, staging, and production environment. You can provide your Terraform template by [connecting your workspace to a source repository](/docs/schematics?topic=schematics-workspace-setup&interface=ui) or by uploading a tape archive file (`.tar`) from your local machine. To customize the {{site.data.keyword.cloud_notm}} resources to your needs, you specify user-defined variables in your workspace. With {{site.data.keyword.iamlong}}, you can control who can manage and configure your resources in your {{site.data.keyword.cloud_notm}} account. |
{: caption="{{site.data.keyword.bplong_notm}} terminologies" caption-side="bottom"}
