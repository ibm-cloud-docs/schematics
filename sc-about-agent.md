---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-10"

keywords: schematics agents, agents, terraform template to set up agents

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} Agents
{: #agents-intro}

{{site.data.keyword.bpshort}} Agents is a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the Beta release.
{: beta}

The {{site.data.keyword.bplong}} Agents extends {{site.data.keyword.bpshort}} ability to reach your cloud infrastructure. Integrate the {{site.data.keyword.bpshort}} Agents running in your private network to the {{site.data.keyword.bplong_notm}} service to provision, configure, and operate your private or on-premises cloud cluster resources without any time, network, or software restrictions. The {{site.data.keyword.bpshort}} Agents runtime uses `Terraform`, `Terraform CLI v1.0.11`, `Terraform CLI v1.1.5`, and `Microservices`. For more information about the Agents utilities, see [{{site.data.keyword.bpshort}} runtime development tools](/docs/schematics?topic=schematics-sch-utilities).
{: shortdesc}

## Usage of an Agent
{: #agent-usage}

The following are the primary drivers to create the {{site.data.keyword.bplong_notm}} Agents.

- Use [{{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-learn-about-schematics) and [{{site.data.keyword.satellitelong}}](/docs/satellite?topic=satellite-getting-started) to deploy and configure hybrid cloud resources such as private cloud resources, private data center resources, and other public cloud resources.
- Use {{site.data.keyword.bpshort}} to securely connect and manage hybrid cloud infrastructure by using [Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-about), [Ansible](/docs/schematics?topic=schematics-getting-started-ansible), and other automation tools to perform the deployment, configurations, and the day-2 operations.
- Use to reduce your wait time in a shared {{site.data.keyword.bpshort}} queue to run your automation.
- Use a dedicated infrastructure to run your automation and the ability to scale up or scale down the capacity depending on your automation workloads.
- Use to fine tune the network policies such as [ingress](/docs/containers?topic=containers-vpc-kube-policies) or egress rules that are used by {{site.data.keyword.bpshort}} to connect to hybrid cloud infrastructure.
- Use your software, and versions in conjunction with automation engine provided by the {{site.data.keyword.bpshort}} runtime.

## {{site.data.keyword.bpshort}} Agent architecture
{: #agents-architecture}

The diagram represents the {{site.data.keyword.bpshort}} Agent architecture, and how it functions in {{site.data.keyword.bpshort}}.

![{{site.data.keyword.bpshort}} Agent Architecture](images/sc_agents_architecture.svg){: caption="{{site.data.keyword.bpshort}} Agent architecture and its components" caption-side="bottom"}

1. As the {{site.data.keyword.bpshort}} Agents user, now you can extend the {{site.data.keyword.bpshort}} ability to reach your cloud infrastructure from your {{site.data.keyword.cloud_notm}} account. 
2. Configure the {{site.data.keyword.bpshort}} Agent by using an [Agents infrastructure workspace](/docs/schematics?topic=schematics-glossary#agentsa3) and an [Agents service workspace](/docs/schematics?topic=schematics-glossary#agentsa2) to create your cluster infrastructure.
3. Integrate the {{site.data.keyword.bpshort}} private endpoint with the {{site.data.keyword.bpshort}} service to provision, configure, and monitor your application through {{site.data.keyword.bpshort}} Agent.
4. Manage the tools and softwares through Agents services containing the microservices.

## Augmenting {{site.data.keyword.bpshort}} with {{site.data.keyword.bpshort}} Agents
{: #agents-augmenting}

The table describes how the {{site.data.keyword.bpshort}} are augmented with {{site.data.keyword.bpshort}} Agents with the components such as, cluster, cloud providers, compute time, latency, software, tenancy, and network configurations.

| Components | {{site.data.keyword.bpshort}} without Agent | {{site.data.keyword.bpshort}} with Agent|
| -- | -- | -- |
| `Cluster` | Runs in {{site.data.keyword.bpshort}} cluster. | Runs in customer's cluster. |
| `Cloud providers` | Works primarily with {{site.data.keyword.cloud_notm}} and not tested with other cloud services. | Can integrate with any cloud service providers or private cloud. |
| `Compute time` | Null resources or [local-exec](/docs/schematics?topic=schematics-schematics-limitations#local-remote-exec) executes for a maximum of 30 minutes. | There is no compute time restrictions. |
| `Latency` | Runs in {{site.data.keyword.bpshort}} clusters provisioned in `us` or `eu` region only. | Can be configured to run on edge cluster or {{site.data.keyword.satelliteshort}} cluster for faster response time. |
| `Network configuration` | `Ingress/egress` policies are controlled by {{site.data.keyword.bpshort}}. Cannot reach out to any external PORT that are not in allowed list.| Can decide on `ingress/egress` policies, and can allow PORTS accordingly.|
| `Software` | Can use only pre-installed software such as `Python / Jquery / {{site.data.keyword.cloud_notm}} command line` cannot install additional software. | Customer is free to install additional software on need basis. |
| `Tenancy` | Multi tenant. | Single tenant. |
{: caption="Usage of {{site.data.keyword.bpshort}} Agents" caption-side="bottom"}

## Setting up an Agent
{: #setting-agent}

The [{{site.data.keyword.bpshort}} Agents](/docs/schematics?topic=schematics-agents-intro) are deployed in your {{site.data.keyword.cloud}} account and configured to connect to your {{site.data.keyword.bpshort}} service instance. The block diagram represents the set up to provision, deploy, connect, and use the required cluster infrastructure.
{: shortdesc}

![{{site.data.keyword.bpshort}} Agents set up](For more information aimages/agents-setup-latest.svgbout "{{site.data.keyword.bpshort}} Agents set up"){: caption="{{site.data.keyword.bpshort}} Agents set up" caption-side="center"}

For more information about estimated time to set up an Agent, refer to [Installing {{site.data.keyword.bpshort}} Agent](/docs/schematics?topic=schematics-agents-setup).

## Next steps
{: #nextsteps-agent-arch}

Now that you learned about the {{site.data.keyword.bpshort}} Agents architecture and its advantages. The next steps to explore {{site.data.keyword.bpshort}} Agents:
- You can explore the steps for [Installing the {{site.data.keyword.bpshort}} Agent](/docs/schematics?topic=schematics-agents-setup) in your {{site.data.keyword.cloud_notm}} account.
