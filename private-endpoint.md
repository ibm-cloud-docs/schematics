---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-02"

keywords: schematics private se, schematics private endpoint, private network schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Using private endpoints
{: #private-endpoints}  

Create and manage {{site.data.keyword.bplong_notm}} Workspaces on the private network by targeting the {{site.data.keyword.bpshort}} private service endpoint.
{: shortdesc} 

To get started, enable [virtual routing and forwarding (VRF) and service endpoints](/docs/account?topic=account-vrf-service-endpoint){: external} for your {{site.data.keyword.cloud}} account. After you enable VRF for your account, you can connect to {{site.data.keyword.bplong_notm}} by using a private IP that is accessible only through the {{site.data.keyword.cloud_notm}} Private network. To learn more about private connections on {{site.data.keyword.cloud_notm}}, see [Service endpoints for private connections](/docs/schematics?topic=schematics-secure-data#pi-location).

To connect to {{site.data.keyword.bplong_notm}} by using a [private network connection](/docs/schematics?topic=schematics-secure-data#pi-location), you must use the {{site.data.keyword.bpshort}} API or the command-line plug-in. This capability is not available from the {{site.data.keyword.cloud_notm}} console.
{: note}

## Private service endpoints in {{site.data.keyword.bpshort}}
{: #private-cse}

The private service endpoints are available for {{site.data.keyword.bpshort}}. {{site.data.keyword.bplong_notm}} CLI users can access their private network by specifying `private-us-south.schematics.cloud.ibm.com` as the API endpoint of {{site.data.keyword.bplong_notm}} CLI. For more information, see [Using private {{site.data.keyword.bpshort}} endpoints](/docs/schematics?topic=schematics-secure-data#pi-location).
{: shortdesc}

To access the private network, you need to first login to private network by using [`ibmcloud login -a private.cloud.ibm.com`](
https://github.com/IBM-Cloud/ibm-cloud-cli-sdk/blob/master/docs/plugin_developer_guide.md#9-private-endpoint-support). Access {{site.data.keyword.bpshort}} commands to interact with the private {{site.data.keyword.bpshort}} endpoint to automatically access the endpoint.
{: important}

### Enable VRF and service endpoints for your account
{: #private-network-prereqs}

Enable your {{site.data.keyword.cloud_notm}} account to work with private service endpoints. 
{: shortdesc}

1. Enable your {{site.data.keyword.cloud_notm}} account for [virtual routing and forwarding (VRF)](/docs/account?topic=account-vrf-service-endpoint#vrf){: external}.

    When you enable VRF, a separate routing table is created for your account, and connections to and from your account's resources are routed separately on the {{site.data.keyword.cloud_notm}} network. To learn more about VRF technology, see [Virtual routing and forwarding on {{site.data.keyword.cloud_notm}}](/docs/account?topic=account-vrf-service-endpoint){: external}.

    Enabling VRF permanently alters networking for your account. Be sure that you understand the impact to your account and resources. After you enable VRF, you cannot disable VRF again.
    {: important}

2. Enable your {{site.data.keyword.cloud_notm}} account for [service endpoints](/docs/account?topic=account-vrf-service-endpoint#service-endpoint){: external}.

    After you enable VRF and service endpoints for your account, all existing and future {{site.data.keyword.bpshort}} Workspaces become available from both the public and private service endpoints.
    {: note}

3. Verify that your account is enabled for VRF and service endpoints.
    1. Log in to {{site.data.keyword.cloud_notm}}.
        ```sh
        ibmcloud login
        ```
        {: pre}

        If the login fails, run the `ibmcloud login --sso` command to try again. The `--sso` parameter is required when you log in with a federated ID. If this option is used, go to the link listed in the command-line output to generate a one-time passcode.
        {: tip}

    2. Show the details of your account.
        ```sh
        ibmcloud account show
        ```
        {: pre}

        Example output:
        ```text
        Retrieving account User's Account of user@email.com...
        OK

        Account ID:                   a111aaaa1aa1aaaaaaaaaaaa1a1aa111   
        Currently Targeted Account:   true   
        Linked Softlayer Account:     000000
        VRF Enabled:                  true  
        Service Endpoint Enabled:     true
        ```
        {: screen}

### Connect to the {{site.data.keyword.bpshort}} private service endpoint
{: #configure-private-network}

Prepare your VSI or test machine by configuring your routing table for the {{site.data.keyword.cloud_notm}} Private network.

1. To connect to the private service endpoint, you must create a virtual server instance (VSI) first. You use this VSI to connect to the {{site.data.keyword.cloud_notm}} Private network. You can create a [classic VSI](/docs/virtual-servers?topic=virtual-servers-getting-started-tutorial) or [VPC VSI](/docs/vpc?topic=vpc-getting-started).

2. After you are connected to the VSI, target the private service endpoint when you send API requests to the {{site.data.keyword.bpshort}} API server. The following example shows the supported Terraform and Helm versions of the {{site.data.keyword.bpshort}} engine.
    ```sh
    curl -X GET https://private-us-south.schematics.cloud.ibm.com/v1/version
    ```
    {: pre}


## Virtual Private Endpoints Gateways for {{site.data.keyword.bpshort}}
{: #endpoint-setup}

A service instance can have a private network endpoint, a public network endpoint, or both. After your account is enabled for VPC and you connect {{site.data.keyword.bpshort}} service on the private network from Virtual Private Endpoint Gateways.
{: shortdesc}

    - **Public:** A service endpoint on the {{site.data.keyword.cloud_notm}} public network.
    - **Private:** A service endpoint that is accessible only on the {{site.data.keyword.cloud_notm}} private network with no access from the public internet.
    - **Both public and private:** Service endpoints that allow access over both networks.

Virtual Private Endpoint Gateways is only supported for the VPC Generation 2.
{: note}

### Before you begin
{: #endpoint-prereq}

Before you begin, to access the  {{site.data.keyword.bpshort}} service through the Virtual Private Endpoint Gateways, ensure that you meet the following criteria:

* Make sure that you have the required [permissions](/docs/schematics?topic=schematics-access#access-setup) to [create VPC](/docs/vpc?topic=vpc-getting-started), to create an endpoint gateway, to create or bind a reserved IP from the subnet, and [account limits for VSI creation](/docs/vpc?topic=vpc-quotas#vpcquotas) for concurrent instances.
* A VPC Generation 2 instance and a subnet zones to bind an IP address at the same time you provision the endpoint gateway. For more information, see [Getting started with VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
* A VSI is created. For more information, see [creating a VSI](/docs/vpc?topic=vpc-creating-virtual-servers&interface=cli).

### Adding Virtual Private Endpoint Gateways for {{site.data.keyword.bpshort}}
{: #endpoint-add}

Now, you can securely connect the Virtual Private Endpoint Gateways to access {{site.data.keyword.bpshort}} services and functions such as `workspace`, `action`, `job`, `plan`, `apply`, and `destroy` for a new instance. For more information, see [Overview of private service endpoints in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-secure-data#pi-location).
{: shortdesc}

You cannot create multiple Virtual Private Endpoint Gateways for the same {{site.data.keyword.bpshort}} instance.
{: important}

The steps to add the private network endpoints for {{site.data.keyword.bpshort}}:

1. Create a {{site.data.keyword.bpshort}} Workspace. For more information, see [creating a workspace](/docs/schematics?topic=schematics-workspace-setup#create-workspace).
2. Optionally, you can deploy a resource instance into {{site.data.keyword.bpshort}} Workspace. For more information, see [deploying your resource](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources).
3. Create a Virtual Private Endpoint Gateways. For more information, see [creating an endpoint gateway](/docs/vpc?topic=vpc-ordering-endpoint-gateway#vpe-creating-ui). And you can assign the listed {{site.data.keyword.bpshort}} services endpoint into Virtual Private Endpoint Gateways.
4. View the created Virtual Private Endpoint Gateways associated with the {{site.data.keyword.bpshort}} services. For more information, see [Viewing details of an endpoint gateway](/docs/vpc?topic=vpc-vpe-viewing-details-of-an-endpoint-gateway). 
