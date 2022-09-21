---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-21"

keywords: schematics blueprint, blueprint, Beta release, blueprint Beta release

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Beta code for {{site.data.keyword.bpshort}} Blueprints
{: #bp-beta-limitations}

The Blueprints level of code is considered beta code as and when the changes in function and capabilities between now, and the General Availability (GA) date.
{: shortdesc}

Join the Beta program, post a question in the [{{site.data.keyword.bplong_notm}} Blueprints Beta 2022 Slack](https://ibm-cloud-schematics.slack.com/archives/C03MPHXKYRZ){: external}, and engage with the {{site.data.keyword.bpshort}} team. For any challenges, see the [steps to join Blueprints Beta Slack](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-join-public-slack) channel.

Join the `#tmp-blueprints-beta-2022` slack channel and post a message include the following information.

- Your name
- email address
- Your Company or Organization name
- Your level of experience with cloud automation, specifically regarding Terraform, or {{site.data.keyword.bplong_notm}}
- Any initial questions or comments

You can come back anytime to your created thread to add information, ask questions, or give feedback.
{: important}

## Beta release limitations 
{: #sc-bp-beta-limitation}

|  Limitation | Resolved | Date |
| --- |--- | --- | 
| Blueprint operations are only supported by using the {{site.data.keyword.cloud_notm}} CLI plug-in.  | | | 
| The Terraform Plan operation is not supported for Blueprints. | | | 
| Install operations are set as a single operation against all Workspaces.  | | | 
| Created Cloud resources are not tagged with Blueprint and Workspace IDs. | | | 
| Only one input file is supported per Blueprint definition. | | |
| Cloud resources that are created by a Blueprint cannot be left in place when a Blueprint is deleted. Now all resources are destroyed to delete a Blueprint.  | | |  
| Delete CLI returns immediately at start of execution and does not wait for successful completion. | | | 
| Operations must not be directly set against linked {{site.data.keyword.bpshort}} Workspaces by using the Workspace commands or UI. Operations can be set by using Blueprint commands.    | | |
| No external Blueprint definition validation command. | | | 
| Only Blueprints and modules in GitHub are formally supported. Testing is not complete for GitLab and other repositories. | | | 
| Only Blueprints and modules in public repositories are formally supported. Testing is not complete for private repositories. | | | 
{: caption="Beta release limitations" caption-side="bottom"}

## Beta release known issues 
{: #sc-bp-beta-knownissues}

| Issue | Resolved | Date |
| --- |--- | --- | 
| On create the Blueprint name and description that is specified on the command line are ignored. | | |
| On update, Blueprint name is not updated. | | |  
| Install, destroy, and delete commands return **`fullfilment_success`** on successful completion.  | | | 
| Automatic `pull latest` of updated module Terraform configuration from Git repositories are not set on the Blueprint update. | | | 
| JSON output option not supported on commands. | | |   
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
