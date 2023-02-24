---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-24"

keywords: schematics locations, schematics regions, schematics zones, schematics endpoints, schematics service endpoints

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Opening needed IP addresses for {{site.data.keyword.bpfull_notm}} in your firewall
{: #allowed-ipaddresses}

By default all IP addresses can be used to log in to the {{site.data.keyword.cloud}} console and access your {{site.data.keyword.bpshort}} Workspace. In the {{site.data.keyword.iamlong}} (IAM) console, you can generate a firewall [by creating an allowlist by specifying which IP addresses have access](/docs/account?topic=account-ips), and all other IP addresses are restricted. If you use an IAM firewall, you must add the CIDRs of the {{site.data.keyword.bpshort}} for the zones in the region where your cluster is located to the allowlist. You must allow these CIDRs ranges, so that {{site.data.keyword.bpshort}} Service to manage the {{site.data.keyword.bpshort}} resources.
{: shortdesc}

You can use these steps to change the IAM allowlist for the user whose credentials are used for the cluster's region and resource group infrastructure permissions. If you have the credentials, you can change your own IAM allowlist settings. If you do not have credentials, but you are assigned the `Editor` or `Administrator` of the {{site.data.keyword.cloud_notm}} IAM platform role for the user management service. Then, you can update the restricted IP addresses for the credentials owner.

1. Log in to the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/login){: external}.
2. From the menu bar click `Manage > Access (IAM)`, and select **Users**.
3. Select your username.
4. From the **User details** page, access IP address restrictions section.
5. For Classic infrastructure, enter the CIDRs of the zones in the region where your cluster is located.

You must allow all the zones within the region that your resource is in. For example, you must allow all `US` region IP addresses to support both the `us-east` and `us-south` region endpoints.
{: note}

| Region | Zone | Public IP addresses | Private IP addresses |
| --- | --- | --- | --- |
| `EU Central` | `fra02`, `fra04`,`fra05` | `149.81.123.64/27`,`149.81.135.64/28`,</br>`158.177.210.176/28`,`158.177.216.144/28`,</br>`161.156.138.80/28`,`159.122.111.224/27`,</br>`161.156.37.160/27`| `10.123.76.192/26`,`10.194.127.64/26`,`10.75.204.128/26` |
| `UK South` | `lon04`,`lon05`,`lon06` | `158.175.106.64/27`,`158.175.138.176/28`,</br>`141.125.79.160/28`,`141.125.142.96/27`,</br>`158.176.111.64/27`,`158.176.134.80/28` | `10.45.215.128/26`,</br>`10.196.59.0/26`,`10.72.173.0/26` |
| `US` | `wdc04`,`wdc06`,`wdc07` and `dal10`,`dal12`,`dal13`| `169.45.235.176/28`,`169.55.82.128/27`,</br>`169.60.115.32/27`,`169.63.150.144/28`,`169.62.1.224/28`,`169.62.53.64/27` and `150.238.230.128/27`,`169.63.254.64/28`,</br>`169.47.104.160/28`,`169.61.191.64/27`,</br>`169.60.172.144/28`,`169.62.204.32/27` | `10.148.98.0/26`,`10.189.2.128/26`,</br>`10.190.16.128/26`,`10.191.181.64/26`,</br>`10.95.173.64/26`,`10.185.16.64/26`,</br>`10.220.38.64/26` |
{: caption="Region and supported public and private IPs" caption-side="bottom"}

You can collapse down the ranges into security group rules. For example, `us-south` and `us-east` as two security group rules like `[169.45.235.176/28, 150.238.230.128/27]`. For more information about creating security group rules, see [IBM security group rules](/docs/security-groups?topic=security-groups-security-groups-guidelines#rules-1).
{: note}



