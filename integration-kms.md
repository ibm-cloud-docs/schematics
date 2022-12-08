---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-02"

keywords: monitoring schematics services, monitoring, integration services

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# KMS integration for BYOK or KYOK
{: #kms-integration}

{{site.data.keyword.bpfull}} integrates to fully manage enterprise-grade key management to manage the lifecycle of your encryption keys that are used in your {{site.data.keyword.cloud_notm}} resources, services, and applications.
{: shortdesc}

## Launching key management
{: #key-mgt-ui}

By default the data that you store in {{site.data.keyword.bpshort}} Workspaces by using the Enterprise plan is encrypted by using randomly generated keys. If you need to control the encryption keys, you can use the {{site.data.keyword.keymanagementservicelong_notm}} to create, import, and manage encryption root keys and standard keys. Then, you can associate those keys with your {{site.data.keyword.bpshort}} resource deployment to encrypt your resources. 
{: shortdesc}

You can use your encryption keys from key management services (KMS), {{site.data.keyword.keymanagementservicelong_notm}}(BYOK), and {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} (KYOK) to encrypt and secure data stored in {{site.data.keyword.bpshort}}. For more information about how to protect sensitive data in {{site.data.keyword.bpshort}}, see [protecting your sensitive data in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-secure-data#data-storage).

### Prerequisites
{: #kms-key-prerequisites}

The key management system will list the instance that are created from your specific location and region. Following prerequisites are followed to perform the KMS activity.

- You should have your `KYOK`, or `BYOK`. To create the {{site.data.keyword.keymanagementservicelong_notm}} keys, see [create KYOK](https://cloud.ibm.com/catalog/services/key-protect). To create an {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}} keys, see [create BYOK](https://cloud.ibm.com/catalog/services/hyper-protect-crypto-services).
- You need to [add root key](/docs/key-protect?topic=key-protect-import-root-keys#import-root-key-gui) to your `KYOK`, or `BYOK` instance.
- You need to configure [service to service authorization](/docs/account?topic=account-serviceauth#create-auth) to integrate `BYOK`, and `KYOK` in {{site.data.keyword.bpshort}} service. Follow these steps to grant service to service authorization {{site.data.keyword.keymanagementserviceshort}} access to {{site.data.keyword.bpshort}} service.
    * In the {{site.data.keyword.cloud_notm}} console, click **Manage** > **Access (IAM)**, and select **Authorizations** > **Create**.
    * Select a **Source Service** as **{{site.data.keyword.bpshort}}**.
    * Select **Target Service** as **{{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}}**. Select the instance you want to provide authorization.
    * Select the **Role** as **Reader**.
    * Click **Authorize**.

      For more information, see IAM authorization to create by using [CLI](/docs/account?topic=account-serviceauth#auth-cli), and [API](/docs/account?topic=account-serviceauth#create-auth).
      {: note}

KMS setting is a one time settings. You need to open the [support ticket](/docs/get-support?topic=get-support-using-avatar) to update KMS settings.
{: note}

### Enabling {{site.data.keyword.keymanagementservicelong_notm}} through UI
{: #integrate-byok-ui}
{: ui}

Follow these steps to launch key management system and encrypt your keys with {{site.data.keyword.bpshort}}.

1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials.
2. From the {{site.data.keyword.cloud_notm}} page, select **Navigation menu** > **{{site.data.keyword.bpshort}}** > **Integrations** > **Connect**. 
3. Click **Connect** > **Key Management** from the drop down.
4. Select **Service** as **{{site.data.keyword.keymanagementserviceshort}}**, or **{{site.data.keyword.hscrypto}}**.
5. Select an **Choose existing instance** instance. If your instance not created, select an **Create a new instance** to create {{site.data.keyword.keymanagementservicelong_notm}}, or {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}. For more information, see [Create a key protect instance](#kms-key-prerequisites).
    
    You can view your instance in the service list, when the prerequisites are met. Or you can see a message **No Keys** found.
    {: note}

6. Select your **Service** and **Root key** that is configured for BYOK or KYOK.
7. Click **Update** to complete the integration of your keys with your {{site.data.keyword.bpshort}} resource deployment.
8. Click **Launch** icon to view your enabled keys in the **Resource list**.

### Enabling {{site.data.keyword.keymanagementservicelong_notm}} through CLI
{: #integrate-byok-cli}
{: cli}

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

