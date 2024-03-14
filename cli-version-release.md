---

copyright:
  years: 2017, 2024
lastupdated: "2024-03-14"

keywords: schematics command-line reference, schematics commands, schematics command-line, schematics reference, command-line, change log, command-line releases

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# CLI version history 
{: #cli_version-releases}

Find a summary of changes for each version of {{site.data.keyword.bpshort}} CLI plug-in. Be sure to keep your CLI up-to-date so that you can use all the available commands and their options.
{: shortdesc}

| Version | Release date | Changes |
| ----- | ------- | -------------- |
| 1.12.18 | 08 March 2024 | Display the Terraform deprecation warning message during workspace commands using less than `terraform_v1.5`, Support for agent infrastructure update is removed, and fixed `index out of range` error by using `ibmcloud schematics state list` command.|
| 1.12.17 | 14 February 2024 | {{site.data.keyword.bpshort}} plug-in installation supports Cloud Shell, and [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-workspace-upload) command now supports the Cloud Shell commands.|
| 1.12.16 | 7 February 2024 | [`ibmcloud schematics workspace list`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=api#schematics-workspace-list) supports caching for API versions. `terraform_v1.2`, `terraform_v1.3`, `terraform_v1.4` depreciation message are populated for creating the [`ibmcloud schematics workspace new`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=api#schematics-workspace-new) templates.|
| 1.12.15 | 24 January 2024 | Support for `refresh_token` in [agent update API](/docs/schematics?topic=schematics-update-agent-overview&interface=api#update-agent-api) request, enhanced the version support for [agent update](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-update) command.|
| 1.12.14 | 10 January 2024 | Added new commands and translations to support the agent and policy. The system workspaces from the workspace list command output are hidden. Enhanced the agent job display on command output. Usage of `/v1/versions` API for the agent versions.|
| 1.12.12 | 17 September 2023 | {{site.data.keyword.bpshort}} Agent create and update added with a [`new flag --metadata`](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-create) and a bug fix to configure an [HTTP timeout for request](/docs/schematics?topic=schematics-general-faq&interface=cli#http-api-call). |
| 1.12.10 | 22 May 2023 | {{site.data.keyword.bpshort}} [Agent update](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-update) and [`agent list`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-list) command bug fixes to set the runtime errors. |
| 1.12.9 | 6 April 2023 | {{site.data.keyword.bpshort}} Agent beta-1 and policy CLI commands are enhanced to include the -`-target-file`, and the `output` of [agent plan](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-plan), [agent apply](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-apply), and [agent health](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-health).  |
| 1.12.8 | 22 Mar 2023 | [{{site.data.keyword.bpshort}} Agent beta-1](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#agents-cmd) and [policy](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#policy-cmd) CLI commands are available in `us-south`, `us-east`, `eu-de`, `eu-gb` region.  |
| 1.12.7 | 07 Feb 2023 | Bug fix to disable `API_AGENT_ATTACHMENT` in `us-south`, `us-east`, `eu-de`, `eu-gb` region.  |
| 1.12.6 | 30 Jan 2023 | Enhanced complex input support through `yaml` file. Fixes related to the status output, index out of range for workspace action output, refresh token issue for long running and spinner panic fixes. |
| 1.12.5 | 18 Dec 2022 | The subcommand usage, and support for specifying complex inputs through a local YAML file by using `-input-file` option.  
| 1.12.3 | 18 Nov 2022 |  Fixed subcommand usage support `source type`. |
| 1.12.3 | 3 Nov 2022 |  Enhanced CLI commands, with the latest SDK update, and the workspace action command update. |
| 1.12.2 | 11 Aug 2022 | Included `--output` flag and bug fixes for the commands in and released {{site.data.keyword.bpshort}} v1.12.2 plug-in.|
| 1.12.1 | 26 July 2022 | Incorporated the bugs and fixes commands in {{site.data.keyword.bpshort}}.|
| 1.12.0 | 11 July 2022 | Support for `agents` commands in {{site.data.keyword.bpshort}} from command-line.|
| 1.11.1 | 8 July 2022 | Support to fix the translation issue in {{site.data.keyword.bpshort}} from command-line.|
| 1.10.0 | 5 May 2022 | Support for `stop`, `force-stop`, and `terminate` in {{site.data.keyword.bpshort}} from command-line.|
| 1.9.0 | 25 April 2022 | Support for `Drift` detection in {{site.data.keyword.bpshort}} from command-line.|
| 1.8.1 | 17 April 2022 | Fixes alias deprecation display message for the {{site.data.keyword.bpshort}} JSON output.|
| 1.8.0 | 13 March 2022 | Supports passing `.tfvars` and `.json` files to plan and apply command. Usage of `ibmcloud terraform` command displays a warning message. The version also supports private {{site.data.keyword.bpshort}} endpoints through command-line and enhances the tabular view output to list the provisioned resource in {{site.data.keyword.bpshort}} workspace. |
| 1.7.3 | 4 March 2022 | Supports passing `vars` files to command-line plan command, displays `commit ID` in `ibmcloud schematics workspace get` command, and edited the `ibmcloud schematics workspace state show` command description. |
| 1.7.2 | 17 February 2022 | Supports Linux&trade; arm64 and Mac OS arm64 platform binaries. Fixes related to `stdout/stderr` stream, invalid `TF vars` file and translation are released. |
| 1.7.1 | 11 February 2022 | Support for trace logging and added integration tests for few commands. Fixes to update `env values metadata`, panic for invalid flags, and `ibmcloud schematics workspace output command` is unavailable. |
| 1.7.0 | 12 January 2022 | Displays Terraform v11.0 deprecation message after command execution. Fix command-line alias. Remove the appearance of the duplicate strings. Support global time in the log file. |
| 1.6.2 | 2 December 2021 | Support for non-English translations. Fix apply command `--var-file` and actions `--target not setting` argument. Fix a pipeline vulnerability.|
| 1.6.1 | 21 October 2021 | Supports `winrm` for {{site.data.keyword.bpshort}} actions. Added the `--inventory-connection-type`, `--bastion-credential-json` and `--credential-json` option value to the create and config updates. Updated non-English translations for the command-line. Fixed duplication display of `command-object` argument in `ibmcloud schematics jobs run` interactive mode.|
| 1.6.0 | 29 September 2021 | Support for `linux-ppc64le`, and `linux-s390x` binaries. Lists `Terraform v1.0` in the details panel. Display `Terraform v0.11` depreciation message in {{site.data.keyword.bpshort}} workspace page. Fixed the resource query list command returns values as empty string.|
| 1.5.12 | 02 September 2021 | Suppress status message for `--output json` flag.|
| 1.5.11 | 27 August 2021 | Added a flag `--pull-latest` to existing workspace **update** command. Fixed `BNPP` issue. Fixed the locale translations.|
| 1.5.10 | 11 August 2021 | Supports `Terraform v0.15`. Fixed locale translations.|
| 1.5.9 | 13 July 2021 | Fixed locale translations.|
| 1.5.8 | 08 July 2021 | Fixed shared data sets API path. Disabled shared data sets commands.|
| 1.5.7 | 04 June 2021 | Enhanced `ibmcloud schematics state list` command to display as tabular data with a new column `taint` status. Fixed `ibmcloud schematics job run` command with `--input` flag description. Fixed `ibmcloud schematics job run` command with `--output json` flag description. Fixed `ibmcloud schematics action update` command with `--credentials` flag and the locale translations.|
| 1.5.6 | 03 June 2021 | Updated `ibmcloud schematics workspace new` command to support `Terraform v0.14` and the locale translations.|
{: caption="Command line version history" caption-side="bottom"}


