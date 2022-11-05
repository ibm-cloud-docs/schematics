---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-05"

keywords: schematics blueprint, blueprint, Beta release, blueprint Beta release

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Beta code for {{site.data.keyword.bpshort}} Blueprints
{: #bp-beta-limitations}

The {{site.data.keyword.bpshort}} Blueprints level of code is considered beta code as and when the changes in function and capabilities between now, and the General Availability (GA) date.
{: shortdesc}

Join the Beta program, post a question in the [{{site.data.keyword.bplong_notm}} Blueprints Beta 2022 Slack](https://ibm-cloud-schematics.slack.com/archives/C03MPHXKYRZ){: external}, and engage with the {{site.data.keyword.bpshort}} team. For any challenges, see the [steps to join blueprints Beta Slack](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-join-public-slack) channel.

Join the `#tmp-blueprints-beta-2022` slack channel and post a message include the following information.

- Your name
- email address
- Your Company or Organization name
- Your level of experience with cloud automation, specifically regarding Terraform, or {{site.data.keyword.bplong_notm}}
- Any initial questions or comments

You can come back anytime to your created thread to add information, ask questions, or give feedback.
{: important}

## Beta changes October 2022
Blueprint commands have been renamed with the 1.12.3 release of the Schematics CLI Plugin 
- `blueprint create` > `blueprint config create`
- `blueprint update` > `blueprint config update`
- `blueprint delete` > `blueprint config delete`
- `blueprint apply` > `blueprint run apply`
- `blueprint destroy` > `blueprint run destroy` 


## Beta release limitations 
{: #sc-bp-beta-limitation}

|  Limitation | Resolved | Date |
| --- |--- | --- | 
| Default values are not supported | Planned for November 2022 | |
| Local values are not supported | Support planned as 'hidden inputs' in November 2022 | | 
| No support for input value operators, e.g. merge | Planned for a 2023 release | |
| Parallelism of 1 during job execution | Concurrent module jobs planned for 2023 release | |    
| Blueprint operations are only supported by using the {{site.data.keyword.cloud_notm}} CLI plug-in.  | UI support planned for November 2022 | | 
| The Terraform Plan operation is not supported for blueprints. | Support planned for year end 2022 | | 
| Run operations are performed as a single operation against all modules. A future 2023 release will support single module operations  | | | 
| Created Cloud resources are not tagged with blueprint and workspace IDs. | | | 
| Only one input file is supported per blueprint configuration. | Multiple input files planned for a 2023 release | |
| Cloud resources that are created on deploy, cannot be left in place when an environment is deleted. At this time all resources must be destroyed to delete a blueprint.  | | |  
| The Delete CLI command returns immediately at start of execution and does not wait for successful completion. | | | 
| Operations must not be directly run against linked {{site.data.keyword.bpshort}} modules (workspaces) by using the workspace commands or UI. Operations must be run by using blueprint commands.    | | |
| No blueprint configuration validation command. | Support planned for November 2022 | | 
| Cost Estimation | Planned for Year End 2022 | | 
| Only blueprint templates and modules in GitHub are formally supported. Testing is not complete for GitLab and other repositories. | Support planned for November 2022 | | 
| Only blueprint templates and modules in public repositories are formally supported. Testing is not complete for private repositories. | Support planned for November 2022 | | 
{: caption="Beta release limitations" caption-side="bottom"}

## Beta release known issues 
{: #sc-bp-beta-knownissues}

| Issue | Resolved | Date |
| --- |--- | --- | 
| Run apply, run destroy, and config deletes return the generic message **`fullfilment_success`** on successful completion.  | | | 
| JSON output option not supported on commands.  | | |   
{: caption="Beta release known issues" caption-side="bottom"}

## Joining public slack channel
{: #sc-bp-join-public-slack}

### Steps to join public slack
{: #sc-bp-join-slack}

Following steps asks you to join the {{site.data.keyword.bpshort}} Blueprints Beta public Slack channel.
- Click [{{site.data.keyword.bplong_notm}} Slack](https://cloud.ibm.com/schematics/slack).
- Select **Request to join Slack** > **Request Invite**.
- A support case page is opened.
- Support Case Subject : **Request invitation to public slack channel for {{site.data.keyword.bpshort}} Blueprints Beta**.
- Support Case Description: **Invite my email address to the {{site.data.keyword.bpshort}} Blueprints Beta public Slack channel**
- Click **Next**.
- Click **Submit case**. Wait for 10 - 15 minutes to get access.
