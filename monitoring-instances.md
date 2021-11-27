---

copyright:
  years: 2017, 2021
lastupdated: "2021-11-27"

keywords: monitoring schematics services, schematics monitoring, monitoring

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Monitoring {{site.data.keyword.bpshort}} services by using {{site.data.keyword.mon_full_notm}}
{: #monitoring-instances}

[{{site.data.keyword.mon_full_notm}}](/docs/monitoring?topic=monitoring-getting-started#getting-started) is a third-party cloud native, and container-intelligence management system that you can include as part of your IBM Cloud Schematics. Use it to gain operational visibility into the performance and health check of your applications, services, and platforms. It offers administrators, developers full stack telemetry with advanced features to monitor and troubleshoot, define alerts, and design custom dashboards.
{: shortdesc}

You need to create Sysdig monitor in `us-south` region to view the {{site.data.keyword.bpshort}} metrics that relates to workspaces, actions, or an environment that you create in `US` region. And create Sysdig monitor in `eu-de` region to view the {{site.data.keyword.bpshort}} metrics that relates to workspaces, actions, or an environment that you create in `EU` region.
{: note}



## Launching Monitoring UI from the {{site.data.keyword.cloud_notm}} 
{: #launch-dashboard}

You can monitor your services instance in the {{site.data.keyword.cloud_notm}} console.
{: shortdesc}

Complete these steps to view your services instances:

1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com){: external} account by using your credentials.
2. From the {{site.data.keyword.cloud_notm}} page, select `Navigation menu > Observability > Monitoring`.
3. From your instance, click **View Monitoring** icon to view the workspace and action that you created. **Note** If you want more information about how to create a service instance, refer to [Create service instance](#create-instance).
4. Click the `Dashboards` icon, and expand `IBM` to view the `IBM Schematics Summary Counts` and `IBM Schematics Summary Charts` dashboard list.
    - Use the `IBM Schematics Summary Counts` dashboard to monitor the counts regarding your workspace state, action, and its success and failure status.
    - Use the `IBM Schematics Summary Charts` dashboard to monitor the charts regarding your workspace by state, by type and outcome, and the vulnerability count.




## Creating service instance
{: #create-instance}

You can create your services instance in the {{site.data.keyword.cloud_notm}} console.
{: shortdesc}

Complete these steps to create your services instance:

1. Login to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com){: external} account by using your credentials.
2. From the {{site.data.keyword.cloud_notm}} page, select `Navigation menu > Observability > Monitoring`.
3. Click `Create instance` by using your plan.
4. Select a region, for example,  `Dallas`.
5. Create an {{site.data.keyword.mon_full_notm}} instance by using the `Lite plan`.
6. Click on `Configure platform metrics`, select the region and instance that you created to view the `Platform metrics` in the  `Region` column.
7. Click `View Sysdig` icon, to view your workspace and action that you created.
    
    You can monitor the status of your workspaces state and action through the {{site.data.keyword.cloud_notm}} dashboards. For more information, about monitoring the status, refer to [Monitoring workspace](#launch-dashboard). If you want to create custom dashboard, refer to [Creating custom dashboard](#create-dashboard).
    {: note}

## Creating custom dashboard
{: #create-dashboard}

You can create your custom dashboard in the {{site.data.keyword.cloud_notm}} console.
{: shortdesc}

Complete these steps to create your custom dashboard:

1. Login to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com){: external} account by using your credentials.
2. From the {{site.data.keyword.cloud_notm}} page, select `Navigation menu > Observability > Monitoring`.
3. Click `Create Custom Dashboard` to view the create dashboard from template pop-up.
4. Provide the name of the dashboard, and click `Create and Open`.
5. Click `Dashboards` icon, to view your dashboard.
    Now, you can use `your custom dashboard` to edit the metrics that you want to monitor, the counts of your workspace state, action, and its success and failure status. For more information on deleting a dashboard, refer to [Deleting a dashboards](/docs/monitoring?topic=monitoring-remove).
    {: note}

## {{site.data.keyword.bplong_notm}} metrics details
{: #metrics-details}

Schematics supports three metrics that you can use to configure in your dashboard for monitoring. The tables provides the details about the metrics.

| Metric name | Enterprise | Lite | Standard |
| --------| -------- | -------- | ------- |
| [ibm_schematics_workspace_actions_count](#wkspace-actions-count) | yes | no | yes |
| [ibm_schematics_workspace_count](#wkspace-actions-count) | yes | no | yes |
| [ibm_schematics_workspace_vulnerability_count](#wkspace-vulnerability-count) | yes | no | yes |
{: caption=“Metrics details” caption-side="bottom"}

### ibm_schematics_workspace_count
{: #wkspace-count}

The number of workspaces state and actions count are stated in the table.
{: shortdesc}

| Metadata | Description |
| ------- | -------- |
| `Metric Name` | `ibm_schematics_workspace_count` |
| `Metric Type` | `gauge` |
| `Frequency` | `60 seconds` |
| `Unit` | `none` |
| `Labels` | `ibm_schematics_workspace_status such as Active, Draft, Inactive, and Template Error are derived.` |
{: caption=“Workspace state and action count” caption-side="bottom"}

For the Schematics instance, following five different time series counts or charts are derived from `ibm_schematics_workspace_count` metric.

| Status | Query |
| ------ | -------- |
| Number of workspaces created  | Sum(ibm_schematics_workspace_count{}) |
| Number of active workspaces | ibm_schematics_workspace_count{ibm_schematics_workspace_status = ”Active”} |
| Number of workspaces in draft state |  ibm_schematics_workspace_count{ibm_schematics_workspace_status = ”Draft”} |
| Number of inactive workspaces | ibm_schematics_workspace_count{ibm_schematics_workspace_status = ”Inactive”} |
| Number of workspaces deleted | ibm_schematics_workspace_count{ibm_schematics_workspace_status = ”Template Error”} |
{: caption=“Worksapce time series count” caption-side="bottom"}

### ibm_schematics_workspace_actions_count
{: #wkspace-actions-count}

The number of workspace actions count are stated in the table.
{: shortdesc}

| Metadata | Description |
| ------- | -------- |
| `Metric Name` | `ibm_schematics_workspace_actions_count` |
| `Metric Type` | `gauge` |
| `Frequency` | `60 seconds` |
| `Unit` | `none` |
| `Labels` | `[ibm_schematics_workspace_action are apply, destroy, and plan ibm_schematics_workspace_action_status such as failure or success are supported.]` |
{: caption=“Workspace actions count” caption-side="bottom"}

For the Schematics instance, following six different time series counts or charts are derived from `ibm_schematics_workspace_actions_count` metric.

| Status | Query |
| ------ | -------- |
| Number of workspace actions  | Sum(ibm_schematics_workspace_actions_count {}) |
| Number of successful workspace actions | ibm_schematics_workspace_actions_count{ibm_schematics_workspace_action_status=”success”} |
| Number of failure workspace actions | ibm_schematics_workspace_actions_count{ibm_schematics_workspace_action_status=”failure”} |
| Number of plan actions| ibm_schematics_workspace_count{ibm_schematics_workspace_action = ”Plan”} |
| Number of successful plan actions |  ibm_schematics_workspace_count{ibm_schematics_workspace_action = ”Plan”, ibm_schematics_workspace_action_status=”success”} |
| Number of failure plan actions |  ibm_schematics_workspace_count{ibm_schematics_workspace_action = ”Plan”, ibm_schematics_workspace_action_status=”failure”} |
{: caption=“{{site.data.keyword.bpshort}} time series counts” caption-side="bottom"}

You can create similar queries to fetch `apply`, `destroy`, and `plan` actions.
{: note}

### ibm_schematics_vulnerability_count
{: #wkspace-vulnerability-count}

Average vulnerability count of the workspaces are stated in the table.
{: shortdesc}

| Metadata | Description |
| ------- | -------- |
| `Metric Name` | `ibm_schematics_workspace_vulnerability_count` |
| `Metric Type` | `gauge` |
| `Frequency` | `60 minutes` |
| `Unit` | `none` |
| `Labels` | `[ibm_schematics_vulnerability_count, such as average]`|
{: caption=“Vulnerability count” caption-side="bottom"}

For the Schematics instance, following time series counts and charts are derived from `ibm_schematics_workspace_vulnerability_count` metric.

| Status | Query |
| ------ | -------- |
| Number of workspace currently managed  | avg(avg(ibm_schematics_vulnerability_count)) |
{: caption=“Vulnerabilty count of {{site.data.keyword.bpshort}} workspace” caption-side="bottom"}
