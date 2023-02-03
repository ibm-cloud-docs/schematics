---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-03"

keywords: monitoring schematics services, schematics monitoring by using monitoring, auditing, key management, logging, integration services

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring integration resources
{: #monitoring-integration}

{{site.data.keyword.bplong}} integrates to fully manage enterprise-grade activity tracker service instance for logging, activity tracking, monitoring, and key management. This feature includes live logs, custom views, and alert of the {{site.data.keyword.bpshort}} Workspaces by connecting, configuring, and view through observability dashboards.
{: shortdesc}

## Launching logging
{: #logging-ui}

You can manage your logging instances through the {{site.data.keyword.bpshort}} dashboard. Use {{site.data.keyword.la_full_notm}} to gain insights into your workspace logs. You can choose the log retention log day as `7`,`14`,or `30` days and have the ability to archive to an {{site.data.keyword.cos_full_notm}} to retain your logs. {{site.data.keyword.la_full}} integrates with IBM access control to quickly integrate into your {{site.data.keyword.bpshort}} Workspace. Complete these steps to launch and analyze the log.
{: shortdesc}

1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials. 
2. From the {{site.data.keyword.cloud_notm}} page, select **Navigation menu** > **{{site.data.keyword.bpshort}}**.
3. Select **Integrations** in the side navigation pane.
4. Select your logging instance.
5. Click **Configure** and select your instance name based on your location.
6. Click **Select**.
7. Click **Connect** > **Logging**. You are redirected to the {{site.data.keyword.la_full_notm}} service form to monitor your instance.

    In case you are redirected to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog) page, search for IBM Log Analysis to view the {{site.data.keyword.la_full_notm}} service form.
    {: note}

8. Analyze the configuration and click **Create**.

    If the create is successful you can view logs of your {{site.data.keyword.bpshort}} service instance in the Log Analysis instance that is configured to receive platform service logs. For more information about viewing logs, see [Viewing logs](/docs/log-analysis?topic=log-analysis-view_logs).
    {: important}

9. In the list of instance name, click `Configure` to view `Select an {{site.data.keyword.la_full_notm}} instance to receive platform logs` page to retrieve the instance summary details and click `Open Dashboard` to view your services.
{: note}

## Launching activity tracking
{: #audit-ui}

Use the add audit UI to generate and maintain an audit trail for a {{site.data.keyword.bpshort}} Workspaces instance events, access, events, and access audit log. Use the audit log to reveal usage instance that would identify workspace misuse, and you can act to eliminate such misuse. You can choose the log retention log day as `7`,`14`,or `30` days and have the ability to archive to {{site.data.keyword.cloud_notm}} Object Storage to retain your logs. Complete these steps to launch activity tracker.
{: shortdesc}

1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials. 
2. From the {{site.data.keyword.cloud_notm}} page, select **Navigation menu** > **{{site.data.keyword.bpshort}}**.
3. Select **Integrations** in the side navigation pane.
4. Select your location and click **Connect** > **Activity tracking**. You are redirected to the {{site.data.keyword.cloudaccesstraillong_notm}} service form.

   In case you are redirected to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog) page, search for {{site.data.keyword.cloudaccesstraillong_notm}} to view the {{site.data.keyword.cloudaccesstraillong_notm}} service form.
   {: note}

5. Analyze the configuration and click **Create**.
    
    If the create is successful you can view logs of your {{site.data.keyword.bpshort}} service instance in the Log Activity Tracker that is configured to receive platform service logs. For more information about viewing Activity tracker logs, see [Viewing logs](/docs/log-analysis?topic=log-analysis-at_events).
    {: important}

6. In the list of instance name, click `Configure` to view `{{site.data.keyword.at_full_notm}}` page to retrieve the instance summary details and click `Open Dashboard` to track your services.


## Launching monitoring
{: #monitoring-ui}

Use monitoring instance to monitor the health of the {{site.data.keyword.bplong_notm}} Workspace. To set up monitoring, create a Monitoring instance in a public cloud region and plan associated to the resource. The region defines where your metrics are centralized. The plan specifies the features and retention period for your metrics. Complete these steps to launch monitoring.
{: shortdesc}

1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials. 
2. From the {{site.data.keyword.cloud_notm}} page, select **Navigation menu** > **{{site.data.keyword.bpshort}}**.
3. Select **Integration** in the side navigation pane.
4. Select your location and click **Connect** > **Monitoring**. You are redirected to the {{site.data.keyword.cloud_notm}} Monitoring form.
   
   In case you are redirected to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog) page, search for {{site.data.keyword.monitoringlong}} to view the {{site.data.keyword.monitoringlong_notm}} service form.
   {: note}

5. Analyze the configuration and click **Create**.

    If the create is successful you can view logs of your {{site.data.keyword.bpshort}} service instance in the {{site.data.keyword.cloud_notm}} Monitoring that is configured to receive platform service logs. For more information about monitoring, see [Viewing logs](/docs/log-analysis?topic=log-analysis-monitor_logs).
    {: important}

6. In the list of instance name, click `Configure` to view `{{site.data.keyword.at_full_notm}}` page to retrieve the instance summary details and click `Open Dashboard` to monitor the hosts and events of your services.

## Launching key management
{: #key-mgt-ui}

The data that you store in {{site.data.keyword.bpshort}} Workspaces by using the Enterprise plan is encrypted by default by using randomly generated keys. If you need to control the encryption keys, you can use the {{site.data.keyword.keymanagementservicelong_notm}} to create, import, and manage encryption root keys and standard keys. Then, you can associate those keys with your {{site.data.keyword.bpshort}} resource deployment to encrypt your resources. 
{: shortdesc}

You can use your encryption keys from key management services (KMS), {{site.data.keyword.keymanagementservicelong_notm}}(BYOK), and {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} (KYOK) to encrypt and secure data stored in {{site.data.keyword.bpshort}}. For more information about how to protect sensitive data in {{site.data.keyword.bpshort}}, see [protecting your sensitive data in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-secure-data#data-storage).

### Prerequisites
{: #key-mgt-prerequisites}

The key management system will list the instance that are created from your specific location and region. Following prerequisites are followed to perform the KMS activity.

- You should have your `KYOK`, or `BYOK`. To create the {{site.data.keyword.keymanagementservicelong_notm}} keys, see [create KYOK](https://cloud.ibm.com/catalog/services/key-protect). To create an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} keys, see [create BYOK](https://cloud.ibm.com/catalog/services/hyper-protect-crypto-services).
- You need to [add root key](/docs/key-protect?topic=key-protect-import-root-keys#import-root-key-gui) to your `KYOK`, or `BYOK` instance.
- You need to configure [service to service authorization](/docs/account?topic=account-serviceauth#create-auth) to integrate `BYOK`, and `KYOK` in {{site.data.keyword.bpshort}} service. Follow these steps to grant service to service authorization {{site.data.keyword.keymanagementserviceshort}} access to {{site.data.keyword.bpshort}} service.
    * In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)**, and select **Authorizations** > **Create**.
    * Select a **Source Service** as **{{site.data.keyword.bpshort}}**.
    * Select **Target Service** as **Key Protect or {{site.data.keyword.hscrypto}}**. Select the instance you want to provide authorization.
    * Select the **Role** as **Reader**.
    * Click **Authorize**.

      For more information, see IAM authorization to create by using [CLI](/docs/account?topic=account-serviceauth#auth-cli), and [API](/docs/account?topic=account-serviceauth#create-auth).
      {: note}

KMS setting is a one time settings. You need to open the [support ticket](/docs/get-support?topic=get-support-using-avatar) to update KMS settings.
{: note}

### Enabling {{site.data.keyword.keymanagementservicelong_notm}} through UI
{: #integrate-byok-ui}

Follow these steps to launch key management system and encrypt your keys with {{site.data.keyword.bpshort}}.

1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials.
2. From the {{site.data.keyword.cloud_notm}} page, select **Navigation menu** > **{{site.data.keyword.bpshort}}** > **Integrations** > **Connect**. 
3. Click **Connect** > **Key Management** from the drop down.
4. Select **Service** as **Key Protect**, or **Hyper Protect Crypto Services**.
5. Select an **Choose existing instance** instance. If your instance not created, select an **Create a new instance** to create {{site.data.keyword.keymanagementservicelong_notm}}, or {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}. For more information, see [Create a key protect instance](#kms-key-prerequisites).
    
    You can view your instance in the service list, when the prerequisites are met. Or you can see a message **No Keys** found.
    {: note}

6. Select your **Service** and **Root key** that is configured for BYOK or KYOK.
7. Click **Update** to complete the integration of your keys with your {{site.data.keyword.bpshort}} resource deployment.
8. Click **Launch** icon to view your enabled keys in the **Resource list**.

### Enabling {{site.data.keyword.keymanagementservicelong_notm}} through CLI
{: #integrate-byok-cli}

Follow the steps to integrate root keys with {{site.data.keyword.bpshort}} to encrypt the data through command-line.

1. [Download and install command-line](/docs/cli?topic=cli-install-ibmcloud-cli).
2. List all the KMS instance in your {{site.data.keyword.cloud_notm}} account to find your {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} instances.
    ```sh
    ibmcloud schematics kms instance ls --location LOCATION_NAME --scheme ENCRYPTION_SCHEME
    ```
    {: pre}

3. Integrate the root key with {{site.data.keyword.bpshort}} to encrypt your data in the specified location.
    ```sh
    ibmcloud schematics kms enable --location LOCATION_NAME --scheme ENCRYPTION_SCHEME --group RESOURCE_GROUP --primary_name PRIMARY_KMS_NAME --primary_crn PRIMARY_KEY_CRN --primary_endpoint PRIMARY_KMSPRIVATEENDPOINT --secondary_name SECONDARY_KMS_NAME --secondary_crn SECONDARY_KEY_CRN --secondary_endpoint SECONDARY_KMSPRIVATEENDPOINT 
    ```
    {: pre}

4. Get current root key information.
    ``` sh
    ibmcloud schematics kms info --location LOCATION_NAME
    ```
    {: pre}

    For more information about enabling the `BYOK` or `KYOK` commands, see [Enable BYOK or KYOK commands](/docs/schematics?topic=schematics-schematics-cli-reference#kms-commands).
    {: note}

