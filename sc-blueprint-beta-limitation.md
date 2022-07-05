---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-05"

keywords: schematics blueprint, blueprint, Beta release, blueprint Beta release

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Beta-level code for {{site.data.keyword.bpshort}} Blueprints
{: #bp-beta-limitations}

The Blueprints level of code is considered Beta-level code as there will be changes in function and capabilities between now and the General Availability (GA) date.

Join our Beta program, post a question in the [{{site.data.keyword.bplong_notm}} Blueprints Beta 2022 Slack](https://ibm-cloud-schematics.slack.com/archives/C03MPHXKYRZ){: external}, and engage with the {{site.data.keyword.bpshort}} team.

Join the `#tmp-blueprints-beta-2022` slack channel and post a message including the following information to join this Beta program.

- Your name
- email address
- Your Company/Organization name
- Your level of experience with cloud automation, specifically with regard to Terraform, or {{site.data.keyword.bplong_notm}}
- Any initial questions or comments

Then at any time come back to the thread you've just created, reply to it to ask more questions or give feedback.
{: important}

## Beta release limitations 
{: #sc-bp-beta-limitation}


|  Limitation | Resolved | Date |
| --- |--- | --- | 
| Red Hat Ansible support is planned for year end 2022.  | | | 
| Blueprint operations are only supported by using the {{site.data.keyword.cloud_notm}} CLI plug-in.  | | | 

| The Terraform Plan operation is not supported for Blueprints | | | 
| Install operations are performed as a single operation against all Workspaces.  | | | 
| Created Cloud resources are not tagged with Blueprint and Workspace IDs. | | | 
| Only one input file is supported per Blueprint definition. | | |
| Cloud resources created by a Blueprint cannot be left in place when a Blueprint is deleted. At this time all resources must be destroyed to delete a Blueprint.  | | |  
| Delete CLI returns immediately at start of execution and does not wait for successful completion | | | 
| Operations should not be directly performed against linked Schematics Workspaces using the workspace commands or UI. Operations should only be performed using Blueprint commands.    | | |
| No external Blueprint definition validation command | | | 



## Beta release known issues 

| Issue | Resolved | Date |
| --- |--- | --- | 
| On create the Blueprint name and description specified on the command line are ignored. | | |
| On update, Blueprint name not updated | | |  
| Install, destroy and delete commands return "fullfilment_success" on successful completion.  | | | 
| Automatic pull latest of updated Terraform configs from Git repos is not performed on Blueprint Update | | | 
| UI not showing log for failed Workspace jobs | | | 
| Rerun of failed install job does not execute | | | 
| Update command requires inputs to be specified to trigger refresh of Blueprint definition from Git | | |   