---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-13"

keywords: schematics blueprint, blueprint, Beta release, blueprint Beta release

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Beta-level code for {{site.data.keyword.bpshort}} Blueprints
{: #bp-beta-limitations}

The Blueprints level of code is considered Beta-level code as there will be changes in function and capabilities between now and the General Availability (GA) date.

Join our Beta program, post a question in the [{{site.data.keyword.bplong_notm}} Blueprints Beta 2022 Slack](https://ibm-cloud-schematics.slack.com/archives/C03MPHXKYRZ){: external}, and engage with the {{site.data.keyword.bpshort}} team. For any challenges, refer to the [steps to join Blueprints Beta Slack](#sc-bp-join-slack) channel.

Join the `#tmp-blueprints-beta-2022` slack channel and post a message including the following information.

- Your name
- email address
- Your Company/Organization name
- Your level of experience with cloud automation, specifically regarding Terraform, or {{site.data.keyword.bplong_notm}}
- Any initial questions or comments

You can come back any time to your created thread to add information, ask questions, or give feedback.
{: important}

## Beta release limitations 
{: #sc-bp-beta-limitation}

|  Limitation | Resolved | Date |
| --- |--- | --- | 
| Red Hat Ansible support is planned for year end 2022.  | | | 
| Blueprint operations are only supported by using the {{site.data.keyword.cloud_notm}} CLI plug-in.  | | | 
| The Terraform Plan operation is not supported for Blueprints. | | | 
| Install operations are performed as a single operation against all Workspaces.  | | | 
| Created Cloud resources are not tagged with Blueprint and Workspace IDs. | | | 
| Only one input file is supported per Blueprint definition. | | |
| Cloud resources created by a Blueprint cannot be left in place when a Blueprint is deleted. At this time all resources must be destroyed to delete a Blueprint.  | | |  
| Delete CLI returns immediately at start of execution and does not wait for successful completion. | | | 
| Operations should not be directly performed against linked Schematics Workspaces using the workspace commands or UI. Operations can only be performed using Blueprint commands.    | | |
| No external Blueprint definition validation command. | | | 
| Only Blueprints and modules in GitHub are formally supported. Testing is not complete for GitLab and other repositories. | | | 
| Only Blueprints and modules in public repositories are formally supported. Testing is not complete for private repositories. | | | 
{: caption="Beta release limitations" caption-side="bottom"}

## Beta release known issues 
{: #sc-bp-beta-knownissues}

| Issue | Resolved | Date |
| --- |--- | --- | 
| On create the Blueprint name and description specified on the command line are ignored. | | |
| On update, Blueprint name not updated. | | |  
| Install, destroy and delete command returns **`fullfilment_success`** on successful completion.  | | | 
| Automatic pull latest of updated module Terraform configuration from Git repositories are not performed on Blueprint update. | | | 
| UI not showing logs for failed workspace jobs. | | | 
| JSON output option not supported on commands. | | |   
| On CLI cursor is lost, if the cancel command is used during spinner. | | | 
| Command output date and time formatting. | | | 
{: caption="Beta release known issues" caption-side="bottom"}

## Joining public slack channel
{: #sc-bp-join-public-slack}

### Steps to join public slack
{: #sc-bp-join-slack}

Following steps allows you to join the {{site.data.keyword.bpshort}} Blueprints Beta public Slack channel.
- Click on https://cloud.ibm.com/schematics/slack
- Select **Request to join Slack** > **Request Invite**.
- A support case page will be opened
- Support Case Subject : **Request invitation to public slack channel for {{site.data.keyword.bpshort}} Blueprints Beta**.
- Support Case Description: **Invite my email address to the {{site.data.keyword.bpshort}} Blueprints Beta public Slack channel**
- Click **Next** .
- Click **Submit case**. Wait for 10 - 15 minutes, to provide an access.
