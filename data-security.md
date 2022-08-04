---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-04"

keywords: byok and kyok, schematics byok, schematics kyok, key management service 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Securing your data in {{site.data.keyword.bpshort}}
{: #secure-data}

Review what data is stored and encrypted when you use {{site.data.keyword.bplong_notm}}, and how you can delete any personal data. 
{: shortdesc}

To ensure that you can securely manage your data when you use {{site.data.keyword.bpshort}}, it is important to know exactly what data is stored and encrypted and how you can delete any stored data. Data encryption by using customer-managed keys is supported by using {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}} with {{site.data.keyword.bpshort}}.
{: shortdesc}

## How your data is stored and encrypted in {{site.data.keyword.bpshort}}?
{: #data-storage}



All data, user inputs and the data generated at runtime during execution of automation code, are stored in {{site.data.keyword.cos_full_notm}}. This data is encrypted at rest by [envelope encryption](#x9860393){: term} technique by using a root key selected for each geographical location. The root keys are secured by FIPS 140-2 Level 3 certified cloud-based [hardware security modules (HSMs)](#x6704988){: term}. {{site.data.keyword.bpshort}} support encryption with the root keys by using following encryption.

1. {{site.data.keyword.bpshort}} owned root key.
2. Bring your own key (BYOK) by integrating with Key Protect.
3. Keep your own key (KYOK) by integrating with Hyper Protect Crypto Services (HPCS)

Key protect offers manual and automatic key rotation. When you rotate a root key, the registered key is used to re-encrypted {{site.data.keyword.bpshort}} resources with a new key version. You can access the {{site.data.keyword.bpshort}} resources metadata such as details till the rotation completes.
{: shortdesc}

### Key deletion or disable
{: #key-delete}


Key Deletion is a destructive action. When you disable or delete a root key that is used to encrypt your {{site.data.keyword.bpshort}} resources, you cannot access transactional data such as activity or job logs, resource list, variable store. However, you can access the metadata details. Furthermore any subsequent deployment or configuration operation through {{site.data.keyword.bpshort}} will result in failure. Key deletion or disable events are sent to the {{site.data.keyword.la_full_notm}} {{site.data.keyword.at_short}}.
{: shortdesc}

### Key enable or restore
{: #key-enable}



When you can [enable or restore a root key](/docs/schematics?topic=schematics-kms-integration#key-mgt-ui), the {{site.data.keyword.bpshort}} resources transactional data that are inaccessible due to disabled or deleted root key is now completely accessible. You can also use {{site.data.keyword.bpshort}} resources for deployment or configuration operations. Key enable or restore events are sent to the {{site.data.keyword.la_full_notm}} {{site.data.keyword.at_short}}. 
{: shortdesc}



## What are the details stored in {{site.data.keyword.bpshort}}?
{: #pi-data}

The following details are stored when you create and use a {{site.data.keyword.bpshort}} workspace: 
- Workspace details
- Workspace variables
- Terraform configuration files that your workspace points to
- Terraform state files
- Terraform log files
- User activity logs

## Where is my information stored?
{: #pi-location}

By default, all information that is stored in {{site.data.keyword.bpshort}} is encrypted in transit and at rest. To make your data highly available, all data is stored in one location and replicated to another location in the same geography. Make sure that your data can be stored in these locations before you start {{site.data.keyword.bpshort}}.
{: shortdesc} 

|Geography/ location| API endpoint|Data is stored in|Data is replicated to|
|------------|----------------|------|--------|
|North America|**Public**: </br>`https://us.schematics.cloud.ibm.com` </br></br>`https://cloud.ibm.com/schematics/overview` </br></br> **Private**: </br> `https://private-us.schematics.cloud.ibm.com` (Deprecated) |Workspaces that are created with this endpoint and all associated data are stored in the US. | Data is replicated between two locations in the US.|
|Dallas|**Public**: </br>`https://us-south.schematics.cloud.ibm.com` </br></br> **Private**: </br> `https://private-us-south.schematics.cloud.ibm.com` |Workspaces that are created with this endpoint and all associated data are stored in the Dallas location.|Data is replicated between two locations in the US.|
|Washington|**Public**: </br>`https://us-east.schematics.cloud.ibm.com` </br></br> **Private**: </br> `https://private-us-east.schematics.cloud.ibm.com` |Workspaces that are created with this endpoint and all associated data are stored in the Washington location.|Data is replicated between two locations in the US.|
|Europe|**Public**: </br> `https://eu.schematics.cloud.ibm.com` </br></br> **Private**: </br> `https://private-eu.schematics.cloud.ibm.com` (Deprecated) | Workspaces that are created with this endpoint and all associated data are stored in Europe. | Data is replicated between two locations in Europe. |
|Frankfurt|**Public**: </br> `https://eu-de.schematics.cloud.ibm.com` </br></br> **Private**: </br> `https://private-eu-de.schematics.cloud.ibm.com`| Workspaces that are created with this endpoint and all associated data are stored in Frankfurt. | Data is replicated between two locations in Europe. |
|London|**Public**: </br> `https://eu-gb.schematics.cloud.ibm.com`  </br></br> **Private**: </br> `https://private-eu-gb.schematics.cloud.ibm.com` | Workspaces that are created with this endpoint and all associated data are stored in London. | Data is replicated between two locations in Europe. |
{: caption="Location information" caption-side="bottom"}


## How is my information encrypted?
{: #pi-encrypt}

The following image shows the main {{site.data.keyword.bplong_notm}} components, how they interact with each other, and how customer-managed Key Protect, and {{site.data.keyword.hscrypto}} are used to encrypt the data? 
{: shortdesc}

![{{site.data.keyword.bplong_notm}} architecture and data encryption process](images/schematics_architecture.png){: caption="{{site.data.keyword.bplong_notm}} architecture and data encryption process" caption-side="bottom"}

1. A user sends a request to create an {{site.data.keyword.bpshort}} Workspaces to the {{site.data.keyword.bpshort}} API server.
2. The API server retrieves the Terraform template and input variables from your GitHub or GitLab source repository, or a tape archive file (`.tar`) that you uploaded from your local machine. 
3. All user-initiated actions, such as creating a workspace, generating a Terraform execution plan, or applying a plan are sent to RabbitMQ and added to the internal queue. RabbitMQ forwards requests to the {{site.data.keyword.bpshort}} engine to execute the action. 
4. The {{site.data.keyword.bpshort}} engine starts the process for provisioning, modifying, or deleting {{site.data.keyword.cloud_notm}} resources.
6. To protect customer data at rest, {{site.data.keyword.bplong_notm}} integrates with {{site.data.keyword.keymanagementserviceshort}} and {{site.data.keyword.hscrypto}}. {{site.data.keyword.bpshort}} uses the customer owned {{site.data.keyword.keymanagementserviceshort}} or {{site.data.keyword.hscrypto}} and {{site.data.keyword.bpshort}} owned {{site.data.keyword.keymanagementserviceshort}} to generate and encrypt data encryption keys (DEK). This DEK is used to encrypt transactional data, such as logs, or the Terraform `tf.state` file at rest.
7. Workspace transactional data is stored in an {{site.data.keyword.cos_full_notm}} bucket and encrypted by using [Server-Side Encryption with {{site.data.keyword.keymanagementserviceshort}}](/docs/cloud-object-storage?topic=cloud-object-storage-encryption) at rest.  
8. Workspace operational data, such as the workspace variables and Terraform template information, is stored in {{site.data.keyword.cloudant}} and encrypted at rest by using the default service encryption. For more information, see [Security](/docs/Cloudant?topic=Cloudant-security).


## How can I delete my information?
{: #delete-data}

To remove your data from {{site.data.keyword.bplong_notm}}, choose among the following options: 
- **Delete the workspace**: When you delete your workspace, all data related to a workspace is permanently deleted. 
- **Open an {{site.data.keyword.cloud_notm}} support case**: Contact IBM Support to remove your workspaces and any associated data by opening a support case. For more information, see [Getting support](/docs/get-support?topic=get-support-using-avatar). 
- **End your {{site.data.keyword.cloud_notm}} subscription**: A {{site.data.keyword.bpshort}} cleanup job runs multiple times a day to verify that all workspaces that are stored with IBM belong to an active {{site.data.keyword.cloud_notm}} account. If no active account is found, the workspace and all associated data are deleted. 
