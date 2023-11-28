---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-28"

keywords: schematics agents, agents, terraform template to set up agents

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} Agents
{: #agent-about-intro}

{{site.data.keyword.bplong}} Agents extends {{site.data.keyword.bpshort}} ability to connect to and configure your private Cloud infrastructure and on premise infrastructure. Deploy a dedicated agent on your private network, to enable {{site.data.keyword.bpshort}} to provision, configure, and securely access private or on-premises resources, including converged-infrastructure, Git or Vault instances. 
{: shortdesc}

When using agents the {{site.data.keyword.bpshort}} service does not have direct access your network, to an agent or your private cloud resources. An agent uses a pull model and polls for jobs that will be run on an agent. You are in control of the agent resources, its network policies and its connection to {{site.data.keyword.bpshort}} and the jobs what will be executed on the agent. Agents are designed not to require inbound access from {{site.data.keyword.bpshort}} and the opening of inbound firewall or network access ports. All communication between the agent and Schematics is outbound from the agent and under your control.    
{: note}

## {{site.data.keyword.bpshort}} Agent overview
{: #about-agentb1-architecture}

Agents enable your Terraform and Ansible jobs to execute on your private cloud network or in any isolated network zone and directly work with your cloud infrastructure. Agents extends the existing {{site.data.keyword.bpshort}} shared multi-tenant service, with private dedicated workers (agents) running Terraform and Ansible jobs on your private network. The diagram illustrates agent based job execution alongside the existing shared multi-tenant service. 
{: shortdesc}

![{{site.data.keyword.bpshort}} Terraform and Ansible operations with agents](images/sc-agents-architecture2.svg){: caption="{{site.data.keyword.bpshort}} Agents architecture running Terraform and Ansible" caption-side="bottom"}

Agent (assignment) policies dynamically route Terraform (workspace) and Ansible (actions) jobs to execute on an agent determined by user specified policy attributes. The default, without policies defined, is for all jobs to execute on the {{site.data.keyword.bpshort}} shared infrastructure. When defined, [Assignment policies](/docs/schematics?topic=schematics-policy-manage) route workspace and action jobs to agents deployed in specific private cloud or network locations. Policy selection attributes include workspace and action Resource Groups, region and user assigned tags.    

Without policy definitions, job execution is performed via the shared multi-tenant {{site.data.keyword.bpshort}} service. This is described more fully in the diagrams for the deployment architectures for [{{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-sc-workspaces) and [{{site.data.keyword.bpshort}} Actions](/docs/schematics?topic=schematics-sc-actions). The shared service executes jobs on the {{site.data.keyword.bpshort}} network. When using the shared service, jobs that require access to the users private network, including SSH commands or Ansible, require access to the private network via the public internet using a user configured bastion host.  

With agents, policies determine the workspaces and actions that desire to run on agents deployed on your private network. Under your control, jobs running on these agents can configure, and access your cloud or on-premises services and cluster resources, managed by your own network access policies. When using agents on your private network, bastion host access via the public internet is not required. The use of agents simplifies the infrastructure and security requirements when using Ansible or Terraform via SSH and eliminates a number of security considerations. 

{{site.data.keyword.bpshort}} identifies and authenticates your agent using an {{site.data.keyword.cloud_notm}} Trusted Profile identity provided by the Kubernetes cluster running the agent. This identity is dynamically created when an agent is created and deployed. The trusted profile identity guarantees that no one can spoof your agent’s identity, and steal data from {{site.data.keyword.bpshort}} or your account. You are in control of the access permissions defined for the trusted profile identity by using IAM access policies.
{: note}

The agent runtime includes `Terraform`, `Ansible`, and additional micro-services. For more information about the software utilities included in the agent runtime, see [{{site.data.keyword.bpshort}} agent runtime](/docs/schematics?topic=schematics-sch-utilities).

## Private network configuration when using agents
{: #about-agentb1-networking}

The following diagram illustrates a possible agent deployment model on a cluster in an environment with multiple VPCs connected via a transit gateway. Here an agent, running Terraform and Ansible jobs, has direct access to cloud resources over the private cloud network. In this deployment model, your Terraform or Ansible automations' can directly configure your cloud resources by using SSH, without the need for bastion hosts to gain access via the public network.  

![{{site.data.keyword.bpshort}} Agents connectivity](images/sc-agents-network.svg){: caption="{{site.data.keyword.bpshort}} Agents connectivity" caption-side="bottom"}

With agents you are in control of the network security policies of the Kubernetes cluster and any VPC Security Group or Access Control List policies for the running agent and therefore the ability of the Terraform and Ansible automations’ to access to your private cloud resources.

## Benefits of using Agents
{: #agentb1-usage}

{{site.data.keyword.bplong}} is a multi-tenant service supporting concurrent usage by a large number of users. The multi-tenant model imposes a number of restrictions to maintain fair usage across all users and maintain network isolation between users.  

The following are some of the benefits of using agents with the {{site.data.keyword.bplong}} service:

- **Agent extends the benefits of using [{{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-learn-about-schematics) in conjunction with [{{site.data.keyword.satellitelong}}](/docs/satellite?topic=satellite-getting-started):** to provision and configure hybrid cloud resources, including private cloud resources, private data-center resources, and other public cloud resources.
- **Agent extends the benefits of {{site.data.keyword.bpshort}} to securely manage hybrid cloud infrastructure by using [Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-about), [Ansible](/docs/schematics?topic=schematics-getting-started-ansible), and other automation tools:** to perform deployment, configuration, and day-2 operations in a consistent manner – using a single pane of glass.
- **While your {{site.data.keyword.bpshort}} jobs are waiting in a shared queue for multiple tenants:** the corresponding jobs that run on the agents will not wait in any queue, and start sooner. In other words, other tenants workloads do not affect your job performance and response time.
- **If your automation needs special software or versions, and require more capacity (CPU, memory) to run:** The multi-tenanted {{site.data.keyword.bpshort}} service may not address your requirements. Agents can be deployed and configured to use dedicated infrastructure to run your automation that can be scaled up or down depending on the capacity needs. In a future release, agent images can be extended to include or use your own automation software and versions in conjunction with automation engine provided by the {{site.data.keyword.bpshort}} runtime.
- **The multi-tenanted {{site.data.keyword.bpshort}} service uses network access policies:** that are common all its tenants. Agents enable you to implement fine gained control over the network access policies when performing workspace or actions jobs to access your private network resources. You can configure the [ingress or egress](/docs/containers?topic=containers-vpc-kube-policies) rules and [VPC security policies](/docs/vpc?topic=vpc-security-in-your-vpc&interface=ui) that are used by agents executing your jobs to connect to your hybrid cloud infrastructure.
- **Agents relieves several of the restrictions of running in a shared service:** that are necessary to ensure fair usage of the shared service. {{site.data.keyword.bpshort}} agents relax the timeout limitation for local-exec, remote-exec and Ansible playbook execution. These are limited to 60 minutes in the multi-tenant service to ensure fair service utilisation by all users. No duration is applied for jobs executed on agents. 

## Next steps
{: #nextsteps-agentb1-arch}

On this page you have learned a little about {{site.data.keyword.bpshort}} Agents and its usage. The next steps to explore {{site.data.keyword.bpshort}} Agents:
- Explore the steps to [prepare {{site.data.keyword.bpshort}} Agent](/docs/schematics?topic=schematics-plan-agent-overview&interface=cli) setup in your {{site.data.keyword.cloud_notm}} account.
