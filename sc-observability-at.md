---

copyright:
  years: 2018, 2024
lastupdated: "2024-09-12"

keywords:

subcollection: schematics activity tracker events, schematics events, schematics audit, schematics audit events, schematics audit logs, atracker, ibm cloud logs, cloud logs,

---

{{site.data.keyword.attribute-definition-list}}

# Activity tracking events for {{site.data.keyword.bpshort}}
{: #at_events}

{{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.bpshort}}, generate activity tracking events.
{: shortdesc}

Activity tracking events report on activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use the events to investigate abnormal activity and critical actions and to comply with regulatory audit requirements.

You can use {{site.data.keyword.atracker_full_notm}}, a platform service, to route auditing events in your account to destinations of your choice by configuring targets and routes that define where activity tracking events are sent. For more information, see [About {{site.data.keyword.atracker_full_notm}}](/docs/atracker?topic=atracker-about).

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed by {{site.data.keyword.atracker_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.

As of 28 March 2024, the {{site.data.keyword.at_full_notm}} service is deprecated and will no longer be supported as of 30 March 2025. Customers will need to migrate to {{site.data.keyword.logs_full_notm}} before 30 March 2025. During the migration period, customers can use {{site.data.keyword.at_full_notm}} along with {{site.data.keyword.logs_full_notm}}. Activity tracking events are the same for both services. For information about migrating from {{site.data.keyword.at_full_notm}} to {{site.data.keyword.logs_full_notm}} and running the services in parallel, see [migration planning](/docs/cloud-logs?topic=cloud-logs-migration-intro).
{: important}

## Locations where activity tracking events are generated
{: #at-locations}



## Locations where activity tracking events are sent to {{site.data.keyword.at_full_notm}} hosted event search
{: #at-legacy-locations}

### Regions
{: #at}
{: #sch-region-events}

{{site.data.keyword.bpshort}} sends activity tracking events to {{site.data.keyword.at_full_notm}} hosted event search in the regions that are indicated in the following table.
{: shortdesc}

To monitor the service, [start the {{site.data.keyword.at_short}} UI](/docs/activity-tracker?topic=activity-tracker-launch#launch_cloud_ui) to access your events.

| Dallas (`us-south`) | Washington (`us-east`)  | Toronto (`ca-tor`) |
|---------------------|-------------------------|-------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where activity tracking events are sent in Americas locations" caption-side="top"}
{: #at-table-1}
{: tab-title="Americas"}
{: tab-group="at"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`)  | London (`eu-gb`) |
|---------------|---------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where activity tracking events are sent in Europe locations" caption-side="top"}
{: #at-table-3}
{: tab-title="Europe"}
{: tab-group="at"}
{: class="simple-tab-table"}
{: row-headers}

### Workspace events
{: #schematics-wks-events}

| Action             | Description      | 
| -------------------| -----------------|
| `schematics.workspace.read`| An event is generated for a request to view a {{site.data.keyword.bpshort}} workspace by a user.|
| `schematics.workspace.create` | An event is generated for a request to create a {{site.data.keyword.bpshort}} workspace. |
| `schematics.workspace.update`| An event is generated for a request to update a {{site.data.keyword.bpshort}} workspace. |
| `schematics.workspace.delete` | An event is generated for a request to delete a {{site.data.keyword.bpshort}} workspace. |
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
{: caption="`Shareddata events`" caption-side="bottom"}

### Other events
{: #schematics-otherevents}

| Action             | Description      |
| -------------------| -----------------|
| `schematics.credentials.ready-to-use` |  Credentials passed by a user as a workspace variable in the {{site.data.keyword.bpshort}} API request is being sent to {{site.data.keyword.cos_full_notm}} to complete the user’s action.|
{: caption="Other events" caption-side="bottom"}

## Locations where activity tracking events are sent by {{site.data.keyword.atracker_full_notm}}
{: #atracker-locations}

{{site.data.keyword.bpshort}} sends activity tracking events by {{site.data.keyword.atracker_full_notm}} in the regions that are indicated in the following table.

| Dallas (`us-south`) | Washington (`us-east`)  |  
|---------------------|-------------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} | 
{: caption="Regions where activity tracking events are sent in Americas locations" caption-side="top"}
{: #atracker-table-1}
{: tab-title="Americas"}
{: tab-group="atracker"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`)  | London (`eu-gb`) |
|---------------|-------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where activity tracking events are sent in Europe locations" caption-side="top"}
{: #atracker-table-3}
{: tab-title="Europe"}
{: tab-group="atracker"}
{: class="simple-tab-table"}
{: row-headers}

## Enabling activity tracking events for {{site.data.keyword.bpshort}}
{: #at-enable}






## Viewing activity tracking events for {{site.data.keyword.bpshort}}
{: #at-viewing}

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed by {{site.data.keyword.atracker_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.

### Launching {{site.data.keyword.logs_full_notm}} from the Observability page
{: #log-launch-standalone}

For information on launching the {{site.data.keyword.logs_full_notm}} UI, see [Launching the UI in the {{site.data.keyword.logs_full_notm}} documentation](/docs/cloud-logs?topic=cloud-logs-instance-launch).

## Analyzing {{site.data.keyword.bpshort}} activity tracking events
{: #at_events_analyze}

### Creating a workspace
{: #at_analyze_wks}

When you create your first workspace, the following events are created by a {{site.data.keyword.bpshort}} owned service ID and sent to {{site.data.keyword.at_full_notm}}.

When you manage a workspace, the following events are created by the {{site.data.keyword.bpshort}} service:

- An event with an action `schematics.instance.create`, when a first workspace is created.
- An event with an action `schematics.instance.update`, when a workspace is modified.
- An event with an action `schematics.instance.delete`, when a workspace is deleted.

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
