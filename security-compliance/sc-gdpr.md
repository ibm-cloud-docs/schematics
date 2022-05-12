---

copyright:
  years: 2017, 2022
lastupdated: "2022-05-12"

keywords: audit access ibm schematics, supported classifications of personal data, personal data, sensitive personal data, restrictions on processing, encrypt data, data locations, service security, delete data

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# General Data Protection Regulation (GDPR)
{: #general-data-protection-regulation-gdpr}

The GDPR seeks to create a harmonized data protection law framework across the EU. It aims to give citizens back the control of their personal data, while it imposes strict rules on those who host and "process" this data, anywhere in the world. The Regulation also introduces rules that relate to the free movement of personal data within and outside the EU. 
{: shortdesc}

With the [General Data Protection Regulation](https://gdpr.eu/){: external}, {{site.data.keyword.bpshort}} customers can rely on
the {{site.data.keyword.bpshort}} team's understanding and compliance with emerging data privacy standards and legislation. Customers can also rely on {{site.data.keyword.IBM_notm}}'s wider ability to provide a comprehensive suite of solutions to assist businesses of all sizes with their own internal data governance requirements.

## How do I audit access to {{site.data.keyword.bpshort}}?
{: #how-do-i-audit-access-to-ibm-schematics}

You can find information about auditing in [Audit logging](/docs/schematics?topic=schematics-at_events) and [managing user access](/docs/schematics?topic=schematics-access#access-roles).

## Supported classifications of Personal Data
{: #supported-classifications-of-personal-data}

The following categories of **Personal Data** are supported by {{site.data.keyword.bpshort}}
for GDPR:

- Identity and civil status
- Personal life
- Professional life
- Location data
- Connectivity and device data

**Sensitive Personal Data** is restricted to the following category:

If you're storing healthcare data, you *must* complete the following tasks to notify {{site.data.keyword.bpshort}} before you write any data.


For more information about supported classifications of Personal Data, see the
[Securing your data in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-secure-data&interface=ui).

## Data about me
{: #data-about-me}

{{site.data.keyword.bpshort}} records some data about its users, and is a Data Controller for said
Personal Information (PI) data. The data that {{site.data.keyword.bpshort}} records depends on the type of account you have.

If you have an {{site.data.keyword.bpshort}} Dedicated Cluster or {{site.data.keyword.bpshort}} Enterprise Cluster, {{site.data.keyword.bpshort}} records data about you and are considered a Data Controller for your data within the context of GDPR. If you have an {{site.data.keyword.bpshort}} Dedicated Cluster or {{site.data.keyword.bpshort}} Enterprise Cluster, {{site.data.keyword.bpshort}} stores the following information about you:

- Name
- Email

The data that {{site.data.keyword.bpshort}} holds can be viewed and updated through the {{site.data.keyword.bpshort}} Dashboard.

If you have an account that is provisioned by {{site.data.keyword.cloud_notm}} (including a dedicated instance),
{{site.data.keyword.bpshort}} *does not* collect the personal data that was previously mentioned. This data is held by {{site.data.keyword.cloud_notm}}.

Do not use sensitive data for {{site.data.keyword.bpshort}} instance names when you provision by using {{site.data.keyword.cloud_notm}}, such as: Personal Information (PI), Personal Identifying Information (PII), and Customer-specific Data.
{: important}

{{site.data.keyword.bpshort}} processes limited customer PI in the course of running the service and optimizing the user experience of it. {{site.data.keyword.bpshort}} uses email for contacting customers. Monitoring customer interactions with the {{site.data.keyword.bpshort}} Dashboard is the other way {{site.data.keyword.bpshort}} processes PI.

### Restriction of processing
{: #restriction-of-processing}

{{site.data.keyword.bpshort}} sends dashboard interaction data to Segment. It's possible to ask {{site.data.keyword.bpshort}} to restrict processing of customer PI in this way with an {{site.data.keyword.bpshort}} support request through the [{{site.data.keyword.cloud_notm}} Support portal](https://www.ibm.com/cloud/support). Upon receipt of such a request, {{site.data.keyword.bpshort}} deletes information that is associated with the customer as sent to Segment, and prevents further data from being sent. {{site.data.keyword.bpshort}} needs to retain the ability to contact dedicated customers by email. {{site.data.keyword.bpshort}} provides an interface for customers to keep this 
information up to date either directly, or by using customer configuration of their contact details with their {{site.data.keyword.cloud_notm}} account details.

## Is the {{site.data.keyword.bpshort}} database encrypted?
{: #is-our-ibm-schematics-database-encrypted}

All clusters have an encrypted file system (encryption at rest) that uses Linux&trade; Unified Key Setup (LUKS). Data in the database is
visible to the operations and support teams (see the following paragraph).

For sensitive data, that you determine must remain invisible to {{site.data.keyword.bpshort}}, you must encrypt or otherwise protect (pseudonymize) your data before you send it to us. Do not use PI as a document `_id` in your URLs, since PI is always visible and written to the access logs.

## Data locations
{: #data-locations}

Locations where {{site.data.keyword.bpshort}} processes personal data are made available, and kept up to date. For more information about data locations, see the [Locations and service endpoints](/docs/schematics?topic=schematics-locations&interface=ui).

## Service security
{: #service-security}

### Using {{site.data.keyword.bpshort}} securely
{: #using-ibm-schematics-securely}

As a user of {{site.data.keyword.bpshort}}, you must follow these guidelines:

- Use the default CORS configuration to prevent unexpected access.
- Use API keys liberally, since components can have `least privileged access`, which is coupled with the audit log. This practice helps you understand who accessed which data.
- Encrypt or otherwise protect (pseudonymize) sensitive data that you determine must remain invisible to {{site.data.keyword.bpshort}}.

### Physical and environmental security measures
{: #physical-and-environmental-security-measures}

Physical security of our data centers is handled by the infrastructure providers: {{site.data.keyword.cloud}}. All hold externally audited certifications for their physical security. {{site.data.keyword.bpshort}} doesn't provide further details of the physical security controls in place at our data centers.

Physical security of the office locations that are used by our personnel is handled by {{site.data.keyword.IBM_notm}} Corporate.

### Technical and Organizational Measures
{: #technical-and-organizational-measures}

Technical and Organizational Measures (TOMs) are employed by {{site.data.keyword.bpshort}} to ensure the security of
Personal Data. {{site.data.keyword.bpshort}} holds externally audited certifications for the controls {{site.data.keyword.bpshort}} employs.

### Service access to data
{: #service-access-to-data}

{{site.data.keyword.bpshort}} operations and support staff has access to customer data and can access it during
routine operations. This access is only done as required in order to operate and support the service. Access is also limited to a *need to know* basis and is logged, monitored, and audited.

## Deletion of data
{: #deletion-of-data}

### Deleting a document
{: #deleting-a-document}

{{site.data.keyword.bplong_notm}} stores your data in a highly available and secure environment. All your data such as automation code, input configuration data, input credentials, and the runtime data are stored in IBM Cloud Object Storage. {{site.data.keyword.bplong_notm}} doesn't guarantee that a database is compacted in a specific time. Compaction is done as a background process across the storage tier. Databases are always being compacted. It isn't guaranteed that the data compacted is the data that you [deleted or changed](/docs/schematics?topic=schematics-delete-schematics-data-intro). 

### When is a deleted document removed?
{: #when-is-a-deleted-document-removed-}

Compaction runs automatically and periodically removes old revisions (deleted or otherwise) from the database. {{site.data.keyword.bpshort}} doesn't guarantee that a database is compacted in a specific time. Compaction is done as a background process across the storage tier. Databases are always being compacted. It isn't guaranteed that the data compacted is the data that you deleted or changed.

{{site.data.keyword.bpshort}} is accepting the *Right to be forgotten* requests through
the [{{site.data.keyword.IBM_notm}} Data Privacy Office (DPO)](http://w3.ibm.com/ibm/privacy/index.html){: external}.
When a *Right to be forgotten* request is made from the {{site.data.keyword.IBM_notm}} DPO, {{site.data.keyword.bpshort}} verifies the request, explicitly triggers database compaction, and verifies that compaction occurred.

### Removal of Objects
{: #removal-of-objects}

{{site.data.keyword.bpshort}} can completely remove all references and data for a document when required. This task is
an operator-managed process called purging. Before you request that documents be purged, it's important to understand that purged documents *cannot be recovered* by {{site.data.keyword.bpshort}} once the process is complete.

In the context of GDPR, purging is only required if PI is used in a document ID. It's a bad idea for an `_id` to store PI for lots of reasons, but a handful of semi-valid use cases exist (for example, a unique email). If possible, encrypt or pseudonymize data so it's opaque
to {{site.data.keyword.bpshort}}.

If a document needs removal through a *Right to be forgotten* request, follow these steps:

- File a request with the [{{site.data.keyword.IBM_notm}} DPO](http://w3-03.ibm.com/ibm/privacy/index.html){: external} to request purging of specific document `_id` values along with the reason.
- On receipt of a formal request by the {{site.data.keyword.IBM_notm}} DPO, {{site.data.keyword.bpshort}} operations verifies the request to confirm the `id` contains PI. {{site.data.keyword.bpshort}} doesn't purge data that doesn't have PI in the `_id`.
- {{site.data.keyword.bpshort}} triggers the purging action to permanently remove the requested data.

This process is only to be used for emergency deletion requests (for example, *Right to be forgotten*) and must not be relied upon long term. If your application is intentionally using PI in document IDs, then it must be changed to either pseudonymize that PI, or not use PI in document IDs. You cannot rely on regular purging by the {{site.data.keyword.bpshort}} operations team to avoid this situation.
Therefore, {{site.data.keyword.bpshort}} rejects the following purge requests:

- The request is for regular purging, for example, *every 30 days*.
- The request is for over 100 documents.

Even with purge, PI in the `_id` field leaks into places you don't want it, such as {{site.data.keyword.bpshort}} logs, so it must be avoided. {{site.data.keyword.bpshort}} has a business reason to keep those logs and doesn't remove log lines that include document `_id` values.

### What about deleting a database?
{: #what-about-deleting-a-database-}

Deleting a database adds it to the trash for up to 48 hours. After which time, the database is removed from the file system. The {{site.data.keyword.bpshort}} team *does not* make back ups of your databases; this task is the [responsibility of the customer](/docs/schematics?topic=schematics-responsibilities&interface=ui). You must ensure that all copies of your database are removed from your system.

If you need more help, go to the [{{site.data.keyword.cloud_notm}} Support portal](https://www.ibm.com/cloud/support).
