---

copyright:
  years: 2017, 2020
lastupdated: "2020-12-17"

keywords: schematics cli reference, schematics commands, schematics cli, schematics reference, cli

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


# Using commands and utilities in {{site.data.keyword.bpshort}} jobs
{: #schematics-utilities}

Ansible job is built on `Red Hat Universal Base Images` (UBI-8), and the runtimes that are available with UBI-8 are:
- Python3
- Python3-pip
- OpenShift client
- Ansible 2.9.7
- Terraform v11 and Terraform v12
- kubectl
- {{site.data.keyword.cloud_notm}} CLI
- jq

The Python modules that are applied in Ansible job are:
- netaddr
- kubernetes
- openshift
- ibm-cloud-sdk-core
- ibm-vpc
{: shortdesc}