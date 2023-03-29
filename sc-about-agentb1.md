---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-29"

keywords: schematics agents, agents, terraform template to set up agents

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bplong_notm}} Agent [beta-1](/docs/schematics?topic=schematics-schematics-relnotes&interface=cli#schematics-mar2223) delivers a simplified Agents installation process.
{: attention}

{{site.data.keyword.bpshort}} Agent is a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta1-limitations) that is available for evaluation and testing purposes. It is not intended for production usage.
{: beta}

# {{site.data.keyword.bpshort}} Agent
{: #agentb1-about-intro}

Agents for the {{site.data.keyword.bplong}} extends its ability to work directly with your cloud infrastructure on your private cloud network or in any network isolation zones. The agents are deployed in the Kubernetes clusters in your private network, and is able to access your private cloud resources.
{: shortdesc}

It is important to note that {{site.data.keyword.bplong_notm}} do not have direct access to an agent or any private cloud resources.The agent always initiates the communication with {{site.data.keyword.bpshort}} service, and polls for jobs that must be run on an agent. You are in control of the agent resources, its network policies, and of underlying Kubernetes cluster.
{: note}

{{site.data.keyword.bpshort}} identifies your agent by using a trusted profile identity provided by the Kubernetes cluster that runs on. This identity is dynamically created, when an agent is created and deployed. The trusted profile identity guarantees that no one can spoof your agent’s identity, and steal data from {{site.data.keyword.bpshort}}. You are in control of the access permissions defined for the trusted profile identity by using IAM access policies.
{: note}

The agent runtime includes `Terraform`, `Ansible`, and additional micro-services. For more information about the software utilities included in the agent runtime, see [{{site.data.keyword.bpshort}} agent runtime](/docs/schematics?topic=schematics-sch-utilities).

Use the agents located on your private network to provision, configure, and access your cloud or on-premises services and cluster resources, managed by your own network access policies. Agents runs your Terraform and Ansible automation to access and work with the private network resources, and in future use software utilities of your own choice.

The following diagram illustrates how the Terraform and Ansible jobs in the agent have direct access to cloud resources through your private cloud network. Hence, your automation can directly configure the cloud resources by using SSH, without the need for Bastion hosts to gain access through the public network.  

You are in control of the network security policies of the Kubernetes cluster for running agent and therefore the ability of the Terraform and Ansible automations’ that access to your private cloud resources.

![{{site.data.keyword.bpshort}} Agents connectivity](images/new/sc-agents-network.svg){: caption="{{site.data.keyword.bpshort}} Agents connectivity" caption-side="bottom"}

## {{site.data.keyword.bpshort}} Agent architecture
{: #about-agentb1-architecture}

The diagram illustrates the {{site.data.keyword.bpshort}} Agent architecture, and the location of Terraform and Ansible job execution within a user's private cloud network.
{: shortdesc}

![{{site.data.keyword.bpshort}} Agents architecture running Terraform and Ansible](images/new/sc-agents-architecture.svg){: caption="{{site.data.keyword.bpshort}} Agents architecture running Terraform and Ansible" caption-side="bottom"}

The difference between job execution in the multi-tenant service and using Agents can be seen by comparing this diagram with the deployment architectures for [{{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-sc-workspaces) and [{{site.data.keyword.bpshort}} Actions](/docs/schematics?topic=schematics-sc-actions).

## Benefits of using Agents
{: #agentb1-usage}

The following are some of the benefits of using agents with the {{site.data.keyword.bplong_notm}}.

- **Agent extends the benefits of using [{{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-learn-about-schematics) in conjunction with [{{site.data.keyword.satellitelong}}](/docs/satellite?topic=satellite-getting-started):** To provision and configure hybrid cloud resources, including private cloud resources, private data-center resources, and other public cloud resources.
- **Agent extends the benefits of {{site.data.keyword.bpshort}} to securely manage hybrid cloud infrastructure by using [Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-about), [Ansible](/docs/schematics?topic=schematics-getting-started-ansible), and other automation tools:** To perform deployment, configuration, and day-2 operations in a consistent manner – using a single pane of glass.
- **While your {{site.data.keyword.bpshort}} jobs are waiting in a shared queue for multiple tenants:** the corresponding jobs that run on the agents will not wait in any queue, and starts sooner. In other words, the workload from other tenants do not affect your performance and response time.
- **If your automation needs special software or versions, and require more capacity (CPU, memory) to run:** The multi-tenanted {{site.data.keyword.bpshort}} service will not be able to handle it. Agents can be deployed and configured to use dedicated infrastructure to run your automation that can be scaled up or down depending on the capacity needs. In addition, Agent images can be extended to include or use your own automation software and versions in conjunction with automation engine provided by the {{site.data.keyword.bpshort}} runtime.
- **The multi-tenanted {{site.data.keyword.bpshort}} service uses network access policies:** that cater to all its tenants, hence cannot be tuned to a single tenant requirements. Agents enables you to implement a fine gained control over your network access policies to access your private network resources. You can configure the [ingress or egress](/docs/containers?topic=containers-vpc-kube-policies) rules that are used by Agents to connect to your hybrid cloud infrastructure.

## Next steps
{: #nextsteps-agentb1-arch}

Now that you learned about the {{site.data.keyword.bpshort}} Agent architecture and its usage. The next steps to explore {{site.data.keyword.bpshort}} Agents:
- You can explore the steps to [prepare {{site.data.keyword.bpshort}} Agent](/docs/schematics?topic=schematics-plan-agent-overview&interface=cli) setup in your {{site.data.keyword.cloud_notm}} account.
