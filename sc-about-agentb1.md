---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-25"

keywords: schematics agents, agents, terraform template to set up agents

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} Agent
{: #agentb1-about-intro}

{{site.data.keyword.bpshort}} Agents is a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

{{site.data.keyword.bplong}} Agents extends {{site.data.keyword.bpshort}} ability to work directly with your cloud infrastructure on your private cloud network. Use Agents located on your private network to provision, configure and access your cloud or on-premises services and cluster resources, managed by your own network access policies. Eliminate execution time limits and enable Terraform and Ansible to access and work with private network resources, and in future use software utilities of your own choosing.
{: shortdesc}

The agent runtime includes `Terraform`, `Terraform CLI v1.0.11`, `Terraform CLI v1.1.5`, `Ansible`, and additional micro-services. For more information about the software utilities included in the agent runtime, see [{{site.data.keyword.bpshort}} runtime deployment utilities](/docs/schematics?topic=schematics-sch-utilities).

## Benefits of using Agents
{: #agentb1-usage}

The following are some of the benefits of using {{site.data.keyword.bplong_notm}} Agents compared to using the default multi-tenant service offering:

- Use [{{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-learn-about-schematics) and [{{site.data.keyword.satellitelong}}](/docs/satellite?topic=satellite-getting-started) to deploy and configure hybrid cloud resources, including private cloud resources, private data center resources, and other public cloud resources.
- Use {{site.data.keyword.bpshort}} to securely connect to and manage hybrid cloud infrastructure by using [Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-about), [Ansible](/docs/schematics?topic=schematics-getting-started-ansible), and other automation tools to perform deployments, configuration, and day-2 operations.
- Reduce the time waiting for {{site.data.keyword.bpshort}} job execution to start by using private job queues to run your automation.
- Use dedicated infrastructure to run your automation and scale up or scale down the capacity depending on your automation workloads.
- Fine gained control over network access policies to access private network resources, configuring [ingress](/docs/containers?topic=containers-vpc-kube-policies) or egress rules that are used by Agents to connect to your hybrid cloud infrastructure.
- Use your own software, and versions in conjunction with automation engine provided by the {{site.data.keyword.bpshort}} runtime.

## {{site.data.keyword.bpshort}} Agent architecture
{: #about-agentb1-architecture}

The diagram illustrates the {{site.data.keyword.bpshort}} Agent architecture, and the location of Terraform and Ansible job execution within a users private cloud network.
{: shortdesc}

![{{site.data.keyword.bpshort}} Agents architecture running Terraform and Ansible](images/new/sc-agents-architecture.svg){: caption="{{site.data.keyword.bpshort}} Agents architecture running Terraform and Ansible" caption-side="bottom"}

The difference between job execution in the multi-tenant service and using Agents can seen by comparing this diagram with the deployment architectures for [{{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-sc-workspaces) and [{{site.data.keyword.bpshort}} Actions](/docs/schematics?topic=schematics-sc-actions).

![{{site.data.keyword.bpshort}} Agents networking](images/new/sc-agents-network.svg){: caption="{{site.data.keyword.bpshort}} Agents networking" caption-side="bottom"}

In this figure, when using Agents, Terraform, and Ansible jobs run on the users private cloud network. They have direct access to a users cloud resources via the users private cloud network. For direct configuration of cloud resources using Ansible or Terraform using SSH, access is via the private network which eliminates the need for Bastion Hosts and the routing of access via the public network. Terraform and Ansible access to private resources is under direct user control via user defined network security policies.   

## Next steps
{: #nextsteps-agentb1-arch}

Now that you learned about the {{site.data.keyword.bpshort}} Agent architecture and its usage. The next steps to explore {{site.data.keyword.bpshort}} Agents:
- You can explore the steps to [prepare {{site.data.keyword.bpshort}} Agent](/docs/schematics?topic=schematics-plan-agent-overview&interface=cli) setup in your {{site.data.keyword.cloud_notm}} account.

