---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-04"

keywords: schematics agent, agent, Beta release, agent Beta release

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Beta-level code for {{site.data.keyword.bpshort}} Agents
{: #agent-beta-limitations}

The Agent level of code is considered Beta-level code as there will be changes in function and capabilities between now and the General Availability (GA) date.

Although Agent usage has no cost involved during Beta, it will have a cost eventually in the GA timeframe. You will be updated in the documentation with the usage and the charges.

Join our Beta program, post a question in the [{{site.data.keyword.bplong_notm}} Agents Beta 2022 Slack](https://ibm-cloud-schematics.slack.com/archives/C03M925E8BH){: external}, and engage with the {{site.data.keyword.bpshort}} team. For any challenges, refer to the [steps to join Agents Beta Slack](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-join-slack) channel.

Join the `#tmp-agents-beta-2022` slack channel and post a message including the following information.

- Your name
- email address
- Your Company/Organization name
- Your level of experience with cloud automation, specifically regarding Terraform, Ansible, or {{site.data.keyword.bplong_notm}}
- Any initial questions or comments

You can come back any time to your created thread to add information, ask questions, or give feedback.
{: important}

## Beta release limitations for Agent
{: #sc-agent-beta-limitation}

There will be multiple Beta releases in short window period, this requires the users to update the Agent infrastructure and Agent services in your environment.
{: note}

|  Limitation | Resolved | Date |
| --- |--- | --- | 
| UI capabilities are not final and will be updated throughout the Beta process.| | |
| Support for drift detection is not available in Agents.| | |
| Support to [store or persist user-defined](/docs/schematics?topic=schematics-general-faq#persist-file) files is not available in Agents.| | |
| Agents supports only `Terraform v1.0` or higher Terraform version. | | |
| Agent customization is not finalized, will be communicated. | | |
| Support to monitor Agent health is limited in this release.| | |
| Supports only `one Agent in one cluster`. | | |
| Agents can be installed in a freshly provisioned Agent infrastructure, not in any other cluster.
| Update to Agent settings is not propagated to the Agent service. It requires a redeployment of Agent service using **Kubernetes Dashboard**. |  | |
{: caption="Beta release limitations" caption-side="bottom"}

