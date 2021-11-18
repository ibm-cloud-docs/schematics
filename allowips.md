---

copyright:
  years: 2017, 2021
lastupdated: "2021-11-16"

keywords: schematics locations, schematics regions, schematics zones, schematics endpoints, schematics service endpoints

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Opening required IP addresses for {{site.data.keyword.bpfull_notm}} in your firewall
{: #allowed-ipaddresses}

By default, all IP addresses can be used to log in to the {{site.data.keyword.cloud}} console and access your Schematics workspace. In the {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) console, you can generate a firewall [by creating an allowlist by specifying which IP addresses have access](/docs/account?topic=account-ips), and all other IP addresses are restricted. If you use an IAM firewall, you must add the CIDRs of the {{site.data.keyword.bplong_notm}} for the zones in the region where your cluster is located to the allowlist. You must allow these CIDRs ranges, so that {{site.data.keyword.bplong_notm}} Service to manage the {{site.data.keyword.bplong_notm}} resources.
{: shortdesc}

You can use these steps to change the IAM allowlist for the user whose credentials are used for the cluster's region and resource group infrastructure permissions. If you are the credentials owner, you can change your own IAM allowlist settings. If you are not the credentials owner, but you are assigned the Editor or Administrator IBM Cloud IAM platform role for the User Management service, you can update the restricted IP addresses for the credentials owner.

1. Log in to the [IBM Cloud console](https://cloud.ibm.com/login){: external}.
2. From the menu bar, click `Manage > Access (IAM)`, and select Users.
3. Select your user name.
4. From the User details page, go to the IP address restrictions section.
5. For Classic infrastructure, enter the CIDRs of the zones in the region where your cluster is located.

You must allow all the zones within the region that your resource is in.
{: note}

| Region | Zone | Public IP addresses | Private IP addresses |
| ------------ | ------------ | ---------- | -------- |
| EU Central | fra02 </br> fra04 </br> fra05 | `158.177.210.176/28` </br> `158.177.216.144/28` </br> `161.156.138.80/28` </br> `149.81.135.64/28` | `10.134.233.192/26` </br> `10.123.76.192/26` </br> `10.194.127.64/26` </br> `10.75.204.128/26` |
| UK South | lon04 </br> lon04 </br> lon05 </br> lon06 | `158.175.90.16/28` </br> `158.175.138.176/28` </br> `141.125.79.160/28` </br> `158.176.134.80/28` | `10.45.190.64/26` </br> `10.45.215.128/26` </br> `10.72.173.0/26` </br> `10.196.59.0/26` |
| US East | wdc04 </br> wdc06 </br> wdc07 | `169.45.235.176/28` </br> `169.61.99.176/28` </br> `169.62.1.224/28` <br> `169.63.150.144/28` </br> `169.63.173.208/28` |
| US South | dal10 </br> dal12 </br> dal13 | `169.47.104.160/28` </br> `169.60.172.144/28` </br> `169.63.254.64/28` |

You can collapse down the ranges into security group rules. For example, `US-South` and `US-East` as two security group rules like `[169.44.0.0/44, 169.60.0.0/14]`. For more information, about creating security group rules, refer to [IBM security group rules](/docs/security-groups?topic=security-groups-security-groups-guidelines#rules-1).
{: note}


