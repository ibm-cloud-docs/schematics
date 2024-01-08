---

copyright:
  years: 2017, 2024
lastupdated: "2024-01-08"

keywords: schematics agents, agents, terraform template to set up agents

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} agents
{: #agent-about-intro}

{{site.data.keyword.bplong}} agents extend {{site.data.keyword.bpshort}} ability to use workspace and action jobs to provision and configure your private Cloud infrastructure and on-premises infrastructure. Dedicated agents on your private network, enable {{site.data.keyword.bpshort}} to provision, configure, and securely access private or on-premises resources. Also dedicated agents include [converged-infrastructure](https://en.wikipedia.org/wiki/Converged_infrastructure){: external}, hypervisors, private Git repositories, configuration, and {{site.data.keyword.secrets-manager_short}} services.
{: shortdesc}

{{site.data.keyword.bpshort}} does not have direct access to your network, to an agent or your private cloud resources. An agent uses a pull model and polls for the workspace or actions jobs that run on an agent. You are in control of the agent resources, its network policies, its connection to {{site.data.keyword.bpshort}} and the jobs that are ran on the agent. Agents are designed not to require inbound access from {{site.data.keyword.bpshort}} or the opening of inbound firewall or network access ports. All communication between the agent and {{site.data.keyword.bpshort}} is outbound from the agent and under your control.
{: note}

## {{site.data.keyword.bpshort}} Agent overview
{: #about-agentb1-architecture}

Agents enable your {{site.data.keyword.bpshort}} workspace and actions jobs to run on your private cloud network or in any isolated network zone and directly work with your cloud infrastructure. Agents extend the existing {{site.data.keyword.bpshort}} shared multi-tenant service, with private dedicated workers (agents) running workspace and action jobs on your private network. The diagram illustrates agent-based job execution alongside the existing shared multi-tenant service. 
{: shortdesc}

![{{site.data.keyword.bpshort}} workspace and action operations with agents](images/sc-agents-architecture2.svg){: caption="{{site.data.keyword.bpshort}} agents architecture running workspaces and actions" caption-side="bottom"}

Agent (assignment) policies dynamically route workspace and action jobs to run on an agent determined by user specified policy attributes. The default, without policies that are defined, are for all workspace and actions jobs to run on the {{site.data.keyword.bpshort}} shared infrastructure. When defined, [assignment policies](/docs/schematics?topic=schematics-policy-manage) route workspace and action jobs to agents deployed in specific private cloud or network locations. Policy selection attributes include workspace and action Resource Groups, region, and user assigned tags.    

Without policy definitions, job execution is installed through the shared multi-tenant {{site.data.keyword.bpshort}} service. The shared environment is described for the deployment architectures in [{{site.data.keyword.bpshort}} workspaces](/docs/schematics?topic=schematics-sc-workspaces) and [{{site.data.keyword.bpshort}} actions](/docs/schematics?topic=schematics-sc-actions). The shared service runs jobs on the {{site.data.keyword.bpshort}} network to connect the {{site.data.keyword.cloud_notm}} APIs over the public or private network as needed. If you use the shared service, jobs that require access to resources on the private network, with the running SSH commands or Ansible, access to the users private network. It is done through the public internet by using a user-configured bastion host.  

With agents, policies determine the workspaces and actions that you want to run on agents deployed on your private network. Under your control, jobs that run on these agents can configure, and access your cloud or on-premises services and cluster resources, managed by your own network access policies. If you use agents on your private network, bastion host access through the public internet is not needed. The use of agents simplifies the infrastructure and security requirements if you use Ansible or Terraform through SSH and eliminates various security considerations.

{{site.data.keyword.bpshort}} identifies and authenticates your agent by using an {{site.data.keyword.cloud_notm}} trusted profile identity that is provided by the Kubernetes cluster that runs the agent. This identity is dynamically created when an agent is created and deployed. The trusted profile identities confirm that no one can spoof your agent’s identity, and steal data from {{site.data.keyword.bpshort}} or your account. You are in control of the access permissions that are defined for the trusted profile identity by using IAM access policies.
{: note}

The agent runtime includes `Terraform`, `Ansible`, and more micro-services. For more information about the software utilities included in the agent runtime, see [{{site.data.keyword.bpshort}} agent runtime](/docs/schematics?topic=schematics-sch-utilities).

## Private network configuration when using agents
{: #about-agentb1-networking}

The following diagram illustrates a possible agent deployment on a cluster environment with multiple VPCs connected through a transit gateway. Here an agent, running workspace and action jobs, has a direct connection to cloud resources over the private cloud network. In this deployment model, your job can directly configure your cloud resources by using SSH, without the need for bastion hosts to gain access through the public network.  

![{{site.data.keyword.bpshort}} agents connectivity](images/sc-agents-network.svg){: caption="{{site.data.keyword.bpshort}} agents connectivity" caption-side="bottom"}

With agents you are in control of the network security policies of the Kubernetes cluster, and any VPC Security Group or Access Control List policies for the running agent. Also the ability of the workspace and actions jobs to have access to your private cloud resources.

## Benefits of using agents
{: #agentb1-usage}

{{site.data.keyword.bplong}} is a multi-tenant service that supports concurrent usage by many users. The multi-tenant model imposes some restrictions to maintain fair usage across all users and maintain network isolation between users.  

The following are some of the benefits of using agents with {{site.data.keyword.bplong}}:

- **Agents extend the benefits of using the [{{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-learn-about-schematics) along with [{{site.data.keyword.satellitelong}}](/docs/satellite?topic=satellite-getting-started):** to provision and configure hybrid cloud resources across multiple cloud providers.
- **Agents give users their own execution queues.** With the multi-tenant shared {{site.data.keyword.bpshort}} service, jobs are placed in a shared queue for all users. Jobs that run on the agents waits in the shared queue and starts sooner. In other words, other tenants workloads do not affect your job performance and response time.
- **If your automation needs special software or versions, or requires more capacity (CPU, memory) to run:** The multi-tenant {{site.data.keyword.bpshort}} service does not address your requirements. Agents can be deployed and configured to use dedicated infrastructure to run your automation that can be scaled up or down depending on the capacity needs. In a future release, agent images can be extended to include or use your own automation software and versions along with automation engine that is provided by the {{site.data.keyword.bpshort}} runtime.
- **The multi-tenanted {{site.data.keyword.bpshort}} service uses network access policies:** that's common to all users. Agents enable you to implement fine-gained control over the network access policies when performing workspace or actions jobs that access your private network resources. You can configure the [ingress or egress](/docs/containers?topic=containers-vpc-kube-policies) rules of the agent cluster and [VPC security policies](/docs/vpc?topic=vpc-security-in-your-vpc&interface=ui) that are used by agents executing your jobs to connect to your private cloud infrastructure.
- **Agents relieves several of the restrictions of running in a shared service:** that is necessary to ensure fair usage of the shared service. {{site.data.keyword.bpshort}} agents relax the timeout limitation for local-exec, remote-exec, and Ansible playbook execution. The timeout limitations in the multi-tenant service are removed to ensure fair service utilisation by all users. No duration is applied for jobs ran on agents.

## Next steps
{: #nextsteps-agentb1-arch}

You learned a about {{site.data.keyword.bpshort}} Agents and its usage. Now, you can explore the {{site.data.keyword.bpshort}} agents:
- Explore the steps to [prepare {{site.data.keyword.bpshort}} Agent](/docs/schematics?topic=schematics-plan-agent-overview&interface=cli) setup in your {{site.data.keyword.cloud_notm}} account.
