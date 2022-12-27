---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-27"

keywords: dbaas data protection, tier 1 physical platforms, secure access control, data loss, corruption, byok, encryption, protection 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Security
{: #security}

Use the powerful tools of {{site.data.keyword.bplong}} to build and spin up your {{site.data.keyword.cloud_notm}} environment, automate cloud resource operations, install software, and run multitiered apps on your cloud resources. The {{site.data.keyword.bplong_notm}} also ensures that your data stays secure and protected.

## Secure access control
{: #secure-access-control}

The list of feature in the table states {{site.data.keyword.bpshort}} has a multitude of built-in security features for you to control access to data.

|Feature | Description|
| ---- | -----|
|Authentication | {{site.data.keyword.bpshort}} is accessed by using the API endpoint. The user gets authenticated for every request that it receives. {{site.data.keyword.bpshort}} supports IAM access controls. For more information, see [Authentication](/apidocs/schematics/schematics#authentication).|
|Authorization | Use IAM roles to control access to {{site.data.keyword.bpshort}}. For more information, see the [Managing user access](/docs/schematics?topic=schematics-access).|
|At-rest encryption | All data that is stored in {{site.data.keyword.bpshort}} instance is encrypted using envelope encryption with AES GCM 256. By default, {{site.data.keyword.bpshort}} manages the encryption keys in its own Key Protect instances for all environments. If you require bring-your-own-key (BYOK) encryption for encryption-at-rest, BYOK is enabled by using your encryption key that is stored in an {{site.data.keyword.cloud_notm}} Key Protect instance in your account. For more information, see [KMS integration for BYOK or KYOK](/docs/schematics?topic=schematics-kms-integration#key-mgt-ui).|
|In-flight encryption | All access to {{site.data.keyword.bpshort}} is encrypted by using HTTPs.
|Public Endpoints | All {{site.data.keyword.bpshort}} instances are provided with [external endpoints](/docs/schematics?topic=schematics-secure-data#pi-location) that are publicly accessible. |
|Private Endpoints | Allows customers to connect to an {{site.data.keyword.bpshort}} instance through the internal {{site.data.keyword.cloud_notm}} network to avoid upstream application traffic from going over the public network and incurring bandwidth charges. For more information, see [Service Endpoint](/docs/schematics?topic=schematics-private-endpoints), and also see [enabling Service Endpoints](/docs/schematics?topic=schematics-secure-data#pi-location) for your {{site.data.keyword.cloud_notm}} account. |
|IP allows lists | You can allow the list of the {{site.data.keyword.bpshort}} IP addresses in their firewall. For more information, see [Opening wanted IP addresses](/docs/schematics?topic=schematics-allowed-ipaddresses) for {{site.data.keyword.bplong}} in your firewall.|
{: caption="{{site.data.keyword.bpshort}} security features" caption-side="bottom"}
