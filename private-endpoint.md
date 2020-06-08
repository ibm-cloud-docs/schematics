---

copyright:
  years: 2017, 2020
lastupdated: "2020-02-07"

keywords: schematics private se, schematics private endpoint, private network schematics

subcollection: schematics

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}


# Using private endpoints
{: #private-endpoints}  

Create and manage {{site.data.keyword.bplong_notm}} workspaces on the private network by targeting the {{site.data.keyword.bpshort}} private service endpoint.
{: shortdesc} 

To get started, enable [virtual routing and forwarding (VRF) and service endpoints](/docs/account?topic=account-vrf-service-endpoint){: external} for your {{site.data.keyword.cloud_notm}} account. After you enable VRF for your account, you can connect to {{site.data.keyword.bplong_notm}} by using a private IP that is accessible only through the {{site.data.keyword.cloud_notm}} private network. To learn more about private connections on {{site.data.keyword.cloud_notm}}, see [Service endpoints for private connections](/docs/resources?topic=resources-service-endpoints){:external}.

To connect to {{site.data.keyword.bplong_notm}} by using a private network connection, you must use the {{site.data.keyword.bpshort}} API or the CLI plug-in. This capability is not available from the {{site.data.keyword.cloud_notm}} console.
{: note}

## Overview of private service endpoints in {{site.data.keyword.bpshort}}
{: #private-cse}

The following private service endpoints are available for {{site.data.keyword.bpshort}}. 
{: shortdesc}

|Location|Private service endpoint|Description|
|-------|--------------|---------------------|
|US|`https://private-us.schematics.cloud.ibm.com`|Workspaces that are created with this endpoint and all associated data are stored in the US. |
|Europe|`https://private-eu.schematics.cloud.ibm.com`|Workspaces that are created with this endpoint and all associated data are stored in Europe. |

## Step 1: Enable VRF and service endpoints for your account
{: #private-network-prereqs}

Enable your {{site.data.keyword.cloud_notm}} account to work with private service endpoints. 
{: shortdesc}

1. Enable your {{site.data.keyword.cloud_notm}} account for [virtual routing and forwarding (VRF)](/docs/account?topic=account-vrf-service-endpoint#vrf){: external}.

   When you enable VRF, a separate routing table is created for your account, and connections to and from your account's resources are routed separately on the {{site.data.keyword.cloud_notm}} network. To learn more about VRF technology, see [Virtual routing and forwarding on {{site.data.keyword.cloud_notm}}](/docs/account?topic=account-vrf-service-endpoint){: external}.

   Enabling VRF permanently alters networking for your account. Be sure that you understand the impact to your account and resources. After you enable VRF, you cannot disable VRF again.
   {: important}
2. Enable your {{site.data.keyword.cloud_notm}} account for [service endpoints](/docs/account?topic=account-vrf-service-endpoint#service-endpoint){: external}.

   After you enable VRF and service endpoints for your account, all existing and future {{site.data.keyword.bpshort}} workspaces become available from both the public and private service endpoints.
    {: note}
    
3. Verify that your account is enabled for VRF and service endpoints. 
   1. Log in to {{site.data.keyword.cloud_notm}}.
      ```
      ibmcloud login
      ```
      {: pre}
      
      If the login fails, run the `ibmcloud login --sso` command to try again. The `--sso` parameter is required when you log in with a federated ID. If this option is used, go to the link listed in the CLI output to generate a one-time passcode.
      {: tip}
      
   2. Show the details of your account. 
      ``` 
      ibmcloud account show
      ```
      {: pre}
      
      Example output: 
      ```
      Retrieving account User's Account of user@email.com...
      OK

      Account ID:                   a111aaaa1aa1aaaaaaaaaaaa1a1aa111   
      Currently Targeted Account:   true   
      Linked Softlayer Account:     1001234
      VRF Enabled:                  true  
      Service Endpoint Enabled:     true
      ```
      {: screen}
    
## Step 2: Connect to the {{site.data.keyword.bpshort}} private service endpoint
{: #configure-private-network}

Prepare your VSI or test machine by configuring your routing table for the {{site.data.keyword.cloud_notm}} private network.

1. To connect to the private service endpoint, you must create a virtual server instance (VSI) first. You use this VSI to connect to the {{site.data.keyword.cloud_notm}} private network. You can create a [classic VSI](/docs/vsi?topic=virtual-servers-getting-started-tutorial) or [VPC VSI](/docs/vpc?topic=vpc-getting-started). 

2. Follow the documentation for your VSI to establish a secure connection to your VSI. 

3. After you are connected to the VSI, target the private service endpoint when you send API requests to the 
{{site.data.keyword.bpshort}} API server. The following examples shows the supported Terraform and Helm versions of the {{site.data.keyword.bpshort}} engine. 
   ```
   curl -X GET https://private-us.schematics.cloud.ibm.com/v1/version
   ```
   {: pre}
   


