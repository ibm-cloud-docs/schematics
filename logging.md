---

copyright:
  years: 2018, 2024
lastupdated: "2024-09-14"

keywords:

subcollection: schematics logging, schematics events, schematics audit, schematics audit events, schematics audit logs, logging, ibm cloud logs, cloud logs

---

{{site.data.keyword.attribute-definition-list}}

# Logging for {{site.data.keyword.bpshort}}
{: #logging}

{{site.data.keyword.bpshort}} generate platform logs that you can use to investigate abnormal activity and critical actions in your account, and troubleshoot problems.
{: shortdesc}

You can use {{site.data.keyword.logs_routing_full_notm}}, a platform service to route platform logs in your account to a destination of your choice by configuring a tenant that defines where platform logs are sent. For more information, see [About Logs Routing](/docs/logs-router?topic=logs-router-about).

You can use {{site.data.keyword.logs_full_notm}} to visualize and alert on platform logs that are generated in your account and routed by {{site.data.keyword.logs_routing_full_notm}} to an {{site.data.keyword.logs_full_notm}} instance.

As of 28 March 2024, the {{site.data.keyword.la_full_notm}} service is deprecated and will no longer be supported as of 30 March 2025. Customers will need to migrate to {{site.data.keyword.logs_full_notm}} before 30 March 2025. During the migration period, customers can use {{site.data.keyword.la_full_notm}} along with {{site.data.keyword.logs_full_notm}}. Logging is the same for both services. For information about migrating from {{site.data.keyword.la_full_notm}} to {{site.data.keyword.logs_full_notm}} and running the services in parallel, see [migration planning](/docs/cloud-logs?topic=cloud-logs-migration-intro).
{: important}

## Locations where platform logs are generated
{: #log-locations}


### Locations where logs are sent to {{site.data.keyword.la_full_notm}}
{: #la-legacy-locations}

{{site.data.keyword.bpshort}} sends platform logs to {{site.data.keyword.la_full_notm}} in the regions indicated in the following table.

| Dallas (`us-south`) | Washington (`us-east`)  | Toronto (`ca-tor`) |
|---------------------|-------------------------|-------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where platform logs are sent in Americas locations" caption-side="top"}
{: #at-table-1}
{: tab-title="Americas"}
{: tab-group="la"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`)  | London (`eu-gb`) |
|---------------|---------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where platform logs are sent in Europe locations" caption-side="top"}
{: #at-table-2}
{: tab-title="Europe"}
{: tab-group="la"}
{: class="simple-tab-table"}
{: row-headers}

## Locations where logs are sent by {{site.data.keyword.logs_routing_full_notm}}
{: #lr-locations}

{{site.data.keyword.bpshort}} sends logs by {{site.data.keyword.logs_routing_full_notm}} in the regions that are indicated in the following table.

| Dallas (`us-south`) | Washington (`us-east`)  | Toronto (`ca-tor`) |
|---------------------|-------------------------|-------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where platform logs are sent in Americas locations" caption-side="top"}
{: #at-table-1}
{: tab-title="Americas"}
{: tab-group="lr"}
{: class="simple-tab-table"}
{: row-headers}

| Frankfurt (`eu-de`)  | London (`eu-gb`) |
|---------------|---------------------|
| [Yes]{: tag-green} | [Yes]{: tag-green} |
{: caption="Regions where platform logs are sent in Europe locations" caption-side="top"}
{: #at-table-2}
{: tab-title="Europe"}
{: tab-group="lr"}
{: class="simple-tab-table"}
{: row-headers}

## Platform logs that are generated
{: #log-platform}

{{site.data.keyword.bpshort}} fully manage enterprise-grade {{site.data.keyword.loganalysislong}} that can store, search, analyze, monitor, and alert on your {{site.data.keyword.cloud_notm}} logging data and events. This feature includes live logs, custom views, and alert of the {{site.data.keyword.bpshort}} workspaces by connecting, configuring, and viewing through observability dashboards.
{: shortdesc}

## Launching {{site.data.keyword.logs_full_notm}} from the Observability page
{: #log-launch-standalone}

For more information about launching the {{site.data.keyword.logs_full_notm}} UI, see [Launching the UI in the {{site.data.keyword.logs_full_notm}} documentation](/docs/cloud-logs?topic=cloud-logs-instance-launch#instance-launch-cloud-ui).

## Fields by log type
{: #log-fields}

For information about fields included in every platform log, see [Fields for platform logs](/docs/logs-router?topic=logs-router-about-platform-logs#platform_reqd). The {{site.data.keyword.bpshort}} follows the [guidelines for log types and log records](/docs/observability-ibm?topic=observability-ibm-log-record).

| Field             | Type       | Description             |
|-------------------|------------|-------------------------|
| `logSourceCRN`    | `Required`   | Defines the account and flow log instance where the log is published. |
| `saveServiceCopy` | `Required`   | Defines whether IBM saves a copy of the record for operational purposes. |
| `message`         | `Required`   | Description of the log that is generated. |
| `messageID`       | `Required`   | ID of the log that is generated. |
| `msg_timestamp`   | `Required`   | The timestamps when the log is generated. |
| `resolution`      | `Optional`   | Guidance on how to proceed if you receive this log record. |
| `documentsURL`    | `Optional`   | More information on how to proceed if you receive this log record. |
{: caption="Log record fields" caption-side="bottom"}

## Log messages
{: #log_messages}

The following tables list the message IDs that are generated and additional information available about these messages.

| Message ID | Type | Learn More |
|------------|------|------------|
| `is.flow-log-collector.00001E` | error | Failed to write a Flow log file for the past 24 hours. Dropping flow log for Virtual Server {{site.data.keyword.bpshort}}. |
{: caption="Additional information about message IDs" caption-side="top"}
