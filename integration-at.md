---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-27"

keywords: tracking schematics services, activity tracking, integration services

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Activity tracking integration
{: #at-integration}

{{site.data.keyword.bpfull}} integrates to fully manage enterprise-grade {{site.data.keyword.at_full_notm}} that enables supervision, compliance, and audits the operations and risk to manage your {{site.data.keyword.cloud_notm}}. This feature includes live logs, custom views, and alert of the {{site.data.keyword.bpshort}} Workspaces by connecting, configuring, and view through observability dashboards.
{: shortdesc}

## Launching activity tracking
{: #audit-ui}

Use the add audit UI to generate and maintain an audit trail for a {{site.data.keyword.bpshort}} Workspaces instance events, access, events, and access audit log. Use the audit log to reveal usage instance that would identify workspace misuse, and you can act to eliminate such misuse. You can choose the log retention log day as `7`,`14`,or `30` days and have the ability to archive to {{site.data.keyword.cos_full_notm}} to retain your logs. Complete these steps to launch {{site.data.keyword.at_short}}.
{: shortdesc}

1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials. 
2. From the {{site.data.keyword.cloud_notm}} page, select **Navigation menu** > **{{site.data.keyword.bpshort}}**.
3. Select **Integrations** in the side navigation pane.
4. Against your activity tracking instance, click **Configure** and select your instance name based on your location.
5. Click **Select**.
6. Select your location and click **Connect** > **Activity tracking**. You are redirected to the {{site.data.keyword.at_full_notm}} service form to monitor your instance.

   In case you are redirected to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog) page, search for {{site.data.keyword.at_full_notm}} to view the {{site.data.keyword.at_full_notm}} service form.
   {: note}

5. Analyze the configuration and click **Create**.
    
    If the create is successful you can view logs of your {{site.data.keyword.bpshort}} service instance in the Log {{site.data.keyword.at_short}} that is configured to receive platform service logs. For more information about viewing {{site.data.keyword.at_short}} logs, see [Viewing logs](/docs/log-analysis?topic=log-analysis-at_events).
    {: important}

6. In the list of instance name, click `Configure` to view `{{site.data.keyword.at_full_notm}}` page to retrieve the instance summary details.
7. Click `Open Dashboard` to track your services.
