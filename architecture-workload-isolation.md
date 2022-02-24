---

copyright:
  years: 2017, 2022
lastupdated: "2022-02-24"

keywords: schematics architecture, schematics compliance, schematics workload isolation, schematics depdendencies

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} architecture
{: #compute-isolation}

Learn about the {{site.data.keyword.bplong}} service architecture, the service dependencies, and how customer workloads are isolated from each other in {{site.data.keyword.bplong_notm}}?
{: shortdesc}

## Service architecture
{: #architecture}

The following image shows the main {{site.data.keyword.bplong_notm}} components, how they interact with each other, and what type of encryption is applied to your personal information. 
{: shortdesc}

![{{site.data.keyword.bplong_notm}} architecture and data encryption process](images/schematics_architecture.png){: caption="{{site.data.keyword.bplong_notm}} architecture and data encryption process" caption-side="bottom"}

1. A user sends a request to create an {{site.data.keyword.bplong_notm}} workspace to the {{site.data.keyword.bpshort}} API server.
2. The API server retrieves the Terraform template and input variables from your GitHub or GitLab source repository, or the tape archive file (`.tar`) that you uploaded from your local machine. 
3. All user-initiated actions, such as creating a workspace, generating a Terraform execution plan, or applying a plan are sent to RabbitMQ and added to the internal queue. RabbitMQ forwards requests to the {{site.data.keyword.bpshort}} engine to execute the action. 
4. The {{site.data.keyword.bpshort}} engine starts the process for provisioning, modifying, or deleting {{site.data.keyword.cloud}} resources. 
5. To protect customer data in transit, {{site.data.keyword.bplong_notm}} integrates with {{site.data.keyword.keymanagementserviceshort}}. {{site.data.keyword.bpshort}} uses root keys in {{site.data.keyword.keymanagementserviceshort}} to create data encryption keys (DEK). The DEK is then used to encrypt workspace transactional data, such as logs, or the Terraform `tf.state` file in transit. 
6. Workspace transactional data is stored in an {{site.data.keyword.cos_full_notm}} bucket and encrypted by using [Server-Side Encryption with {{site.data.keyword.keymanagementserviceshort}}](/docs/cloud-object-storage?topic=cloud-object-storage-encryption) at rest.  
7. Workspace operational data, such as the workspace variables and Terraform template information, is stored in {{site.data.keyword.cloudant}} and encrypted at rest by using the default service encryption. For more information, see [Security](/docs/Cloudant?topic=Cloudant-security).

## Workload isolation
{: #workload-isolation}

Review how your workloads are isolated in {{site.data.keyword.bplong_notm}}.
{: shortdesc}

### How are API requests to the service isolated from other API requests?
{: #workload-api-isolation}

All API requests to the {{site.data.keyword.bpshort}} API server are queued in Messages for RabbitMQ. RabbitMQ forwards these requests to the {{site.data.keyword.bpshort}} engine that processes these requests. At any given time, a maximum of n API requests are processed by the {{site.data.keyword.bpshort}} engine. By default, n equals 20, but this number is manually adjusted by the {{site.data.keyword.bpshort}} operator based on the current API workload. For every API request from a {{site.data.keyword.bpshort}} tenant, the {{site.data.keyword.bpshort}} engine creates a dedicated job that runs to completion and is then removed when the API request is fully processed. The {{site.data.keyword.bpshort}} jobs are not shared between tenants or reused later.

### How is the information in {{site.data.keyword.cloudant}} and {{site.data.keyword.cos_full_notm}} isolated from other tenant data?
{: #workload-info-isolation}

{{site.data.keyword.bpshort}} does not store any personal information, but stores sensitive technical information for a workspace as described in [What are the details stored in {{site.data.keyword.bpshort}}?](/docs/schematics?topic=schematics-secure-data#pi-data). All data that is stored in Cloudant and {{site.data.keyword.cos_full_notm}} is encrypted in transit and at rest by using the encryption mechanism.

### How are the workloads isolated from other tenants? 
{: #workload-tenant-isolation}

When you use {{site.data.keyword.bpshort}} to provision {{site.data.keyword.cloud}} resources, these resources are created in your personal {{site.data.keyword.cloud_notm}} account. You are responsible to manage these resources and to keep them up-to-date to avoid security vulnerabilities or downtime for your workloads. {{site.data.keyword.cloud_notm}} resources are provisioned, updated, and deleted as defined in the Terraform template and requested by the user. Because all resources are created in your personal account, resources are not shared with or reused by other {{site.data.keyword.cloud_notm}} tenants.

