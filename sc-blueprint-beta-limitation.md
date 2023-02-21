---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-21"

keywords: schematics blueprint, blueprint, beta release, blueprint beta release

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Beta code for {{site.data.keyword.bpshort}} Blueprints
{: #bp-beta-limitations}

The {{site.data.keyword.bpshort}} Blueprints level of code is considered beta code as and when the changes in function and capabilities between now, and the General Availability (GA) date.
{: shortdesc}



Join the beta program, post a question in the [{{site.data.keyword.bplong_notm}} users Slack](https://ibm-argonauts.slack.com/archives/CLKR4FE90){: external}, and engage with the {{site.data.keyword.bpshort}} team.

Join the `#schematics-users` slack channel and post a message include the following information.

- Your name
- email address
- Your Company or Organization name
- Your level of experience with cloud automation, specifically regarding Terraform, or {{site.data.keyword.bplong_notm}}
- Any initial questions or comments

You can come back anytime to your created thread to add information, ask questions, or give feedback.
{: important}

## Beta changes October 2022
{: #bp-beta-changes-oct}

Blueprint commands have been renamed with the 1.12.3 release of the {{site.data.keyword.bpshort}} CLI Plugin 
- `blueprint create` > `blueprint create`
- `blueprint update` > `blueprint update`
- `blueprint delete` > `blueprint delete`
- `blueprint apply` > `blueprint apply`
- `blueprint destroy` > `blueprint destroy` 


## Beta release limitations 
{: #sc-bp-beta-limitation}

|  Limitation | Resolved | Date |
| --- |--- | --- | 
| Default values | Planned for December 2022 | Delivered Dec 2022 |
| Large input value sizes up to Terraform limit of 4MB | Planned for December 2022 | |
| Local values | Local support planned for 2023 | |
| Input value operators, for example: merge, concat | Planned for a 2023 release | |
| Performance optimization - Parallel job execution for plan and apply | Concurrent module jobs planned for 2023 release | | 
| Performance optimization - Plan and Apply only run against changed modules | Planned for 2023 release | | 
| Blueprint operations are only supported by using the {{site.data.keyword.cloud_notm}} CLI plug-in. | UI support planned for December 2022 | Delivered Dec 2022 | 
| The Terraform Plan (all) operation for blueprint template. | Support planned for December 2022 | | 
| Run operations are performed as a single operation against all modules (plan + apply).|  A future 2023 release will support single module operations  | | 
| Created Cloud resources tagged with blueprint, catalog and workspace IDs. | Planned for December 2022  | Delivered Dec 2022 | 
| Only one input file is supported per blueprint configuration. | Multiple input files planned for a 2023 release | |
| Cloud resources that are created on deploy, cannot be left in place when an environment is deleted. At this time all resources must be destroyed to delete a blueprint. | | |  
| The Delete CLI command returns immediately at start of execution and does not wait for successful completion. | | | 
| Operations must not be directly run against linked {{site.data.keyword.bpshort}} modules (workspaces) by using the workspace commands or UI. Operations must be run by using blueprint commands. | | |
| Blueprint configuration validation command. | Support planned for November 2022 | VSCode language extension delivered in November 2022  | 
| Cost Estimation in CLI and UI | Planned for 2023 release | | 
| Only blueprint templates and modules in GitHub are formally supported. Testing is not complete for GitLab and other repositories. | Planned  November  2022 | Delivered November 2022 | 
| Only blueprint templates and modules in public repositories are formally supported. Testing is not complete for private repositories using Git tokens. | Planned in November 2022 | Delivered November 2022 | 
| Templates and modules in IBM private Gitlab using IAM token support | Planned in December 2022 |  | 
{: caption="Beta release limitations" caption-side="bottom"}

## Beta release known issues 
{: #sc-bp-beta-knownissues}

| Issue | Resolved | Date |
| --- |--- | --- | 
| apply, destroy, and deletes return the generic message **`fullfilment_success`** on successful completion. | | | 
| JSON output option not supported on commands. | | |   
{: caption="Beta release known issues" caption-side="bottom"}

## Joining public slack channel
{: #sc-bp-join-public-slack}

### Steps to join public slack
{: #sc-bp-join-slack}

Following steps asks you to join the {{site.data.keyword.bpshort}} Blueprints beta public Slack channel.
- Click [{{site.data.keyword.bplong_notm}} Slack](https://cloud.ibm.com/schematics/slack).
- Select **Request to join Slack** > **Request Invite**.
- A support case page is opened.
- Support Case Subject : **Request invitation to public slack channel for {{site.data.keyword.bpshort}} Blueprints beta**.
- Support Case Description: **Invite my email address to the {{site.data.keyword.bpshort}} Blueprints beta public Slack channel**
- Click **Next**.
- Click **Submit case**. Wait for 10 - 15 minutes to get access.
