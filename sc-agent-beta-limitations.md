---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-21"

keywords: schematics agent, agent, beta release, agent beta release

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Beta code for {{site.data.keyword.bpshort}} Agents
{: #agent-beta-limitations}

The Agent level of code is considered beta code as there will be changes in function and capabilities between now and the General Availability (GA) date.

Although Agent usage has no cost involved during beta, it will have a cost eventually in the GA timeframe. You will be updated in the documentation with the usage and the charges.



Join our beta program, post a question in the [{{site.data.keyword.bplong_notm}} users Slack](https://ibm-argonauts.slack.com/archives/CLKR4FE90){: external}, and engage with the {{site.data.keyword.bpshort}} team.

Join the `#schematics-users` slack channel and post a message include the following information.

- Your name
- email address
- Your Company/Organization name
- Your level of experience with cloud automation, specifically regarding Terraform, Ansible, or {{site.data.keyword.bplong_notm}}
- Any initial questions or comments

You can come back any time to your created thread to add information, ask questions, or give feedback.
{: important}

## Beta release limitations for Agent
{: #sc-agent-beta-limitation}

There will be multiple beta releases in short window period, this requires the users to update the Agent infrastructure and Agent services in your environment.
{: note}

|  Limitation | Resolved | Date |
| --- |--- | --- | 
| UI capabilities are not final and will be updated throughout the beta process.| | |
| Support for drift detection is not available in Agents.| | |
| Support to [store or persist user-defined](/docs/schematics?topic=schematics-general-faq#persist-file) files is not available in Agents.| | |
| Agents supports only `Terraform v1.0` or higher Terraform version. | | |
| Agent customization is not finalized, will be communicated. | | |
| Support to monitor Agent health is limited in this release.| | |
| Supports only `one Agent in one cluster`. | | |
| Agents can be installed in a freshly provisioned Agent infrastructure, not in any other cluster.
| Update to Agent settings is not propagated to the Agent service. It requires a redeployment of Agent service using **Kubernetes Dashboard**. |  | |
{: caption="Beta release limitations" caption-side="bottom"}

## Joining public slack channel
{: #sc-agent-join-public-slack}

### Steps to join public slack
{: #sc-agent-join-slack}

Following steps allows you to join the {{site.data.keyword.bpshort}} Agents beta public Slack channel.
- Click [{{site.data.keyword.bplong_notm}} Slack](https://cloud.ibm.com/schematics/slack).
- Select **Request to join Slack** > **Request Invite**.
- A support case page is opened.
- Support Case Subject : **Request invitation to public slack channel for {{site.data.keyword.bpshort}} Agents beta**.
- Support Case Description: **Invite my email address to the {{site.data.keyword.bpshort}} Agents beta public Slack channel**
- Click **Next**.
- Click **Submit case**. Wait for 10 - 15 minutes to get an access.
