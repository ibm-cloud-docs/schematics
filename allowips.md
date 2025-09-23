---

copyright:
  years: 2017, 2025
lastupdated: "2025-09-23"

keywords: schematics locations, schematics regions, schematics zones, schematics endpoints, schematics service endpoints

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Firewall access - allowed IP addresses
{: #allowed-ipaddresses}

Access to {{site.data.keyword.bpshort}} using IAM allowed IP addresses has been replaced with [context based restrictions](/docs/schematics?topic=schematics-access-control-cbr).
{: note}

Performing post-configuration of deployed resources using workspace and action jobs requires IP network access to the resources private cloud network zones. Typically these private networks are protected using a firewall or VPC access control policies. To allow the {{site.data.keyword.bpshort}} hosted instances of Terraform and Ansible to access these zones, firewall or VPC access policies must be configured to permit access to the {{site.data.keyword.bpshort}} originating IP addresses.

Typically post-configuration is performed through SSH as illustrated with [{{site.data.keyword.bpshort}} actions](/docs/schematics?topic=schematics-sc-actions) performing configuration operations over SSH using Ansible. With Ansible a bastion host must be configured to enable secure SSH access. Refer to the [{{site.data.keyword.bpshort}} actions](/docs/schematics?topic=schematics-sc-actions#sc-actions-overview) documentation for details of the required VPC network configuration and bastion host setup.

## {{site.data.keyword.bpshort}} IP addresses
{: #ipaddresses}

The following tables document the public IP addresses used by {{site.data.keyword.bpshort}} that must be allowed access to private network resources to perform post-configuration.

At run time {{site.data.keyword.bpshort}} dynamically selects a worker node and region to execute the job. The job may run on any of the defined IP addresses within a geography. For instance in the `US` using any of the `us-south` and `us-east` IP addresses, or for `Europe` using any of the `eu-gb` or `eu-de` addresses.
{: note}

| Region | Zone | Public IP addresses | Private IP addresses |
| --- | --- | --- | --- |
| `EU Central` | `fra02`, `fra04`,`fra05` | `149.81.123.64/27`,`149.81.135.64/28`,</br>`158.177.210.176/28`,`158.177.216.144/28`,</br>`161.156.138.80/28`,`159.122.111.224/27`,</br>`161.156.37.160/27`,</br>`149.81.15.201`,`158.176.1.26`,`149.81.159.90` | `10.123.76.192/26`,`10.194.127.64/26`,</br>`10.75.204.128/26`,`10.16.207.5`,</br>`10.223.39.136`,`10.22.120.190` |
| `UK South` | `lon04`,`lon05`,`lon06` | `158.175.106.64/27`,`158.175.138.176/28`,</br>`141.125.79.160/28`,`141.125.142.96/27`,</br>`158.176.111.64/27`,`158.176.134.80/28`,</br> `158.176.179.166`,`141.125.162.185`,`158.175.188.210`| `10.45.215.128/26`,`10.196.59.0/26`,</br>`10.72.173.0/26`,`10.223.13.27`,</br>`10.223.19.119`,`10.223.20.189` |
| `US` | `wdc04`,`wdc06`,`wdc07` and </br>`dal10`,`dal12`,`dal13`| `169.45.235.176/28`,`169.55.82.128/27`,`169.60.115.32/27`,</br>`169.63.150.144/28`,`169.62.1.224/28`,`169.62.53.64/27`,</br>`52.117.126.44`,`169.59.190.179`,`169.63.103.233` and `150.238.230.128/27`,`169.63.254.64/28`,`169.47.104.160/28`,</br>`169.61.191.64/27`,`169.60.172.144/28`,`169.62.204.32/27`</br>`67.18.90.188`,`52.116.193.125`,`52.118.145.125` | `10.148.98.0/26`,`10.189.2.128/26`,</br>`10.190.16.128/26`,`10.191.181.64/26`,</br>`10.22.47.223`,`10.12.110.204`,`10.12.112.17` and </br>`10.95.173.64/26`,`10.185.16.64/26`,</br>`10.220.38.64/26`,`10.249.64.170`,</br>`10.22.221.100`,`10.119.53.93` |
| `Toronto` | `ca-tor-1`, `ca-tor-2`, `ca-tor-3` | `163.74.95.217`, `163.66.91.30`, `163.75.88.96` | `192.168.16.0/22`,`192.168.36.0/22`,`192.168.8.0/22`,`192.168.32.0/22`, </br> `192.168.20.0/22`,`192.168.24.0/22`,`192.168.0.0/22`,`192.168.4.0/22`</br> `192.168.40.0/22`, `10.223.153.6`, `10.223.168.102`, `10.223.183.79`|
| `Montreal` | `ca-mon-1`, `ca-mon-2`, `ca-mon-3` | `64.5.41.76`,  `64.5.47.225`, `64.5.49.94` | `10.46.73.205`, `10.46.77.205`, `10.46.81.205`|
{: caption="Region and supported public and private IPs" caption-side="bottom"}

You can collapse down the ranges into security group rules. For example, `us-south` and `us-east` as two security group rules like `[169.45.235.176/28, 150.238.230.128/27]`. For more information about creating security group rules, see [IBM security group rules](/docs/security-groups?topic=security-groups-security-groups-guidelines#rules-1).
{: note}
