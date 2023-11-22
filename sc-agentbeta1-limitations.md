---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-22"

keywords: schematics agent, agent, beta-1 release, agent beta-1 release

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Beta-1 code for {{site.data.keyword.bpshort}} Agent
{: #agent-beta1-limitations}

The agent level of code is considered beta-1 code as there will be changes in function and capabilities between now and the General Availability (GA) date.

Although agent usage has no cost involved during beta, it will have a cost eventually in the GA timeframe. You will be updated in the documentation with the usage and the charges.

Join our beta program, post a question in the [{{site.data.keyword.bplong_notm}} users Slack](https://ibm-argonauts.slack.com/archives/CLKR4FE90){: external}, and engage with the {{site.data.keyword.bpshort}} team. If you do not have access to this Slack, [request an invitation to this Slack](https://cloud.ibm.com/schematics/slack){: external}.

Join the `#schematics-users` slack channel and post a message include the following information.

- Your name
- email address
- Your Company/Organization name
- Your level of experience with cloud automation, specifically regarding Terraform, Ansible, or {{site.data.keyword.bplong_notm}}
- Any initial questions or comments

You can come back any time to your created thread to add information, ask questions, or give feedback.
{: important}

## Beta-1 release limitations for Agent
{: #sc-agent-beta1-limitation}

There will be multiple beta releases in short window period, this requires the users to update the Agent infrastructure and Agent services in your environment.
{: note}

|  Limitation | Resolved | Date |
| --- |--- | --- | 
| The cluster and COS bucket must be in the same resource group | | | 
| Only IKS clusters are supported at this time. ROKS support is planned for the future |||
| UI capabilities are not final and will be updated throughout the beta process.| | |
| Support to [store or persist user-defined](/docs/schematics?topic=schematics-general-faq#persist-file) files is not available in Agents.| | |
| Agents supports only `Terraform v1.0` or higher Terraform version. | | |
| Agent customization is not finalized, will be communicated. | | |
| Support to monitor agent health is limited in this release.| | |
| Supports for only `one Agent per cluster`. | | |
| Agents must be installed in a fresh provisioned infrastructure, not in any other existing cluster.
| On update agent settings are not propagated to a running agent. The agent pods must be redeployed using the **Kubernetes Dashboard**. |  | |
{: caption="Beta-1 release limitations" caption-side="bottom"}

## Joining public slack channel
{: #sc-agentb1-join-public-slack}

### Steps to join public slack
{: #sc-agentb1-join-slack}

Following steps allows you to join the {{site.data.keyword.bpshort}} Agents beta public Slack channel.
- Click [{{site.data.keyword.bplong_notm}} Slack](https://cloud.ibm.com/schematics/slack).
- Select **Request to join Slack** > **Request Invite**.
- A support case page is opened.
- Support Case Subject : **Request invitation to public slack channel for {{site.data.keyword.bpshort}} Agents beta**.
- Support Case Description: **Invite my email address to the {{site.data.keyword.bpshort}} Agents beta public Slack channel**
- Click **Next**.
- Click **Submit case**. Wait for 10 - 15 minutes to get an access.

## Agent beta-1 release limitations
{: #sc-agentb1-limitation}

The following are the limitation of agent beta-1.

1. Do not support destroying an agent resources feature.
2. Agent migration is done only through {{site.data.keyword.containerlong_notm}} dashboard namespace configuration.


