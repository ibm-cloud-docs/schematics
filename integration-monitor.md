---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-05"

keywords: monitoring schematics services, monitoring, integration services

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.monitoringshort_notm}} integration
{: #monitoring-integration}

{{site.data.keyword.bpfull}} integrates to fully manage enterprise-grade {{site.data.keyword.monitoringfull}} that provides visibility of the performance and health of your {{site.data.keyword.cloud_notm}} powered resources application. This feature includes live logs, custom views, and alert of the {{site.data.keyword.bpshort}} Workspaces by connecting, configuring, and view through observability dashboards.
{: shortdesc}

## Launching monitoring
{: #monitoring-ui}

Use monitoring instance to monitor the health of the {{site.data.keyword.bplong_notm}} Workspace. To set up {{site.data.keyword.monitoringshort_notm}}, create a {{site.data.keyword.monitoringshort_notm}} instance in a public cloud region and plan associated to the resource. The region defines where your metrics are centralized. The plan specifies the features and retention period for your metrics. Complete these steps to launch {{site.data.keyword.monitoringshort_notm}}.
{: shortdesc}

1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials. 
2. From the {{site.data.keyword.cloud_notm}} page, select **Navigation menu** > **{{site.data.keyword.bpshort}}**.
3. Select **Integration** in the side navigation pane.
4. Against your monitoring instance, click **Configure** and select your instance name based on your location.
5. Click **Select**.
6. Select your location and click **Connect** > **Activity tracking**. You are redirected to the {{site.data.keyword.at_full_notm}} service form to monitor your instance.
7. Select your location and click **Connect** > **Monitoring**. You are redirected to the {{site.data.keyword.monitoringshort_notm}} service form to monitor your instance.
   
   In case you are redirected to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog) page, search for {{site.data.keyword.monitoringshort_notm}} to view the {{site.data.keyword.monitoringshort_notm}} service form.
   {: note}

8. Analyze the configuration and click **Create**.

    If the create is successful you can view logs of your {{site.data.keyword.bpshort}} service instance in the {{site.data.keyword.monitoringlong_notm}} that is configured to receive platform service logs. For more information about monitoring, see [Viewing logs](/docs/log-analysis?topic=log-analysis-monitor_logs).
    {: important}

9. In the list of instance name, click `Configure` to view `{{site.data.keyword.monitoringshort_notm}}` page to retrieve the instance summary details and click `Open Dashboard` to monitor the hosts and events of your services.
