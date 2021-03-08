---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-08"

keywords: bastion host, schematics actions, vsi using ssh, bastion host vpc

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
{:faq: data-hd-content-type='faq'}
{:external: target="_blank" .external}
{:support: data-reuse='support'}

# Configuring SSH access to access VSIs by using {{site.data.keyword.bpshort}} actions
{: #configure-ssh-access}

Bastion host is an instance that is provisioned with a public IP address and can be accessed through SSH. Once configured, the bastion host acts as a jump server to allow secure connection to instances provisioned without a public IP address.
You can perform software installation and configuration tasks on {{site.data.keyword.cloud}} VSIs for VPC, {{site.data.keyword.bpshort}} actions connect to the target VSIs by using SSH. The SSH traffic traverses over the public network from {{site.data.keyword.bplong_notm}} to the target VSIs. It is not possible to route the connection from {{site.data.keyword.bplong_notm}} to a user’s VSIs through the {{site.data.keyword.cloud_notm}} Private network.
{: shortdesc}

## Architecture
{: #architecture}

To secure the end-to-end connection between {{site.data.keyword.bplong_notm}} and the user’s private cloud network, {{site.data.keyword.bpshort}} actions connect by using SSH through a bastion host (jump server).
It is the user’s responsibility to deploy the right configured bastion host and an {{site.data.keyword.cloud_notm}} for VPC environment to create a secure connection to the target VSIs. The following diagram illustrates the {{site.data.keyword.bpshort}} actions deployment architecture.
{: shortdesc}

<img src="images/actions-bastion-host-connection.png" alt="Bastion Host Connection by using {{site.data.keyword.bpshort}} actions" width="800" style="width: 800px; border-style: none"/>

## Deploying VPC by using bastion host
{: #deploying-in-bastion-host}

To use {{site.data.keyword.bpshort}} actions, the bastion host must be deployed on the user’s private cloud network in a VPC and configure to allow Schematics access to the target VSIs. To secure and protect the connection the `VPC Security Groups` and `Network ACLs` are configured. This configuration allows  access to the bastion host and VSIs in the {{site.data.keyword.bpshort}}. All other IP traffic that are not configured receives access denied.

The user of {{site.data.keyword.bpshort}} actions is responsible for the network configuration to avoid unintended access to use VSIs.
{: note}

To assist with deploying a VPC environment on {{site.data.keyword.cloud_notm}} with bastion host access, refer to [The IBM Developer article discover best-practice VPC configuration for application deployment](https://developer.ibm.com/articles/secure-vpc-access-with-a-bastion-host-and-terraform){: external}. This article explains to configure VPC security groups and network ACLs to create a secure SSH connection that restricts public network access to the bastion host. 

## Deploying multitier VPC by using bastion host
{: #multitier-with-bastion-host}

Deploy an {{site.data.keyword.cloud_notm}} VPC with a bastion host to provide secure remote SSH access. The intended usage is for remote software installation by using Terraform remote-exec or `Redhat Ansible` executed by Schematics. For more information, about to deploy a multitier VPC with bastion host SSH access by using Terraform, refer to [Example Terraform configuration](https://github.com/Cloud-Schematics/multitier-vpc-bastion-host){: external}. 

In the {{site.data.keyword.bpshort}} actions beta release, the input values for target IP and bastion host IP to be entered. It is recommended that the Terraform configuration used to deploy the VPC environment and VSIs, includes Terraform output statements to return the IP addresses allocated by {{site.data.keyword.cloud_notm}}.
{: note}
 
