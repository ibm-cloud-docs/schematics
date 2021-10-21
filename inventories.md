---

copyright:
  years: 2017, 2021
lastupdated: "2021-10-21"

keywords: schematics inventory, ansible inventory, inventories, ibm cloud schematics inventories

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Creating resource inventories for {{site.data.keyword.bpshort}} actions
{: #inventories-setup}

A resource inventory defines a single {{site.data.keyword.cloud_notm}} resource or a group of resources where you want to run Ansible playbooks, modules, or roles by using {{site.data.keyword.bpshort}} actions.  
{: shortdesc}

You can specify your resource inventory by using a [static inventory file](#static-inv), or [dynamically retrieve](#dynamic-inv) your target {{site.data.keyword.cloud_notm}} resources from {{site.data.keyword.bpshort}} workspaces that you created.  

## Creating static inventory files
{: #static-inv}

{{site.data.keyword.bpshort}} supports the definition of `hosts.ini` files where you specify a single target host or a group of target hosts by using their hostnames or IP addresses. You can assign names to a group of target hosts, such as `[webserver]`, and use this name in your Ansible playbook to instruct {{site.data.keyword.bpshort}} where to run the playbook tasks.
{: shortdesc}

1. From the [{{site.data.keyword.bpshort}} inventories dashboard](https://cloud.ibm.com/schematics/inventories){: external}, click **Create Inventory**. 
2. Enter a name for your inventory, and select the location and resource group where you want to create the inventory. 
3. Select the **Create file** tab. 
4. In the **File** field, enter the target hosts where you want to run the Ansible playbook. Make sure to specify your hosts in an `INI` format. For a sample file, see [File format](#inv-file-format). Review the [limitations](#inv-file-limitation) to ensure that your inventory definition is supported in {{site.data.keyword.bpshort}}.
5. Click **Create inventory**. 
6. Follow the [steps](/docs/schematics?topic=schematics-action-setup#create-action) to create a {{site.data.keyword.bpshort}} action and use the resource inventory that you created. 


### File format
{: #inv-file-format}

Review the following sample `hosts.ini` file to see the structure of your static host inventory list. For more information about `hosts.ini` file structures, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html#how-to-build-your-inventory){: external}.

```
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
{: codeblock}


### Limitations
{: #inv-file-limitation}

Review the following limitations of static inventory files in {{site.data.keyword.bpshort}}: 

- {{site.data.keyword.bpshort}} supports the `INI` file format only.
- Variables are not supported in `hosts.ini` files.
- Specifying host groups by using key-value pairs in `hosts.ini` files is not supported.
- You must manually update the `hosts.ini` file if hostnames or IP addresses of target hosts change.
- All target hosts must be configured with the same public SSH key. When you use the static inventory file in your {{site.data.keyword.bpshort}} action, you can specify one SSH key only to authenticate with all target hosts that are included in your resource inventory. The SSH key should contain `\n` at the end of the key details in case of command-line or API calls.


## Dynamically building resource inventories from {{site.data.keyword.bpshort}} workspaces
{: #dynamic-inv}

You can dynamically build your resource inventory from the {{site.data.keyword.cloud_notm}} resources that you provisioned with {{site.data.keyword.bpshort}} workspaces. 
{: shortdesc}

Dynamic resource inventories reference {{site.data.keyword.cloud_notm}} resources that you provisioned with {{site.data.keyword.bpshort}} workspaces. To retrieve the {{site.data.keyword.cloud_notm}} resources from your workspaces, you use pre-defined resource queries. You do not need to keep track of the IP addresses that were assigned to your target resources as {{site.data.keyword.bpshort}} automatically determines these when you use this inventory in a {{site.data.keyword.bpshort}} action. 

1. From the [{{site.data.keyword.bpshort}} inventories dashboard](https://cloud.ibm.com/schematics/inventories){: external}, click **Create Inventory**. 
2. Enter a name for your inventory, and select the location and resource group where you want to create the inventory. 
3. Select the **Host groups** tab.
4. Click **Select host group**. 
5. Enter a name for your host group and select the {{site.data.keyword.bpshort}} workspace that provisioned the target hosts that you want to add to your host group.
6. Optional: Click **Add query** to add another condition to your query and limit the number of target hosts that are added to your host group. You can choose to identify your hosts by using the resource name or a tag. For more information, see [supported resource queries](#supported-queries).
7. Repeat step 4-6 to add more host groups. 
8. From the **Filter table**, select the host groups that you want to include in your resource inventory.
9. Click **Create inventory**. 
10. Follow the [steps](/docs/schematics?topic=schematics-action-setup#create-action) to create a {{site.data.keyword.bpshort}} action and use the resource inventory that you created. 

### Supported resource queries
{: #supported-queries}

|Supported query|Description|
|--|--|
|Workspace name|Select all {{site.data.keyword.cloud_notm}} resources from a specific workspace.|
|Workspace name AND resource name|Select a specific resource from a specific workspace by using the resource name. To select multiple resources from the same workspace, you can add multiple queries of this type. |
|Workspace name AND tag|Select a specific resource from a specific workspace by using tags. Tags are added to the resource when you create the workspace and provision your resource. To select multiple resources with different tags from the same workspace, you can add multiple queries of this type. |

### Limitations
{: #dynamic-inv-limitation}

Review the following limitations of dynamic inventories in {{site.data.keyword.bpshort}}: 

- You can choose among the [supported queries](#supported-queries) to select the target virtual server instances that you want to include in your resource inventory only.
- {{site.data.keyword.bpshort}} retrieves the IP address of a target virtual server instances and adds this IP address to the resource inventory. Hostnames cannot be added, if a public IP address is assigned to the target virtual server instance, the public IP address is added to the resource inventory. If no public IP address exists, the private IP address is added to the resource inventory.
- All target hosts must be configured with the same public SSH key. When you use the static inventory file in your {{site.data.keyword.bpshort}} action, you can specify one SSH key only to authenticate with all target hosts that are included in your resource inventory. The SSH key should contain `\n` at the end of the key details in case of command-line or API calls.




