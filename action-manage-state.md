---

copyright:
  years: 2017, 2025
lastupdated: "2025-10-30"

keywords: schematics, schematics action state,  schematics actions state, ansible state,  schematics action,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Managing {{site.data.keyword.bpshort}} Action state
{: #action-state-diagram}

The {{site.data.keyword.bpshort}} action state indicates the result of creating and processing an action, which is recognized by the {{site.data.keyword.bpshort}} system. The following table represents the {{site.data.keyword.bpshort}} action states and their descriptions:
{: shortdesc}

|State|Description|
|------|-------|
| `Critical` | The template fails to download the repository or the repository name is invalid, resulting in a critical action state. |
| `Disabled` | Effectively halting its usage. |
| `Locked` | After configuration is in `Normal` state, an administrator can lock the action to prevent further changes. |
| `Normal` | An administrator publishes the action to make it visible for user execution. |
| `Pending` | When a user provides the template during creation, update, or deletion, the action enters the `Pending` state. |
{: caption="Action state" caption-side="top"}

## State diagram flow
{: #state-diagram-flow}

The following table represents the {{site.data.keyword.bpshort}} action state workflow:

| Action | State diagram | Description |
| ---- | ---- | ---- |
| **Create** | ![Create action](images/createaction.png "Create action state diagram") | The initial action state is `Draft`. Providing a template, either during creation or afterward, transitions the state to `Pending`. Once the template is successfully processed, the state changes to `Normal`. If an administrator disables the template, the state becomes `Disabled`. Any errors during template processing result in a `Critical` state. When a deletion is initiated, the action first enters a `Pending` state. If the template deletion fails, the status changes to `Critical`.|
| **Delete** | ![Delete action](images/deleteaction.png "Delete action state diagram")| Upon initiating a delete, the action goes to `Pending` state. Any failure the template results in `Critical` state.|
| **Update** | ![Update action](images/updateaction.png "Update action state diagram") | Upon initiating an update, the action immediately enters the `Pending` state. If the template is processed successfully, the state transitions to `Normal`. Setting the template repository to disable changes the state to `Disabled`. Any failures during template processing result in a `Critical` state. |
{: caption="Action state workflow" caption-side="top"}
