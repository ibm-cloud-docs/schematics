---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-02"

keywords: logging schematics services, logging, integration services

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Logging integration
{: #logging-integration}

{{site.data.keyword.bpfull}} integrates to fully manage enterprise-grade {{site.data.keyword.loganalysislong}} that can store, search, analyze, monitor, and alert on your {{site.data.keyword.cloud_notm}} logging data and events. This feature includes live logs, custom views, and alert of the {{site.data.keyword.bpshort}} Workspaces by connecting, configuring, and view through observability dashboards.
{: shortdesc}

## Launching logging
{: #logging-ui}

You can manage your logging instances through the {{site.data.keyword.bpshort}} dashboard. Use {{site.data.keyword.la_full_notm}} to gain insights into your workspace logs. You can choose the log retention log day as `7`,`14`,or `30` days and have the ability to archive to an {{site.data.keyword.cos_full_notm}} to retain your logs. {{site.data.keyword.la_full}} integrates with IBM access control to quickly integrate into your {{site.data.keyword.bpshort}} Workspace. Complete these steps to launch and analyze the log.
{: shortdesc}

1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials. 
2. From the {{site.data.keyword.cloud_notm}} page, select **Navigation menu** > **{{site.data.keyword.bpshort}}**.
3. Select **Integrations** in the side navigation pane.
4. Against your logging instance, click **Configure** and select your instance name based on your location.
5. Click **Select**.
6. Select your location and click **Connect** > **Logging**. You are redirected to the {{site.data.keyword.la_full_notm}} service form to monitor your instance.

    In case you are redirected to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog) page, search for IBM Log Analysis to view the {{site.data.keyword.la_full_notm}} service form.
    {: note}

7. Analyze the configuration and click **Create**.

    If the create is successful you can view logs of your {{site.data.keyword.bpshort}} service instance in the Log Analysis instance that is configured to receive platform service logs. For more information about viewing logs, see [Viewing logs](/docs/log-analysis?topic=log-analysis-view_logs).
    {: important}

8. In the list of instance name, click `Configure` to view `Select an {{site.data.keyword.la_full_notm}} instance to receive platform logs` page to retrieve the instance summary details and click `Open Dashboard` to view your services.

