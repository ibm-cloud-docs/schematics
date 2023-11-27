---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-27"

keywords: schematics agents, agents, terraform template to set up agents

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} Agents
{: #agent-about-intro}

{{site.data.keyword.bplong}} Agents extends {{site.data.keyword.bpshort}} ability to connect to your private Cloud infrastructure and on premise infrastructure. Deploy a dedicated agent on your private network, to enable {{site.data.keyword.bpshort}} to provision, configure, and securely access your private or on-premises resources, including converged-infrastructure, Git or Vault instances. 
{: shortdesc}

When using agents the {{site.data.keyword.bpshort}} service does not have direct access your network, to an agent or any private cloud resources. An agent uses a pull model and polls for jobs that must be run on an agent. You are in control of the agent resources, its network policies and its connection to {{site.data.keyword.bpshort}}. Agents are designed not to require inbound access from {{site.data.keyword.bpshort}} and the opening of inbound firewall or network access ports. All communication between the agent and Schematics is outbound from the agent and under your control.    
{: note}

## {{site.data.keyword.bpshort}} Agent overview
{: #about-agentb1-architecture}

Agents enable your Terraform and Ansible jobs to run on your private cloud network or in any isolated network zone and directly work with your cloud infrastructure. Agents extends the existing {{site.data.keyword.bpshort}} shared multi-tenant service, with private dedicated workers (agents) running Terraform and Ansible jobs on your private network. The diagram illustrates agent based job execution alongside the existing shared multi-tenant service. 
{: shortdesc}

![{{site.data.keyword.bpshort}} Terraform and Ansible operations with agents](images/sc-agents-architecture2.svg){: caption="{{site.data.keyword.bpshort}} Agents architecture running Terraform and Ansible" caption-side="bottom"}

Agent assignment policies dynamically route Terraform (workspace) and Ansible (actions) jobs to execute on an agent determined by the defined policies. The default, without policies defined, is for all jobs to execute on the {{site.data.keyword.bpshort}} shared infrastructure. [Assignment policies](/docs/schematics?topic=schematics-policy-manage) can be selectively defined to route jobs for workspaces and actions that meet the policy criteria to target agents and locations. Policies are defined using workspace and actions attributes, including workspace Resource Group, Schematics region and user assigned tags.    

Without policy definitions, job execution is performed via the shared multi-tenant {{site.data.keyword.bpshort}} service.  This is described more fully in the diagrams for the deployment architectures for [{{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-sc-workspaces) and [{{site.data.keyword.bpshort}} Actions](/docs/schematics?topic=schematics-sc-actions). The shared service executes jobs on the {{site.data.keyword.bpshort}} network. To access the users private network, the shared service will require bastion host access via the public internet.  

Where policies are defined, selecting workspaces and attributes by policy, jobs are executed on the target agent on your private network. These can configure, and access your cloud or on-premises services and cluster resources, managed by your own network access policies. When using agents on your private network, bastion host access via the public internet is not required. This simplifies the infrastructure requirements when using Ansible or Terraform via SSH and eliminates a number of security considerations. 

{{site.data.keyword.bpshort}} identifies and authenticates your agent using a trusted profile identity provided by the Kubernetes cluster that it runs on. This identity is dynamically created when an agent is created and deployed. The trusted profile identity guarantees that no one can spoof your agent’s identity, and steal data from {{site.data.keyword.bpshort}}. You are in control of the access permissions defined for the trusted profile identity by using IAM access policies.
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

- **Agent extends the benefits of using [{{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-learn-about-schematics) in conjunction with [{{site.data.keyword.satellitelong}}](/docs/satellite?topic=satellite-getting-started):** To provision and configure hybrid cloud resources, including private cloud resources, private data-center resources, and other public cloud resources.
- **Agent extends the benefits of {{site.data.keyword.bpshort}} to securely manage hybrid cloud infrastructure by using [Terraform](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-about), [Ansible](/docs/schematics?topic=schematics-getting-started-ansible), and other automation tools:** To perform deployment, configuration, and day-2 operations in a consistent manner – using a single pane of glass.
- **While your {{site.data.keyword.bpshort}} jobs are waiting in a shared queue for multiple tenants:** the corresponding jobs that run on the agents will not wait in any queue, and starts sooner. In other words, the workload from other tenants do not affect your performance and response time.
- **If your automation needs special software or versions, and require more capacity (CPU, memory) to run:** The multi-tenanted {{site.data.keyword.bpshort}} service will not be able to handle it. Agents can be deployed and configured to use dedicated infrastructure to run your automation that can be scaled up or down depending on the capacity needs. In addition, Agent images can be extended to include or use your own automation software and versions in conjunction with automation engine provided by the {{site.data.keyword.bpshort}} runtime.
- **The multi-tenanted {{site.data.keyword.bpshort}} service uses network access policies:** that cater to all its tenants, hence cannot be tuned to a single tenant requirements. Agents enables you to implement fine gained control over your network access policies to access your private network resources. You can configure the [ingress or egress](/docs/containers?topic=containers-vpc-kube-policies) rules and [VPC security policies](/docs/vpc?topic=vpc-security-in-your-vpc&interface=ui) that are used by Agents to connect to your hybrid cloud infrastructure.
- **Agent supports [private catalogs](/docs/account?topic=account-restrict-by-user&interface=ui):** In the {{site.data.keyword.bpshort}} Agent `1.0.0-beta2` version you can onboard a Terraform template from private repository in [IBM GitLab (public network)](https://www.ibm.com/garage/method/practices/code/tool_gitlab/?_gl=1*pyjb9z*_ga*MTYzOTIwMTM2MC4xNjk2NDExMTgy*_ga_FYECCCS21D*MTY5NjQ0Mjg1Ni42LjEuMTY5NjQ0NzE0Ni4wLjAuMA..){: external}, and deploy the Terraform template from `User private catalog`.

## Next steps
{: #nextsteps-agentb1-arch}

Now that you learned about the {{site.data.keyword.bpshort}} Agent architecture and its usage. The next steps to explore {{site.data.keyword.bpshort}} Agents:
- You can explore the steps to [prepare {{site.data.keyword.bpshort}} Agent](/docs/schematics?topic=schematics-plan-agent-overview&interface=cli) setup in your {{site.data.keyword.cloud_notm}} account.
