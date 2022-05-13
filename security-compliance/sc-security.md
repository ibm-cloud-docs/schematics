---

copyright:
  years: 2017, 2022
lastupdated: "2022-05-13"

keywords: dbaas data protection, tier 1 physical platforms, secure access control, data loss, corruption, byok, encryption, protection 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Security
{: #security}

Use the powerful tools of {{site.data.keyword.bplong}} to build and spin up your {{site.data.keyword.cloud_notm}} environment, automate cloud resource operations, install software, and run multitiered apps on your cloud resources. The {{site.data.keyword.bplong_notm}} also ensures your data stays secure and protected.

Review what data is stored and encrypted when you use {{site.data.keyword.bplong_notm}}, and how you can delete any personal data? 
{: shortdesc}

## Tier one physical platforms
{: #top-tier-physical-platforms}

The {{site.data.keyword.bpshort}} data is physically hosted on Tier-1 cloud infrastructure. Therefore, your data is protected by the network and physical security measures that are employed by those providers, including (but not limited to):

- Certifications - Compliance with SSAE16, SOC2 Type 1, ISAE 3402, ISO 27001, CSA, and other standards.
- Access and identity management.
- General physical security of data centers and network operations center monitoring.

More details about the certifications are available in the [Compliance information](/docs/security-compliance?topic=security-compliance-getting-started).
{: tip}

## Secure access control
{: #secure-access-control}

{{site.data.keyword.bpshort}} has a multitude of built-in security features for you to control access to data:

Feature | Description
--------|------------
Authentication | {{site.data.keyword.bpshort}} is accessed by using the API endpoint, the user is authenticated for every request {{site.data.keyword.bpshort}} receives. {{site.data.keyword.bpshort}} supports both legacy and IAM access controls. For more information, see the [IAM guide](/docs/account?topic=account-userroles) or the legacy [Authentication API document](/apidocs/schematics/schematics#authentication).
Authorization | {{site.data.keyword.bpshort}} supports both legacy and IAM access controls. The {{site.data.keyword.bpshort}} team recommends that you use IAM access controls for authentication whenever possible. If you're using {{site.data.keyword.bpshort}} legacy authentication, {{site.data.keyword.bpshort}} team recommends that you use [API keys](/apidocs/schematics/schematics#introduction){: external} rather than account-level credentials for programmatic access and replication jobs. For more information, see the [IAM guide](/docs/schematics?topic=schematics-access) or the legacy [Authentication document](/docs/schematics?topic=schematics-setup-api) and the legacy [Authorization document](/apidocs/schematics/schematics#authorization).
At-rest encryption | All data that is stored in an {{site.data.keyword.bpshort}} instance is encrypted at rest by using LUKS1 with 256-bit Advanced Encryption Standard (AES-256). By default, {{site.data.keyword.bpshort}} manages the encryption keys for all environments. If you require bring-your-own-key (BYOK) encryption for encryption-at-rest, BYOK is enabled by using your encryption key that is stored in an {{site.data.keyword.cloud_notm}} Key Protect instance. {{site.data.keyword.bpshort}} supports the BYOK feature for new {{site.data.keyword.bpshort}} Dedicated Hardware plan instances that are deployed in all regions. For more information, see the [Securing your data in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-secure-data) tutorial for details on how to choose BYOK at provisioning time. 
In-flight encryption | All access to {{site.data.keyword.bpshort}} is [encrypted](/docs/schematics?topic=schematics-secure-data#pi-encrypt) by using HTTPS.
Public Endpoints | All {{site.data.keyword.bpshort}} instances are provided with [external endpoints](/docs/schematics?topic=schematics-secure-data#pi-location) that are publicly accessible. 
Private Endpoints | All instances that you deploy on Dedicated Hardware plan environments also have private (internal) endpoints. Using private endpoints allows customers to connect to an {{site.data.keyword.bpshort}} instance through the internal {{site.data.keyword.cloud}} network to avoid upstream application traffic from going over the public network and incurring bandwidth charges. For more information, see [Service Endpoint documentation](/docs/schematics?topic=schematics-private-endpoints){: external}, and also, see documentation about [enabling Service Endpoints](/docs/schematics?topic=schematics-secure-data#pi-location) for your {{site.data.keyword.cloud}} account. If you want to allow only a subset of IP addresses to be able to access your application, refer to IP allowlisting in the next row.
IP allowlisting | {{site.data.keyword.bpshort}} customers who have an {{site.data.keyword.bpshort}} Dedicated Hardware plan environment can allowlist IP addresses to restrict access to only specified servers and users. IP allowlisting isn't available for any {{site.data.keyword.cloud_notm}} Public Lite or Standard plans that are deployed on multi-tenant environments. Open a support ticket to request an IP allowlist for a specific set of IP addresses or IP ranges. The [public and private network allowlists](/docs/schematics?topic=schematics-secure-data#pi-location) can be managed independently, and the public allowlist can be set to block all traffic so that all traffic is over the private endpoints. IP allowlists apply to both the {{site.data.keyword.bpshort}} API and Dashboard, so be mindful to include any administrator IP addresses that need to access the {{site.data.keyword.bpshort}} Dashboard directly. 
{: caption="Table 1. {{site.data.keyword.bpshort}} security features" caption-side="top"}

## Protection against data loss or corruption
{: #protection-against-data-loss-or-corruption}

{{site.data.keyword.bpshort}} has a number of features to help you maintain data quality and availability.

Feature | Description
--------|------------
Redundant and durable data storage | By default, {{site.data.keyword.bpshort}} has failover in secondary site cloudant. Saving the copies ensures that a working failover copy of your data is always available, regardless of failures.
Data replication and export | You do not have control on replication, it is autonomously done by the {{site.data.keyword.bpshort}}.
{: caption="Table 2. {{site.data.keyword.bpshort}} data quality and availability features" caption-side="top"}

For more information, see [Understanding high availability and disaster recovery for Schematics](/docs/schematics?topic=schematics-high-availability).
{: note}
