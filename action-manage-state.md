---

copyright:
  years: 2017, 2025
lastupdated: "2025-10-27"

keywords: schematics, schematics action state,  schematics actions state, ansible state,  schematics action,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Managing {{site.data.keyword.bpshort}} Action state
{: #action-state-diagram}

The {{site.data.keyword.bpshort}} action state indicates the result of creating and processing an action, which is recognized by the {{site.data.keyword.bpshort}} system. The following table represents the {{site.data.keyword.bpshort}} action states and their descriptions.
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
| **Create** | ![Create action](images/createaction.png "Create action state diagram") | Initially, the action state is `Draft`. Providing a template during creation or after results in a `Pending` state. Successful template processing leads to a `Normal` state. Disabling the template by an administrator changes the state to `Disabled`. Template processing errors result in a `Critical` state. |
| **Delete** | ![Delete action](images/deleteaction.png "Delete action state diagram")| Initially, the action goes to `Pending` state upon deletion initiation. Template deletion failure results in a `Critical` state. |
| **Update** | ![Update action](images/updateaction.png "Update action state diagram") | Immediately enters the `Pending` state upon update initiation. Successful template processing results in a `Normal` state. Setting the template repository to disable changes the state to `Disabled`. Template processing failures lead to a `Critical` state. |
{: caption="Action state workflow" caption-side="top"}
