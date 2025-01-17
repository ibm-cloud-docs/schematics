---

copyright:
  years: 2017, 2025
lastupdated: "2025-01-17"

keywords: schematics inventory, ansible inventory, inventories, ibm cloud schematics inventories

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Creating resource inventories for {{site.data.keyword.bpshort}} actions
{: #inventories-setup}

A resource inventory defines a single {{site.data.keyword.cloud_notm}} resource or a group of resources where you want to run Ansible playbooks, modules, or roles by using {{site.data.keyword.bpshort}} actions.
{: shortdesc}

You can specify your resource inventory by using a [static inventory file](#static-inv), or [dynamically retrieve](#dynamic-inv) target {{site.data.keyword.cloud_notm}} resources from {{site.data.keyword.bpshort}} workspaces.

## Creating static inventory files
{: #static-inv}

{{site.data.keyword.bpshort}} supports the definition of `hosts.ini` files where you specify a single target host or a group of target hosts using their IP address. You can assign names to a group of target hosts, such as `[webserver]`, and use this name in your Ansible playbook to instruct {{site.data.keyword.bpshort}} where to run the playbook tasks.
{: shortdesc}

### Defining static hosts
{: #static-host-defs}

To connect to a target host through SSH, Ansible requires either a DNS resolvable hostname or an IP address.  

Typical usage of {{site.data.keyword.bpshort}} actions to configure VSI's on IBM Cloud is through a bastion host. See [actions documentation](/docs/schematics?topic=schematics-sc-actions). Access to the private network interface of a VSI is through a bastion host. This means using the hosts private IP address to connect to the target host, as private host names are not advertised by DNS to {{site.data.keyword.bpshort}}. For private hosts, inventory must be configured with an IP address and the IP address of a bastion host.

VSI's with public IP addresses and publicly registered DNS host names can be referenced by fully qualified DNS host names.

The use of (short form) hostnames is not supported.

### Creating the host file
{: #static-inv-create}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![hamburger icon](images/icon_hamburger.svg) > **Platform Automation**  >  **Schematics** > **Ansible**.
3. Click the playbook name.
4. Click **Inventory** > **Create Inventory**.
5. Enter a name for your inventory, verify your location, and select your `Resource group` where you want to create an inventory.
6. Select the **Define manually** tab.
7. In the **File** field, enter the target hosts where you want to run the Ansible playbook. Make sure to specify your hosts in an `INI` syntax. For a sample syntax, see [File format](#inv-file-format). Review the [limitations](#inv-file-limitation) to ensure that your inventory definition is supported in {{site.data.keyword.bpshort}}.
8. Click **Create inventory**.
9. Follow the [steps](/docs/schematics?topic=schematics-action-working#create-action) to create a {{site.data.keyword.bpshort}} actions and use the resource inventory that you created.

### File format
{: #inv-file-format}

Review the following sample `hosts.ini` file to see the structure of the static host inventory list. For more information about `hosts.ini` file structures, see [Ansible documentation](https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html#how-to-build-your-inventory){: external}.

This example references both publicly accessible hosts through host name and private hosts through IP address.  

    ```text
    mail.example.com

    [webservers]
    web1.mydomain.com
    web2.mydomain.com

    [dbservers]
    172.16.5.9
    172.16.5.10
    172.16.5.11

    [datacenters]
    172.15.4.0
    ```
    {: screen}

### Limitations
{: #inv-file-limitation}

Review the following limitations of static inventory files in {{site.data.keyword.bpshort}}: 

- {{site.data.keyword.bpshort}} supports only the `INI` file syntax.
- Variables are not supported in `hosts.ini` files.
- Specifying host groups by using key-value pairs in `hosts.ini` files is not supported.
- You must manually update the `hosts.ini` file if hostname or IP address of target hosts change.
- Where hosts are secured by a bastion host and only accessible through private IP address, the IP address of the host must be specified.
- Where hosts are accessible through a publicly accessible IP address and have a registered DNS name, the host can be referenced by its fully qualified host name.   
- All target hosts must be configured with the same public SSH key. When you use the static inventory file in your {{site.data.keyword.bpshort}} action, you can specify one SSH key to authenticate with all target hosts that are included in your resource inventory. The SSH key should contain `\n` at the end of the key details in case of command-line or API calls. For more information about SSH keys, see [Adding an SSH key](/docs/ssh-keys?topic=ssh-keys-adding-an-ssh-key).

## Dynamically building resource inventories from {{site.data.keyword.bpshort}} workspaces
{: #dynamic-inv}

You can dynamically build your resource inventory from the {{site.data.keyword.cloud_notm}} resources that you provisioned with {{site.data.keyword.bpshort}} workspaces.
{: shortdesc}

Dynamic resource inventories references {{site.data.keyword.cloud_notm}} resources that you provisioned with {{site.data.keyword.bpshort}} workspaces. To retrieve the {{site.data.keyword.cloud_notm}} resources from your workspaces, use predefined resource queries. You do not need to keep track of the IP addresses that were assigned to your target resources as {{site.data.keyword.bpshort}} automatically determines the target hosts when you use this inventory in the {{site.data.keyword.bpshort}} action.

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![hamburger icon](images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > [**Ansible**](https://cloud.ibm.com/automation/schematics/ansible){: external}.
3. Click the playbook name.
4. Click **Inventory** > **Create Inventory**.
5. Enter a name for your inventory, verify your location, and select your `Resource group` where you want to create an inventory.
6. Select the **Host groups** tab.
7. Click **Create host group**.
8. Enter a name for your host group and select the {{site.data.keyword.bpshort}} workspace that provisioned the target hosts that you want to add to your host group.
9. Optional: Click **Add query** to add another condition to your query and limit the number of target hosts that are added to your host group. You can choose to identify your hosts by using the resource name or a tag. For more information, see [supported resource queries](#supported-queries).
10. Repeat step 5-8 to add more host groups.
11. From the table list, select the host groups that you want to include in your resource inventory.
12. Click **Create inventory**.
13. Follow the [steps](/docs/schematics?topic=schematics-action-working#create-action) to create the {{site.data.keyword.bpshort}} actions and use the resource inventory that you created.

### Supported resource queries
{: #supported-queries}

|Supported query|Description|
|--|--|
|Workspace name|Select all the {{site.data.keyword.cloud_notm}} resources from a specific workspace.|
|Workspace name AND resource name|Select a specific resource from a specific workspace by using the resource name. To select multiple resources from the same workspace, you can add multiple queries of this type. |
|Workspace name AND tag|Select a specific resource from a specific workspace by using tags. Tags are added to the resource when you create the workspace and provision your resource. To select multiple resources with different tags from the same workspace, you can add multiple queries of this type. |
{: caption="Resource query" caption-side="top"}

### Limitations
{: #dynamic-inv-limitation}

Review the following limitations of dynamic inventories in {{site.data.keyword.bpshort}}: 

- You can choose among the [supported queries](#supported-queries) to select the target virtual server instances to include in your resource inventory.
- {{site.data.keyword.bpshort}} retrieves the IP address of a target {{site.data.keyword.vsi_is_short}}s and adds the IP address to the resource inventory. If the target {{site.data.keyword.vsi_is_short}} has a public IP address, the public IP address is used for the resource inventory. If there is no public IP address, the private IP address is used for the resource inventory.
- All target hosts must be configured with the same public SSH key. When you use the static inventory file in your {{site.data.keyword.bpshort}} action, you can specify one SSH key to authenticate with all target hosts that are included in your resource inventory. The SSH key should contain `\n` at the end of the key details in case of command-line or API calls. For more information about SSH keys, see [Adding an SSH key](/docs/ssh-keys?topic=ssh-keys-adding-an-ssh-key).
