---

copyright:
  years: 2017, 2024
lastupdated: "2024-01-04"

keywords: schematics agent, agent, release, agent release

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# {{site.data.keyword.bpshort}} Agent limitation
{: #agent-limitation}


Currently the {{site.data.keyword.bpshort}} Agent usage has no cost involved, it will have a cost eventually in the General Availability (GA) timeframe. You will be updated in the documentation with the usage and the charges.

Join our GA program, post a question in the [{{site.data.keyword.bplong_notm}} users Slack](https://ibm-argonauts.slack.com/archives/CLKR4FE90){: external}, and engage with the {{site.data.keyword.bpshort}} team. If you do not have access to this Slack, [request an invitation to this Slack](https://cloud.ibm.com/schematics/slack){: external}.

Join the `#schematics-users` slack channel and post a message include the following information.

- Your name
- email address
- Your Company/Organization name
- Your level of experience with cloud automation, specifically regarding Terraform, Ansible, or {{site.data.keyword.bplong_notm}}
- Any initial questions or comments

You can come back any time to your created thread to add information, ask questions, or give feedback.
{: important}

## Limitations of an Agent
{: #sc-agent-beta1-limitation}

There will be multiple beta releases in short window period, this requires the users to update the Agent infrastructure and Agent services in your environment.
{: note}

|  Limitation | Resolved | Date |
| --- |--- | --- | 
| The cluster and COS bucket must be in the same resource group | | |
| Only IKS clusters are supported at this time. ROKS support is planned for the future |||
| UI capabilities are not final and will be updated throughout the beta process.| | |
| Support to [store or persist user-defined](/docs/schematics?topic=schematics-general-faq#persist-file) files is not available in agents.| | |
| Agents supports only `Terraform v1.0` or higher Terraform version. | | |
| Agent customization is not finalized, will be communicated. | | |
| Support to monitor agent health is limited in this release.| | |
| Supports for only `one Agent per cluster`. | | |
| Agents must be installed in a fresh provisioned infrastructure, not in any other existing cluster.
| On update agent settings are not propagated to a running agent. The agent pods must be redeployed using the **Kubernetes Dashboard**. |  | |
{: caption="Limitations" caption-side="bottom"}

## Joining public slack channel
{: #sc-agentb1-join-public-slack}

### Steps to join public slack
{: #sc-agentb1-join-slack}

Following steps allows you to join the {{site.data.keyword.bpshort}} agents beta public Slack channel.
- Click [{{site.data.keyword.bplong_notm}} Slack](https://cloud.ibm.com/schematics/slack).
- Select **Request to join Slack** > **Request Invite**.
- A support case page is opened.
- Support Case Subject : **Request invitation to public slack channel for {{site.data.keyword.bpshort}} agents beta**.
- Support Case Description: **Invite my email address to the {{site.data.keyword.bpshort}} agents beta public Slack channel**
- Click **Next**.
- Click **Submit case**. Wait for 10 - 15 minutes to get an access.

## Agent release limitations
{: #sc-agentb1-limitation}

The following are the planned future enhancement of an agent.

1. Deploying an agent on OpenShift Cluster.
