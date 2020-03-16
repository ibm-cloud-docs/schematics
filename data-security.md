 

## How is my information encrypted?
The following image shows the main {{site.data.keyword.bplong_notm}} components, how they interact with each other, and what type of encryption is applied to your personal information. 
{: shortdesc}

<img src="images/schematics_architecture.png" alt="{{site.data.keyword.bplong_notm}} architecture and data encryption process" width="900" style="width: 900px; border-style: none"/>

1. A user sends a request to create an {{site.data.keyword.bplong_notm}} workspace to the {{site.data.keyword.bpshort}} API server.
2. The API server retrieves the Terraform template and input variables from your GitHub or GitLab source repository. 
3. All user-initiated actions, such as creating a workspace, generating a Terraform execution plan, or applying a plan are sent to RabbitMQ and added to the internal queue. RabbitMQ forwards requests to the {{site.data.keyword.bpshort}} engine to execute the action. 
4. The {{site.data.keyword.bpshort}} engine starts the process for provisioning, modifying, or deleting {{site.data.keyword.cloud_notm}} resources. 
5. To protect customer data in transit, {{site.data.keyword.bplong_notm}} integrates with {{site.data.keyword.keymanagementserviceshort}}. {{site.data.keyword.bpshort}} uses root keys in {{site.data.keyword.keymanagementserviceshort}} to create data encryption keys (DEK). The DEK is then used to encrypt workspace transactional data, such as logs, or the Terraform `tf.state` file in transit. 
6. Workspace transactional data is stored in an {{site.data.keyword.cos_full_notm}} bucket and encrypted by using [Server-Side Encryption with {{site.data.keyword.keymanagementserviceshort}}](/docs/services/cloud-object-storage?topic=cloud-object-storage-encryption#encryption-kp) at rest.  
7. Workspace operational data, such as the workspace variables and Terraform template information, is stored in {{site.data.keyword.cloudant}} and encrypted at rest by using the default service encryption. For more information, see [Security](/docs/services/Cloudant?topic=cloudant-security).

## How can I delete my personal information?
{: #delete-data}

To remove your personal information from {{site.data.keyword.bplong_notm}}, choose among the following options: 
- **Delete the workspace**: When you delete your workspace, all personal information related to a workspace is permanently deleted. 
- **Open an {{site.data.keyword.cloud_notm}} support case**: Contact IBM Support to remove your workspaces and personal information by opening a support case. For more information, see [Getting support](/docs/get-support?topic=get-support-getting-customer-support). 
- **End your {{site.data.keyword.cloud_notm}} subscription**: A {{site.data.keyword.bpshort}} cleanup job runs multiple times a day to verify that all workspaces that are stored with IBM belong to an active {{site.data.keyword.cloud_notm}} account. If no active account is found, the workspace and all personal information are deleted. 


</staging>
