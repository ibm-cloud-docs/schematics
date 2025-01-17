---

copyright:
  years: 2018, 2025
lastupdated: "2025-01-17"

keywords: schematics activity tracker events, schematics events, schematics audit, schematics audit events, schematics audit logs, atracker, ibm cloud logs, cloud logs

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Activity tracking events for {{site.data.keyword.bpshort}}
{: #at_events}

You can view, manage, and audit user-initiated activities made in your {{site.data.keyword.bplong}} service instance by using the {{site.data.keyword.cloudaccesstraillong}} service. For more information, see [About {{site.data.keyword.atracker_full_notm}}](/docs/atracker?topic=atracker-about).
{: shordesc}

{{site.data.keyword.cloudaccesstraillong_notm}} records user-initiated activities that change the state of a service in {{site.data.keyword.cloud_notm}}. You can use this service to investigate abnormal activity and critical actions and to follow regulatory audit requirements. You can also be alerted about actions as they happen. The events that are collected follow the Cloud Auditing Data Federation (CADF) standard. For more information, see the [Getting Started tutorial for {{site.data.keyword.cloudaccesstraillong_notm}}](/docs/atracker?topic=atracker-getting-started).

As of 28 March 2024, the {{site.data.keyword.at_full_notm}} service is deprecated and will no longer be supported as of 30 March 2025. Customers will need to migrate to {{site.data.keyword.logs_full_notm}} before 30 March 2025. During the migration period, customers can use {{site.data.keyword.at_full_notm}} along with {{site.data.keyword.logs_full_notm}}. Activity tracking events are the same for both services. For information about migrating from {{site.data.keyword.at_full_notm}} to {{site.data.keyword.logs_full_notm}} and running the services in parallel, see [migration planning](/docs/cloud-logs?topic=cloud-logs-migration-intro).
{: important}

## Locations where activity tracking events are generated
{: #at-locations}

## Locations where activity tracking events are sent to {{site.data.keyword.at_full_notm}} hosted event search
{: #at-legacy-locations}

{{site.data.keyword.bpshort}} sends activity tracking events to {{site.data.keyword.at_full_notm}} hosted event search in the regions that are indicated in the following table.
{: shortdesc}

To monitor the service, [start the {{site.data.keyword.at_short}} UI](/docs/cloud-logs?topic=cloud-logs-cl-at-events) to access your events.

| Dallas (`us-south`) | Washington (`us-east`)  |
|---------------------|-------------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where activity tracker events are sent in Americas locations" caption-side="top"}
{: #at-table-1}
{: tab-title="Americas"}
{: tab-group="at"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`)  | London (`eu-gb`) |
|---------------|---------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where activity tracker events are sent in Europe locations" caption-side="top"}
{: #at-table-2}
{: tab-title="Europe"}
{: tab-group="at"}
{: class="simple-tab-table"}
{: row-headers}

## Locations where activity tracking events are sent by {{site.data.keyword.atracker_full_notm}}
{: #atracker-locations}

{{site.data.keyword.bpshort}} sends activity tracking events by {{site.data.keyword.atracker_full_notm}} in the regions that are indicated in the following table.

| Dallas (`us-south`) | Washington (`us-east`)  |  
|---------------------|-------------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} |
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

## Viewing activity tracking events for {{site.data.keyword.bpshort}}
{: #at-viewing}

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on events that are generated in your account and routed by {{site.data.keyword.atracker_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.

## Launching {{site.data.keyword.logs_full_notm}}
{: #sc-log-launch-standalone}

For information on launching the {{site.data.keyword.logs_full_notm}} UI, see [Launching the UI in the {{site.data.keyword.logs_full_notm}} documentation](/docs/cloud-logs?topic=cloud-logs-instance-launch).

## List of platform events
{: #at_actions_platform}

Following are the activity tracking event actions that the {{site.data.keyword.cloud_notm}} platform generates when {{site.data.keyword.bpshort}} instances are processed.

### Terraform (Workspace) events
{: #schematics-wks-events}

The following table lists the actions that generate an event for the {{site.data.keyword.bpshort}} Workspace that are associated with a service instance.

| Action             | Description      | 
| -------------------| -----------------|
| `schematics.workspace.read`| An event is generated for a request to view a {{site.data.keyword.bpshort}} workspace by a user.|
| `schematics.workspace.create` | An event is generated for a request to create a {{site.data.keyword.bpshort}} workspace. |
| `schematics.workspace.update`| An event is generated for a request to update a {{site.data.keyword.bpshort}} workspace. |
| `schematics.workspace.delete` | An event is generated for a request to delete a {{site.data.keyword.bpshort}} workspace. |
| `schematics.workspace-resources.create` | An event is generated when a Terraform execution apply is created for a workspace. |
| `schematics.workspace-resources.plan` | An event is generated when a Terraform execution plan is created for a workspace. |
| `schematics.workspace-resources.delete` | An event is generated for a request to delete the {{site.data.keyword.cloud_notm}} resources that are provisioned through a Terraform plan and the workspace.|
{: caption="Workspace events where activity tracking events are sent" caption-side="bottom"}

### Ansible events
{: #schematics-action-events}

The following table lists the actions that generate an event for the {{site.data.keyword.bpshort}} Ansible that are associated with a service instance.

| Action             | Description      |
| -------------------| -----------------|
| `schematics.action.create` | A {{site.data.keyword.bpshort}} action is created or failed to create. |
| `schematics.action.delete` | A {{site.data.keyword.bpshort}} action was deleted or failed to delete. |
| `schematics.action.read`| A user views the {{site.data.keyword.bpshort}} action.|
| `schematics.action.update`| A {{site.data.keyword.bpshort}} action is updated successfully or failed to update.|
{: caption="Action events where activity tracking events are sent" caption-side="bottom"}

### Job events
{: #schematics-job-events}

The following table lists the actions that generate an event for the {{site.data.keyword.bpshort}} job that are associated with a service instance.

| Action             | Description      |
| -------------------| -----------------|
| `schematics.job.create` | A {{site.data.keyword.bpshort}} job is created or failed to create. |
| `schematics.job.delete` | A {{site.data.keyword.bpshort}} job was deleted or failed to delete. |
| `schematics.job.read`| A user views the {{site.data.keyword.bpshort}} job.|
| `schematics.job.update`| A {{site.data.keyword.bpshort}} job is updated successfully or failed to update.|
{: caption="Job events" caption-side="bottom"}




## Analyzing {{site.data.keyword.bpshort}} activity tracking events
{: #at_events_analyze}

### Creating a workspace
{: #at_analyze_wks}

When you create your first workspace, the following events are sent to {{site.data.keyword.bpshort}} owned service ID and {{site.data.keyword.at_full_notm}}.
{: shortdesc}

When you manage a workspace, the {{site.data.keyword.bpshort}} service creates the following events:

- An event `schematics.instance.create`, when a first workspace is created.
- An event `schematics.instance.update`, when a workspace is modified.
- An event `schematics.instance.delete`, when a workspace is deleted.

The `initiatorId` of the request for these actions is set to a service ID where the {{site.data.keyword.bpshort}} service owns.

In addition, when a workspace is created, more events are also generated:

- Event `schematics.tag.attach` to report tagging of the workspace
- Event `schematics.instance.create` to report the creation of the workspace instance in your account
- Event `schematics.instance.update` to report updates to the workspace properties

You can search by `target.id` to identify all events that report actions on a workspace. For example, you can use a query, such as `crn:v1:bluemix:public:schematics:eu-de:a/xxxxxx:xxxxxxx:workspace:eu-de.workspace.observability-workspace.xxxxxxxx`.

Events that are generated by {{site.data.keyword.bpshort}} are automatically forwarded to your {{site.data.keyword.at_full_notm}} service instance based on the regions.
