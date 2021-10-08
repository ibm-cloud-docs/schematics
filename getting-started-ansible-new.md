---

copyright:
  years: 2017, 2021
lastupdated: "2021-10-08"

keywords: get started with schematics, infrastructure management, infrastructure as code, iac, schematics cloud environment, schematics infrastructure, schematics terraform, terraform provider
subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Getting started with configuration management in {{site.data.keyword.bplong_notm}}
{: #getting-started-ansible}

Use one of the IBM-provided Ansible playbooks to start and stop {{site.data.keyword.vsi_is_short}}. 
{: shortdesc}

An Ansible playbook is a set of instructions that you can run on a single target host or a group of hosts. You create a {{site.data.keyword.bpshort}} action that points to your playbook and use the built-in Ansible capabilities in {{site.data.keyword.bpshort}} to run the instructions in your playbook. For more information about how {{site.data.keyword.bpshort}} runs your Ansible playbooks, see [Configuration management with {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-about-schematics#how-to-actions). 

## Before you begin
{: #ansible-prereq}

Before you can use this Ansible playbook, you must complete the following tasks:

- Make sure that you have the permissions to [create a {{site.data.keyword.bpshort}} action](/docs/schematics?topic=schematics-access#access-roles). 
- Create an {{site.data.keyword.vpc_full}} and a virtual server instance. For more information, see [Getting started with Virtual Private Cloud (VPC)](/docs/vpc?topic=vpc-getting-started). **Note** the **private** or **Floating IP** address of your virtual server instance. 

## Starting and stopping {{site.data.keyword.vsi_is_short}}
{: #ansible-vsi}

1. From the [{{site.data.keyword.bpshort}} actions](https://cloud.ibm.com/schematics/actions){: external} page, click **Create action**. 
2. Enter a name for your action, for example, `Stop_VSIaction`, resource group, and the region where you want to create the action. Then, click **Create** to view the **Details** section.
3. In the **Ansible playbook** section, click **Edit icon** enter `https://github.com/Cloud-Schematics/ansible-is-instance-actions` in the **GitHub or GitLab repository URL** field. 
4. Click **Retrieve playbooks**. 
5. Select the **`stop-vsi-playbook.yaml`** playbook. 
   The [floating IP address](/docs/vpc?topic=vpc-using-instance-vnics#editing-network-interfaces) of the VSI is available to set your input variable.
   {: note}
6. Expand the **Advanced options**. 
7. In the **Define your variables** section, enter `instance_ip` as the **key** and the floating IP address of your virtual server instance as the **value**. 
8. Click **Save**. 
9. Click **Check action** to verify your action details. The **Jobs** page opens automatically and you can view the results of this check by looking at the logs. 
10. Click **Run action** to stop the virtual server instance. You can monitor the progress of this action by reviewing the logs on the **Jobs** page. 
11. Verify that your virtual server instance stopped. 
    1. From the [Virtual server instances for VPC dashboard](https://cloud.ibm.com/vpc-ext/compute/vs){: external}, find your virtual server instance. 
    2. Verify that your instance shows a `Stopped` status. 
12. Optional: Repeat the steps in this getting started tutorial to create another {{site.data.keyword.bpshort}} action, and select the **`start-vsi-playbook.yaml`** Ansible playbook to start your virtual server instance again. 

Congratulations! You used the built-in Ansible capabilities of {{site.data.keyword.bpshort}} to start and stop a {{site.data.keyword.vsi_is_short}} instance. 

## What's next? 
{: ansible-whats-next}

Now that you ran your first operation on an {{site.data.keyword.cloud_notm}} resource, you can explore the following options:

- Learn how to [create your own Ansible playbooks](/docs/schematics?topic=schematics-create-playbooks).
- Explore other [IBM-provided playbooks](https://github.com/Cloud-Schematics){: external}.
- Set up the {{site.data.keyword.bpshort}} [CLI](/docs/schematics?topic=schematics-setup-cli) or [API](/docs/schematics?topic=schematics-setup-api) to start automating the configuration of {{site.data.keyword.cloud_notm}} resources.


