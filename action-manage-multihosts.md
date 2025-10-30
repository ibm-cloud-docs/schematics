---

copyright:
  years: 2017, 2025
lastupdated: "2025-10-30"

keywords: schematics multihost, ansible multihost credentials, multihost variables, multihost credentials, ansible multihost inventories, multihost

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}
ai-gen-assist: wca

# Secure and streamline Ansible automation with {{site.data.keyword.bpshort}} multihost credentials and variables
{: #sch-multihost-setup}

Ansible playbooks in {{site.data.keyword.bplong}} enable automation of configuration and application deployment on various hosts. For secure management of access to these hosts, {{site.data.keyword.bpshort}} offers multihost credentials and variables support. The multihost approach centralizes and securely handles authentication details, including SSH keys, usernames, and passwords for numerous target systems in one place. This streamlines the process and enhances security by avoiding the need to hardcode credentials directly into playbooks.
{: shortdesc}

## Managing Credentials
{: #sch-multihost-credentials}

Multihost credentials in {{site.data.keyword.bpshort}} streamlines Ansible deployment management by centralizing credentials, reducing duplication, enhancing security through encryption and role-based access, and can ensure consistency across environments. This approach simplifies large-scale automation, maintains compliance, and strengthens security.

## Types of credentials
{: #sch-multihost-credential-types}

{{site.data.keyword.bpshort}} supports different credential types to manage host and group access. These credential types offer flexibility and security in managing host access for automation workflows.

1. Group and Host Credentials: Shared access details for multiple hosts, with host credentials specific to individual hosts.
2. Common Credential: A global access credential shared by all hosts, simplifying management by reducing the need for unique credentials per host.
3. Bastion Credentials: Used to connect through a Bastion host, acting as a secure gateway to protected hosts or groups.

## Managing variables
{: #sch-multihost-variable}

Managing variables simplifies updates, reduces inconsistencies, minimizes sensitive data exposure, promotes consistency, and can easily adapt in environments, making automation tasks more robust and maintainable. The following are the primary types of variables.

1. **Host Variables**: Settings are applied to individual hosts, enabling customization of specific configurations like IP addresses or credentials for a particular server.

2. **Group Variables**: Applies the same settings to all hosts within a group, simplifying the management of shared configurations, such as common users or environment settings.

Both managing credentials and variables are applicable to static inventory in {{site.data.keyword.bpshort}}. You can define and securely store access details (credentials) and configuration settings (variables) for your hosts within a static inventory file, ensuring consistent and secure automation task execution across your infrastructure. For more information about static inventory, see [static inventory](/docs/schematics?topic=schematics-inventories-setup#static-inv).
{: note}

## Managing credentials and variables by using UI
{: #sch-multihost-mange}
{: ui}

To manage credentials and variables using the {{site.data.keyword.bpshort}} UI, follow these steps:

1. **Log in** to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![Hamburger icon](images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > **Ansible**.
3. Select your playbook and click **Inventory** from the left pane.
4. Click the three dots next to the inventory name and select **Manage Credentials**.
5. To add new credentials, click **Add credentials**. And enter the required details. For example, the username and password for [Common Credentials](/docs/schematics?topic=schematics-sch-multihost-setup#sch-multihost-credential-types) and click **Add**. You can edit or delete existing credentials as needed.
6. For managing variables, click **Add variables**. Choose the **Host or Hostgroup**, specify the **Variable** name, **value**, and mark the variable as sensitive if necessary. You can also edit or delete existing variables.
7. After configuring credentials and variables, Navigate to the **Overview** panel, review your actions, and make any necessary edits.
8. Click **Run your action** and monitor the logs for successful execution.

## Managing credentials and variables by using CLI
{: #sch-multihost-manage-cli}
{: cli}

To manage credentials and variables by using the {{site.data.keyword.bpshort}} CLI, follow these steps:
