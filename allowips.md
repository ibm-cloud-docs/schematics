 ---

copyright:
  years: 2017, 2020
lastupdated: "2020-07-15"

keywords: terraform template guidelines, terraform config file guidelines, sample terraform files, terraform provider, terraform variables, terraform input variables, terraform template

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

# Opening required IP addresses for {{site.data.keyword.bpfull_notm}} in your firewall
{: #allowed-ipaddresses}

If you set up a custom firewall and want to access and manage {{site.ibm.keyword.cloud_notm}} services with {{site.ibm.keyword.bpshort}}, you need to open up the following IP addresses and CIDR ranges in your firewall.
{: shortdesc}

| Region | Zone | Public IP addresses | Private IP addresses |
| ------------ | ------------ | ---------- | -------- |
| EU Central | fra02 </br> fra04 </br> fra05 | `158.177.218.64/28` </br> `161.156.139.192/28` </br> `149.81.103.128/28` | `10.134.126.192/26` </br> `10.75.127.192/26` </br> `10.123.141.128/26` |
| UK South | lon04 </br> lon04 </br> lon05 </br> lon06 | `158.175.115.176/28` </br> `158.175.138.48/28` </br> `141.125.75.80/28` </br> `158.176.121.128/28` | `10.45.115.64/26` </br> `10.45.229.0/26` </br> `10.72.195.192/26` </br> `10.196.55.64/26` |
| US East | wdc04 </br> wdc06 </br> wdc07 | `169.55.115.16/28` </br> `169.63.133.32/28` </br> `169.62.60.224/28` | `10.148.29.64/26` </br> `10.190.109.0/26` </br> `10.188.197.128/26` |
| US South | dal10 </br> dal12 </br> dal13 | `169.46.20.240/28` </br> `169.48.193.160/28` </br> `52.117.217.128/28` | `10.177.217.64/26` </br> `10.185.151.192/26` </br> `10.220.47.192/26` |

