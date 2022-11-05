---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-05"

keywords: schematics activity tracker events, schematics events, schematics audit, schematics audit events, schematics audit logs

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Auditing events
{: #at_events}

You can use {{site.data.keyword.at_full}} to track and audit how users and applications interact with {{site.data.keyword.bplong}}.
{: shortdesc}

## {{site.data.keyword.bpshort}} events
{: #schematics-events}

{{site.data.keyword.at_full_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to comply with regulatory audit requirements. In addition, you can be alerted about actions as they happen. The events that are collected comply with the Cloud Auditing Data Federation (CADF) standard. For more information, see [Getting started tutorial for {{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started).

The following lists of {{site.data.keyword.bpshort}} events are sent to {{site.data.keyword.at_full_notm}}.
{: shortdesc}

### Workspace events
{: #schematics-wks-events} 

| Action             | Description      | 
| -------------------| -----------------|
| `schematics.workspace.read`| An event is generated for a request to view a {{site.data.keyword.bpshort}} Workspace by a user.|
| `schematics.workspace.create` | An event is generated for a request to create a {{site.data.keyword.bpshort}} Workspace. |
| `schematics.workspace.update`| An event is generated for a request to update a {{site.data.keyword.bpshort}} Workspace. |
| `schematics.workspace.delete` | An event is generated for a request to delete a {{site.data.keyword.bpshort}} Workspace. |
| `schematics.workspace-resources.create` | An event is generated when a Terraform execution apply is created for a workspace. |
| `schematics.workspace-resources.plan` | An event is generated when a Terraform execution plan is created for a workspace. |
| `schematics.workspace-resources.delete` | An event is generated for a request to delete the {{site.data.keyword.cloud_notm}} resources that are provisioned through a Terraform plan and the workspace.|
{: caption="Workspace events" caption-side="bottom"}

### Action events
{: #schematics-action-events} 

| Action             | Description      | 
| -------------------| -----------------|
| `schematics.action.create` | A {{site.data.keyword.bpshort}} action is created or failed to create. | 
| `schematics.action.delete` | A {{site.data.keyword.bpshort}} action was deleted or failed to delete. | 
| `schematics.action.read`| A {{site.data.keyword.bpshort}} action is viewed by a user.|
| `schematics.action.update`| A {{site.data.keyword.bpshort}} action is updated successfully or failed to update.|
{: caption="Action events" caption-side="bottom"}

### Job events
{: #schematics-job-events} 

| Action             | Description      | 
| -------------------| -----------------|
| `schematics.job.create` | A {{site.data.keyword.bpshort}} job is created or failed to create. | 
| `schematics.job.delete` | A {{site.data.keyword.bpshort}} job was deleted or failed to delete. | 
| `schematics.job.read`| A {{site.data.keyword.bpshort}} job is viewed by a user.|
| `schematics.job.update`| A {{site.data.keyword.bpshort}} job is updated successfully or failed to update.|
{: caption="Job events" caption-side="bottom"}

### `Shareddata` events
{: #schematics-shareddata-events} 

| Action             | Description      | 
| -------------------| -----------------|
| `schematics.shareddatas.create` | A {{site.data.keyword.bpshort}} shared data set was created or failed to create. |
| `schematics.shareddatas.delete` | A {{site.data.keyword.bpshort}} shared data set was deleted or failed to delete. |
| `schematics.shareddatas.update` | A {{site.data.keyword.bpshort}} shared data set was updated or failed to updated. |
{: caption="Shareddata events" caption-side="bottom"}


### Other events
{: #schematics-otherevents} 

| Action             | Description      | 
| -------------------| -----------------|
| `schematics.credentials.ready-to-use` |  Credentials passed by a user as a workspace variable in the {{site.data.keyword.bpshort}} API request is being sent to {{site.data.keyword.cos_full_notm}} to complete the user’s action.|
{: caption="Other events" caption-side="bottom"}

## Viewing events
{: #at_ui}

You can monitor the {{site.data.keyword.bpshort}} through any of the following regions only:
- `Dallas (us-south)`
- `Washington (us-east)`
- `Frankfurt (eu-de)` 
- `London (eu-gb)`

You must [create an {{site.data.keyword.at_short}} instance](/docs/activity-tracker?topic=activity-tracker-provision) in `Frankfurt`, `Dallas`, or both to monitor the {{site.data.keyword.bpshort}} service. 

| {{site.data.keyword.bpshort}} region | {{site.data.keyword.at_short}} region where events are available |
|----|----|
| `us-south` | `us-south` |
| `us-east` | `us-south` |
| `eu-de` | `eu-de` |
| `eu-gb` | `eu-de` |
{: caption="Location of events per region " caption-side="bottom"}

Events that are generated by {{site.data.keyword.bpshort}} are automatically forwarded to the {{site.data.keyword.at_short}} service.

To monitor the service, [start the {{site.data.keyword.at_short}} UI](/docs/activity-tracker?topic=activity-tracker-launch) to access your events.

## Analyzing events
{: #at_analyze}

### Creating a workspace
{: #at_analyze_1}

When you create your first workspace, the following events are created by a {{site.data.keyword.bpshort}} owned service ID and sent to {{site.data.keyword.at_full_notm}}.

When you manage a workspace, the following events are created by the {{site.data.keyword.bpshort}} service:
* An event with an action `schematics.instance.create`, when a first workspace is created.
* An event with an action `schematics.instance.update`, when a workspace is modified.
* An event with an action `schematics.instance.delete`, when a workspace is deleted.

The `initiatorId` of the request for these actions is set to a service ID that is owned by the {{site.data.keyword.bpshort}} service.

In addition, when a workspace is created, more events are also generated:
- Event with action `schematics.tag.attach` to report tagging of the workspace
- Event with action `schematics.instance.create` to report the creation of the workspace instance in your account
- Event with action `schematics.instance.update` to report updates to the workspace properties


You can search by `target.id` to identify all events that report actions on a workspace. For example, you can use a query such as, `crn:v1:bluemix:public:schematics:eu-de:a/xxxxxx:xxxxxxx:workspace:eu-de.workspace.observability-workspace.xxxxxxxx`.

Events that are generated by {{site.data.keyword.bpshort}} are automatically forwarded to your {{site.data.keyword.at_full_notm}} service instance based on the regions. {{site.data.keyword.bpshort}} sends events to the `us-south` or `eu-de` region only. You can create an instance of {{site.data.keyword.at_full_notm}} in the `us-south/eu-de` region to view event details. 
{: shortdesc}

1. Create a service instance of [{{site.data.keyword.at_full_notm}}](/docs/activity-tracker?topic=activity-tracker-getting-started) in the `us-south/eu-de` region. 
2. [Start the {{site.data.keyword.at_full_notm}} web console](/docs/activity-tracker?topic=activity-tracker-launch) to access your events.
