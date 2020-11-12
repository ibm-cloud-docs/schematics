---

copyright:
  years: 2017, 2020
lastupdated: "2020-11-12"

keywords: monitoring schematics services, schematics monitoring by using sysdig, auditing, key management, logging, integration services

subcollection: schematics

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# Monitoring integration resources by using LogDNA
{: #monitoring-integration}

{{site.data.keyword.cloud_notm}} Schematics integrates to fully manage enterprise-grade activity tracker service for logging, auditing, monitoring, and key management. This feature includes live logs, custom views, and alert of the Schemtics workspaces.
{: shortdesc}

## Launching log UI
{: #logging-ui}

You can manage your logging instances through the Schematics dashboard. Use IBM Log Analysis with LogDNA to gain insights into your workspace logs. You can choose the log retention log day as `7`,`14`,or `30` day and have the ability to archive to IBM Cloud Object Storage to retain your logs. LogDNA integrates with IBM access control to quickly integrate into your Schematics workspace. Complete the following steps to launch and analyze the log.
{: shortdesc}

1. Login to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials. 
2. From the {{site.data.keyword.cloud_notm}} page, select **Navigation menu** > **Schematics**.
3. Select **Integration** in the side navigation pane.
4. Select your location and click **Add logging**. You are redirected to the {{site.data.keyword.cloud_notm}} Log Analysis with LogDNA service form.
5. Analyze the configuration and click **Create**.
   If the create is successful you can view logs of your Schematics service instance in the Log Analysis instance that is configured to receive platform service logs. For detailed information on viewing logs, refer to [Viewing logs](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-view_logs).
   {: important}
6. In the list of instance name, click `Add logging` to view `Select an IBM Log Analysis with LogDNA instance to receive platform logs` page to retrieve the instance summary details and view the `Launch logging` hyperlink.
{: note}

## Launching audit UI
{: #audit-ui}

Use the add audit UI to generate and maintain an audit trail for a Schematics workspace instance events, access, events, and access audit log. Use the audit log to reveal usage patterns that would identify workspace misuse, and you can also take action to eliminate such misuse. You can choose the log retention log day as `7`,`14`,or `30` day and have the ability to archive to {{site.data.keyword.cloud_notm}} Object Storage to retain your logs. Complete the following steps to launch auditing.
{: shortdesc}

1. Login to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials. 
2. From the {{site.data.keyword.cloud_notm}} page, select **Navigation menu** > **Schematics**.
3. Select **Integration** in the side navigation pane.
4. Select your location and click **Add auditing**. You are redirected to the {{site.data.keyword.cloud_notm}} Activity Tracker with LogDNA service form.
5. Analyze the configuration and click **Create**.
   If the create is successful you can view logs of your Schematics service instance in the Log Activity Tracker that is configured to receive platform service logs. For detailed information on viewing logs, refer to [Viewing logs](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-view_logs).
   {: important}
6. In the list of instance name, click `Add auditing` to view `{{site.data.keyword.cloud_notm}} Activity Tracker with LogDNA` page to retrieve the instance summary details and view the `Launch auditing` hyperlink.


## Launching monitoring UI
{: #monitoring-ui}

Use Sysdig to monitor the health of the {{site.data.keyword.cloud_notm}} Schematics workspace. To set up monitoring, create a Sysdig instance in a public cloud region and plan associated to the resource. The region defines where your metrics are centralized. The plan specifies the features and retention period for your metrics. Complete the following steps to launch monitoring.
{: shortdesc}

1. Login to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials. 
2. From the {{site.data.keyword.cloud_notm}} page, select **Navigation menu** > **Schematics**.
3. Select **Integration** in the side navigation pane.
4. Select your location and click **Add monitoring**. You are redirected to the {{site.data.keyword.cloud_notm}} Monitoring with Sysdig form.
5. Analyze the configuration and click **Create**.
  If the create is successful you can view logs of your Schematics service instance in the Log Activity Tracker that is configured to receive platform service logs. For detailed information on viewing logs, refer to [Viewing logs](/docs/Log-Analysis-with-LogDNA?topic=Log-Analysis-with-LogDNA-view_logs).
  {: important}
6. In the list of instance name, click `Add monitoring` to view `{{site.data.keyword.cloud_notm}} Activity Tracker with LogDNA` page to retrieve the instance summary details and view the `Launch monitoring` hyperlink.

## Launching key management
{: #key-mgt-ui}

The data that you store in Schematics workspace when using the Enterprise plan is encrypted by default by using randomly generated keys. If you need to control the encryption keys, you can use the IBM Key Protect CLI plug-in to create, import, and manage encryption root keys and standard keys. Then, you can associate those keys with your Schematics resource deployment to encrypt your resources.
{: shortdesc}

This feature is only available in the CLI, refer to [Performing key management operations](/docs/hs-crypto?topic=hs-crypto-set-up-cli) to add key management services to your workspaces and actions.
{: note}