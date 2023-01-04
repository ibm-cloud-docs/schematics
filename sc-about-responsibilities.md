---

copyright:
  years: 2020, 2022
lastupdated: "2022-06-27"

keywords: schematics , hybrid, multi-cloud, RACI

subcollection: schematics 

---

{{site.data.keyword.attribute-definition-list}}


# Your responsibilities when using {{site.data.keyword.bpshort}} 
{: #sc-responsibilities}

Learn about the management responsibilities and terms and conditions that you have when you use {{site.data.keyword.bplong}}. For a high-level view of the service types in {{site.data.keyword.cloud_notm}} and the breakdown of responsibilities between you as the customer and {{site.data.keyword.IBM_notm}} for each type, see [Shared responsibilities for {{site.data.keyword.cloud_notm}} offerings](/docs/overview?topic=overview-shared-responsibilities).
{: shortdesc}

Review the following sections for the specific responsibilities for you and for {{site.data.keyword.IBM_notm}} when you use {{site.data.keyword.bplong_notm}}. For the overall terms of use, see [{{site.data.keyword.cloud_notm}} Terms and Notices](/docs/overview/terms-of-use?topic=overview-terms). For responsibilities that you have for other {{site.data.keyword.cloud_notm}} services that you use with {{site.data.keyword.bpshort}}, refer to the documentation of those services, such as [{{site.data.keyword.openshiftlong_notm}} responsibilities](/docs/openshift?topic=openshift-responsibilities_iks).

| Resource | Description | {{site.data.keyword.bpshort}} service | {{site.data.keyword.bpshort}} Agents |
| -- | -- | -- | -- |
| Data | Customer-owned content that includes all data that is managed and controlled by the customer. Examples include information that are stored into volumes, files, and databases hosted on {{site.data.keyword.cloud_notm}} resources and data processed, stored, and logged by the client applications hosted on {{site.data.keyword.cloud_notm}}. It doesn't include client metadata, the information that is used by {{site.data.keyword.IBM_notm}} to provide services to the customer support and operate the client account, services, and resources that are always considered to be shared responsibility between client and {{site.data.keyword.IBM_notm}}.| Customer-owned such as Templates, Git repository URL, input data, Terraform logs, state file. </br> Client metadata such as  Client email ID, workspace, action names. | Customer-owned content such as Agent data, Agent policies. </br> Client metadata such as Agent location, Agent name. |
| Applications | Customer-owned software components, such as executables, web applications, middleware, frameworks, libraries, and other software packages that the client developed or acquired by third parties and deployed in {{site.data.keyword.cloud_notm}}. | None | Customer-owned software components installed in Agents runtime. |
| Service instance | An entity that consists of resources that are reserved for a particular service. | {{site.data.keyword.bpshort}} service instance such as Workspaces, Actions, Inventories, Blueprints | {{site.data.keyword.bpshort}} service instance such as Agents instance. |
| Operating systems | The Operating System software and configuration that are deployed in virtual or bare metal servers, such as Linux, Windows, or similar to the ones provided in stock images.| [Universal Base Image (UBI-8)](https://catalog.redhat.com/software/containers/ubi8/ubi/5c359854d70cc534b3a3784e) | [Universal Base Image (UBI-8)](https://catalog.redhat.com/software/containers/ubi8/ubi/5c359854d70cc534b3a3784e) |
| Virtual and bare metal servers | The virtual or bare metal servers that are ordered and managed through {{site.data.keyword.cloud_notm}} services. | {{site.data.keyword.IBM_notm}} owns the IKS used by {{site.data.keyword.bpshort}}. | Customer manages the [IKS / ROKS / Kubernetes](/docs/containers?topic=containers-responsibilities_iks) cluster where Agents are deployed. |
| Virtual storage | The block, file, or Object Storage buckets ordered and managed through {{site.data.keyword.cloud_notm}}.| {{site.data.keyword.IBM_notm}} owns Cloudant, COS, RabbitMQ, Redis - used by {{site.data.keyword.bpshort}}  | Customer owns and manages the instances. {{site.data.keyword.IBM_notm}} owns the IKS local storage used by {{site.data.keyword.bpshort}}  Agents COS that are used by {{site.data.keyword.bpshort}}  Agents |
| Virtual network | Network resources such as VLAN, VPC, subnets, or `IPs` provided by classic infrastructure and VPC services that are ordered and managed through {{site.data.keyword.cloud_notm}}. | {{site.data.keyword.IBM_notm}} owned Network resources used by {{site.data.keyword.bpshort}}  |Customer owns and manages the network resources such as ingress, egress policies used by Agents. |
| Hypervisor | The software and configuration that is deployed in physical servers to host and manage the lifecycle of virtual servers. | {{site.data.keyword.IBM_notm}} owns the IKS used by {{site.data.keyword.bpshort}} | Customer owns [IKS / ROKS / Kubernetes](/docs/containers?topic=containers-responsibilities_iks) cluster only if customer uses cluster provided by {{site.data.keyword.IBM_notm}} IKS. |
| Physical servers and memory | The physical compute devices and resources, such as cores, memory, and GPUs used to host the virtual or bare metal servers. | {{site.data.keyword.IBM_notm}} owns the [IKS](/docs/containers?topic=containers-responsibilities_iks) used by {{site.data.keyword.bpshort}}  | Customer owns, if cluster provided by Customer. {{site.data.keyword.IBM_notm}} owns if the cluster is provided by [IKS / ROKS / Kubernetes](/docs/containers?topic=containers-responsibilities_iks). |
| Physical storage | The physical storage devices and resources, such as disks and storage devices that are used to host the virtual block, file, or Object Storage buckets. | {{site.data.keyword.IBM_notm}} owns the [IKS](/docs/containers?topic=containers-responsibilities_iks) used by {{site.data.keyword.bpshort}}  | Customer owns, if cluster provided by Customer. {{site.data.keyword.IBM_notm}} owns if the cluster is provided by [IKS / ROKS / Kubernetes](/docs/containers?topic=containers-responsibilities_iks). |
| Physical network and devices | The physical network devices and resources, such as switches, routers, gateways, firewalls, and load balancers that are used to host the virtual network resources. | {{site.data.keyword.IBM_notm}} owns the [IKS](/docs/containers?topic=containers-responsibilities_iks) used by {{site.data.keyword.bpshort}}  | Customer owns, if cluster provided by Customer. {{site.data.keyword.IBM_notm}} owns if the cluster is provided by [IKS / ROKS / Kubernetes](/docs/containers?topic=containers-responsibilities_iks). |
| Facilities and data centers | The physical data center buildings with power, cooling, and rooms for all the {{site.data.keyword.cloud_notm}} physical equipment.| {{site.data.keyword.IBM_notm}} owns the [IKS](/docs/containers?topic=containers-responsibilities_iks) used by {{site.data.keyword.bpshort}}  | Customer owns, if cluster provided by Customer. {{site.data.keyword.IBM_notm}} owns if the cluster is provided by [IKS / ROKS / Kubernetes](/docs/containers?topic=containers-responsibilities_iks). |
{: caption="Shared responsibilities for the managed products" caption-side="top"}

## Incident and operations management
{: #incident-ops-mgt}

Includes tasks such as monitoring, event management, high availability, problem determination, recovery, and full state backup and recovery.

## Change management
{: #change-mgt}

Includes tasks such as deployment, configuration, upgrades, patching, configuration changes, and deletion.

## Identity and access management
{: #iam}

Includes tasks such as authentication, authorization, access control policies, and approving, granting, and revoking access.

## Security and regulation compliance
{: #security-reg-compliance}

Includes tasks such as security controls implementation and compliance certification.

## Disaster recovery
{: #disaster-recovery}

Includes tasks such as providing dependencies on disaster recovery sites, provision disaster recovery environments, data and configuration backup, replicating data and configuration to the disaster recovery environment, and failover on disaster events.
