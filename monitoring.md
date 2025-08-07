---

copyright:
  years: 2018, 2025
lastupdated: "2025-08-07"

keywords: IBM Cloud, observability, schematics monitoring events, schematics events, schematics audit, schematics audit events, schematics audit logs, atracker

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring metrics for {{site.data.keyword.bpshort}}
{: #monitoring}

{{site.data.keyword.cloud_notm}} services, such as {{site.data.keyword.bpshort}}, generate platform metrics that you can use to gain operational visibility into the performance and health of the service in your account.
{: shortdesc}

{{site.data.keyword.mon_full_notm}} is a third-party cloud-native, and container-intelligence management system that can be included as part of your {{site.data.keyword.cloud_notm}} architecture. It offers administrators, DevOps teams, and developers full-stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards. For more information, see [Monitoring in IBM Cloud](/docs/monitoring?topic=monitoring-about-monitor).

You can use {{site.data.keyword.metrics_router_full_notm}}, a platform service to route platform metrics in your account to a destination of your choice by configuring targets and routes that define where platform metrics are sent. For more information, see [About {{site.data.keyword.metrics_router_full_notm}} in {{site.data.keyword.cloud_notm}}](/docs/metrics-router?topic=metrics-router-about).

You can use {{site.data.keyword.mon_full}} to visualize and alert on metrics that are generated in your account and routed by {{site.data.keyword.metrics_router_full_notm}} to an {{site.data.keyword.mon_full_notm}} instance.

## Locations where metrics are generated
{: #mon-locations}

{{site.data.keyword.bpshort}} sends metrics in the regions that are indicated in the following table.

| Dallas (`us-south`) | Washington (`us-east`)  |
|---------------------|------------------------- |
| [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where  metrics are sent in Americas locations" caption-side="top"}
{: #at-table-1}
{: tab-title="Americas"}
{: tab-group="mon"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`)  | London (`eu-gb`) |
|---------------|---------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where metrics are sent in Europe locations" caption-side="top"}
{: #at-table-2}
{: tab-title="Europe"}
{: tab-group="mon"}
{: class="simple-tab-table"}
{: row-headers}

| Toronto (`ca-tor`)  | 
|---------------|
| [Yes]{: tag-green} | 
{: caption="Regions where metrics are sent in Toronto locations" caption-side="top"}
{: #at-table-3}
{: tab-title="Toronto"}
{: tab-group="mon"}
{: class="simple-tab-table"}
{: row-headers}

| Montreal (`ca-mon`)  | 
|---------------|
| [Yes]{: tag-green} | 
{: caption="Regions where metrics are sent in Montreal locations" caption-side="top"}
{: #at-table-4}
{: tab-title="Montreal"}
{: tab-group="mon"}
{: class="simple-tab-table"}
{: row-headers}

## Enabling platform metrics for {{site.data.keyword.bpshort}}
{: #monitoring-enable}

## Viewing metrics
{: #monitoring-view}

To monitor {{site.data.keyword.bpshort}} metrics, you must launch the {{site.data.keyword.mon_full_notm}} web UI for the instance that is enabled for platform metrics in the region where your {{site.data.keyword.bpshort}} instance is provisioned.
{: important}

### Launching and monitoring {{site.data.keyword.mon_full}} from the Observability page
{: #monitoring-view-ob}

You can monitor your services instance in the {{site.data.keyword.cloud_notm}} console. For more information about launching the {{site.data.keyword.mon_full_notm}} UI, see [Launching the UI in the {{site.data.keyword.mon_full_notm}} documentation](/docs/monitoring?topic=monitoring-launch#launch_step2) and [Monitoring in {{site.data.keyword.cloud_notm}}](/docs/monitoring?topic=monitoring-about-monitor).
{: shortdesc}

## Creating a custom dashboard
{: #create-dashboard}

You can create [custom dashboard in the web UI](/docs/monitoring?topic=monitoring-dashboards#dashboards_create) to edit the metrics that you want to monitor, the counts of your Terraform state, Ansible, and its success and failure status.
{: shortdesc}

   For more information about deleting a dashboard, see [Deleting a dashboard](/docs/monitoring?topic=monitoring-remove).
   {: note}

## {{site.data.keyword.bplong_notm}} metrics dictionary
{: #monitoring-metrics-dictionary}

{{site.data.keyword.bpshort}} supports three metrics that you can use to configure in your dashboard for monitoring. The tables provide the details about the metrics.

| Metric name | Enterprise | Lite | Standard |
| --------| -------- | -------- | ------- |
| [`ibm_schematics_workspace_actions_count`](#wkspace-actions-count) | Yes | No | Yes |
| [`ibm_schematics_workspace_count`](#wkspace-actions-count) | Yes | No | Yes |
| [`ibm_schematics_workspace_vulnerability_count`](#wkspace-vulnerability-count) | Yes | No | Yes |
{: caption="Metrics details" caption-side="bottom"}

### `ibm_schematics_workspace_count`
{: #wkspace-count}

The number of workspaces states and actions count is stated in the table.
{: shortdesc}

| Metadata | Description |
| ------- | -------- |
| `Metric Name` | `ibm_schematics_workspace_count` |
| `Metric Type` | `gauge` |
| `Frequency` | `60 seconds` |
| `Unit` | `none` |
| `Labels` | `ibm_schematics_workspace_status such as Active, Draft, Inactive, and Template Error are derived.` |
{: caption="Workspace state and action count" caption-side="bottom"}

For the {{site.data.keyword.bpshort}} instance, the following five different time series counts or charts are derived from `ibm_schematics_workspace_count` metric.

| Status | Query |
| ------ | -------- |
| Number of workspaces created  | `Sum(ibm_schematics_workspace_count{})` |
| Number of active workspaces | `ibm_schematics_workspace_count{ibm_schematics_workspace_status = ”Active”}` |
| Number of workspaces in draft state |  `ibm_schematics_workspace_count{ibm_schematics_workspace_status = ”Draft”}` |
| Number of inactive workspaces | `ibm_schematics_workspace_count{ibm_schematics_workspace_status = ”Inactive”}` |
| Number of workspaces deleted | `ibm_schematics_workspace_count{ibm_schematics_workspace_status = ”Template Error”}` |
{: caption="Workspace time series count" caption-side="bottom"}

### `ibm_schematics_workspace_actions_count`
{: #wkspace-actions-count}

The numbers of workspace actions that are counted are stated in the table.
{: shortdesc}

| Metadata | Description |
| ------- | -------- |
| `Metric Name` | `ibm_schematics_workspace_actions_count` |
| `Metric Type` | `gauge` |
| `Frequency` | `60 seconds` |
| `Unit` | `none` |
| `Labels` | `[ibm_schematics_workspace_action are apply, destroy, and plan ibm_schematics_workspace_action_status such as failure or success are supported.]` |
{: caption="Workspace actions count" caption-side="bottom"}

For the {{site.data.keyword.bpshort}} instance, the following six different time series counts or charts are derived from `ibm_schematics_workspace_actions_count` metric.

| Status | Query |
| ------ | -------- |
| Number of workspace actions  | `Sum(ibm_schematics_workspace_actions_count {})` |
| Number of successful workspace actions | `ibm_schematics_workspace_actions_count{ibm_schematics_workspace_action_status="success"}` |
| Number of failure workspace actions | `ibm_schematics_workspace_actions_count{ibm_schematics_workspace_action_status="failure"}` |
| Number of planned actions| `ibm_schematics_workspace_count{ibm_schematics_workspace_action = "Plan"}` |
| Number of successful plan actions |  `ibm_schematics_workspace_count{ibm_schematics_workspace_action = ”Plan”, ibm_schematics_workspace_action_status="success"}` |
| Number of failure plan actions |  `ibm_schematics_workspace_count{ibm_schematics_workspace_action = ”Plan”, ibm_schematics_workspace_action_status="failure"}` |
{: caption="{{site.data.keyword.bpshort}} time series counts" caption-side="bottom"}

You can create similar queries to fetch `apply`, `destroy`, and `plan` actions.
{: note}

### `ibm_schematics_vulnerability_count`
{: #wkspace-vulnerability-count}

The average vulnerability count of the workspaces is stated in the table.
{: shortdesc}

| Metadata | Description |
| ------- | -------- |
| `Metric Name` | `ibm_schematics_workspace_vulnerability_count` |
| `Metric Type` | `gauge` |
| `Frequency` | `60 minutes` |
| `Unit` | `none` |
| `Labels` | `[ibm_schematics_vulnerability_count, such as average]`|
{: caption="Vulnerability count" caption-side="bottom"}

For the {{site.data.keyword.bpshort}} instance, the following time series counts and charts are derived from `ibm_schematics_workspace_vulnerability_count` metric.

| Status | Query |
| ------ | -------- |
| Number of workspaces currently managed  | `avg(avg(ibm_schematics_vulnerability_count))` |
{: caption="Vulnerability count of {{site.data.keyword.bpshort}} workspace" caption-side="bottom"}
