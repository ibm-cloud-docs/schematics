---

copyright:
  years: 2017, 2024
lastupdated: "2024-08-29"

keywords: display resources with schematics, show resources, show schematics resources

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Displaying managed resources 
{: #resource-features}

The following features are supported in the {{site.data.keyword.bpshort}} workspaces resource view page.

- The resources related data are displayed in a tabular format.
- You can search for resources by using a table search.
- You can sort the resources by using a table sort.
- You can view the support of pagination.

The resources that are displayed are the Terraform resources defined for the workspace, this includes related {{site.data.keyword.cloud}}resources and Terraform null resources. Null resources are used by Terraform to allow execution of scripts and commands. They do not represent {{site.data.keyword.cloud}} resources.
{: note}

**The table lists the resource data displayed.**

| Data | Description |
| --- | --- |
| Resource | The name of the resource. The resource name can have a resource controller URL that appears as a anchor link, so that you can click on the link and navigate to the resource dashboard page. If a resource does not have a resource controller URL, it appears as plain text. The resource controller URL is derived from the Terraform state file. Note you cannot link to the resource directly, only to the resource list. |
| Type | The resource type. For example, `ibm_is_vpc`, `ibm_is_subnet`. |
| Taint status | States the resource is `degraded` or `damaged`. When a resource is tainted the value displayed are`Tainted`. If a resource is not `degraded` or `damaged`, empty value is displayed. |
| Resource group | Displays the [resource group](/docs/account?topic=account-rgs) that are used to provision your resource. |
| Tags | The [tags](/docs/account?topic=account-tag) that are attached to the resources. It can be a `user tag` or `Access management tag`. |
{: caption="{{site.data.keyword.bpshort}} resources" caption-side="bottom"}

The state information shown in the list depends on workspace status. Following are the supporting workspace states for the resource state.

| State | Description |
| -- | -- |
| `Not applied` | When your workspace state is in `Draft`, `Connecting`, or `Inactive`. In this state, your resources are yet to provision. You need to **Generate plan** and run **Apply plan** to provision the resources. |
| `Inprogress` | When your workspace state is in `Inprogress` means your job is in progress, and you need to check after sometime. |
| `Active`| When your workspace state is in `Active` means your apply job is successful. Also you can view the resource related data, else you would see `Your plan did not generate any resource` message. |
{: caption="{{site.data.keyword.bpshort}} workspace status versus resource state" caption-side="bottom"}
