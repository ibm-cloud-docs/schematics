---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-28"

keywords: audit access ibm schematics, supported classifications of personal data, personal data, sensitive personal data, restrictions on processing, encrypt data, data locations, service security, delete data

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# General Data Protection Regulation (GDPR)
{: #general-data-protection-regulation-gdpr}

The GDPR seeks to create a harmonized data protection law framework across the EU. It aims to give citizens back the control of their personal data, while it imposes strict rules on the host and process the data, anywhere in the world. The Regulation also introduces rules that relate to the movement of personal data within and outside the EU. 
{: shortdesc}

With the [General Data Protection Regulation](https://gdpr.eu/){: external}, {{site.data.keyword.bpshort}} customers can rely on
the {{site.data.keyword.bpshort}} team's understanding and compliance with emerging data privacy standards and legislation. Customers can also rely on {{site.data.keyword.IBM_notm}}'s wider ability to provide a comprehensive suite of solutions to assist businesses of all sizes with their own internal data governance requirements.

## How do you audit access to {{site.data.keyword.bpshort}}?
{: #how-do-i-audit-access-to-ibm-schematics}

You can find information about auditing in [Audit logging](/docs/schematics?topic=schematics-at_events) and [managing user access](/docs/schematics?topic=schematics-access#access-roles).
{: shortdesc}

## Supporting classifications of personal data
{: #supported-classifications-of-personal-data}

The following categories of personal data are supported by {{site.data.keyword.bpshort}} for GDPR:

- Basic contact information, such as email address, name, which is a subset of basic personal information.
- Technically identifiable personal information, such as authentication credentials, IP address.

For more information about data security in {{site.data.keyword.bpshort}}, see [Securing your data in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-secure-data).

## About user data
{: #data-about-me}

{{site.data.keyword.bpshort}} records few data about its users, which is limited to basic contact information such as email address, and name. {{site.data.keyword.bpshort}} is a data processor for said Personal Information (PI) data. {{site.data.keyword.bpshort}} processes the limited customer PI in the course of running the service and optimizing the user experience. {{site.data.keyword.bpshort}} uses email for contacting customers. Monitoring customer interactions with {{site.data.keyword.bpshort}} is another way {{site.data.keyword.bpshort}} processes PI. 

Do not enter sensitive data for {{site.data.keyword.bpshort}}. For example, do not use any Personal Information (PI), Personal Identifying Information (PII), and customer-specific data in a workspace name.

## Is the {{site.data.keyword.bpshort}} database encrypted?
{: #is-our-ibm-schematics-database-encrypted}

For more information about how your data is encrypted in {{site.data.keyword.bpshort}}? see [How your data is stored and encrypted in Schematics?](/docs/schematics?topic=schematics-secure-data#data-storage)

## Data locations
{: #data-locations}

Locations where {{site.data.keyword.bpshort}} processes personal data are made available, and kept up-to-date. For more information about data locations, see [Locations and service endpoints](/docs/schematics?topic=schematics-locations).

## Service security
{: #service-security}

Following are the list of service security measures taken by the {{site.data.keyword.bplong_notm}}.

- Physical and environmental security measures.
- Physical security of the data centers is handled by the {{site.data.keyword.cloud_notm}} infrastructure providers. All hold externally audited certifications for the physical security. {{site.data.keyword.bpshort}} doesn't provide further details of the physical security controls in place at the data centers.
- Physical security of an office location that are used personnel is handled by {{site.data.keyword.IBM_notm}} corporate.
- Technical and Organizational Measures. Technical and Organizational Measures (TOMs) are employed by {{site.data.keyword.bpshort}} to ensure the security of personal data. {{site.data.keyword.bpshort}} holds externally audited certifications for the controls {{site.data.keyword.bpshort}} employs.
- Service access to data.
- {{site.data.keyword.bpshort}} operations and support staff have access to customer data and can access during routine operations. The access is only done to operate, and support the service. Access is limited to a need to know basis and also is logged, monitored, and audited.

## Deletion of data
{: #deletion-of-data}

{{site.data.keyword.bplong_notm}} stores your data in a highly available and secure environment. All your data such as automation code, input configuration data, input credentials, and the runtime data are stored in {{site.data.keyword.cos_full}}. For more information about how to delete your data in {{site.data.keyword.bpshort}}? see [deleting {{site.data.keyword.bplong_notm}} data](/docs/schematics?topic=schematics-delete-schematics-data-intro).

{{site.data.keyword.bpshort}} can completely remove all references and data for a customer document when an operator-managed purging is run. Before you request that data to purge, it's important to understand that purged documents cannot recover the process is complete.

Follow these steps, if data needs to be removed through a right to be forgotten request.
- Raise a request with the [{{site.data.keyword.IBM_notm}} DPO](https://w3.ibm.com/ibm/privacy/){: external} to request purging of {{site.data.keyword.bpshort}} data that is related to a specific {{site.data.keyword.cloud_notm}} account along with the reason.
- On receipt of a formal request by the [{{site.data.keyword.IBM_notm}} DPO](https://w3.ibm.com/ibm/privacy/){: external}, {{site.data.keyword.bpshort}} operations verifies the request.
- {{site.data.keyword.bpshort}} triggers the purging action to permanently remove the requested data.

