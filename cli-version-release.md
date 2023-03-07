---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-27"

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
| 1.12.7 | 07 Feb 2023 | Bug fix to disable `API_AGENT_ATTACHMENT` in `us-south`, `us-east`, `eu-de`, `eu-gb` region.  |
| 1.12.6 | 30 Jan 2023 | Enhanced complex input support through blueprint `yaml` file. Fixes related to blueprint status output in the blueprint list table, index out of range for workspace action output, refresh token issue for long running `blueprint jobs`, and spinner panic fixes in `get blueprint`. |
| 1.12.5 | 18 Dec 2022 | Blueprint subcommand usage, revert to original usage of `blueprint create`, `blueprint apply` etc. Support for specifying complex inputs via a local YAML file using `-input-file` option.  
| 1.12.3 | 18 Nov 2022 |  Fixed blueprint subcommand usage, support `source type` in the blueprint list table. |
| 1.12.3 | 3 Nov 2022 |  Enhanced blueprint commands, CLI with the latest SDK update, and the workspace action command update. |
| 1.12.2 | 11 Aug 2022 | Included `--output` flag and bug fixes for all blueprint commands in  and released  {{site.data.keyword.bpshort}} v1.12.2 plug-in.|
| 1.12.1 | 26 July 2022 | Incorporated the bugs and fixes related to blueprint commands in {{site.data.keyword.bpshort}} and released v1.12.1 plug-in.|
| 1.12.0 | 11 July 2022 | Support for `Agents` commands in {{site.data.keyword.bpshort}} from command-line.|
| 1.11.1 | 8 July 2022 | Support for `blueprint` commands, and fix the translation issue in {{site.data.keyword.bpshort}} from command-line.|
| 1.10.0 | 5 May 2022 | Support for `stop`, `force-stop`, and `terminate` in {{site.data.keyword.bpshort}} from command-line.|
| 1.9.0 | 25 April 2022 | Support for `Drift` detection in {{site.data.keyword.bpshort}} from command-line.|
| 1.8.1 | 17 April 2022 | Fixes alias deprecation display message for the {{site.data.keyword.bpshort}} JSON output.|
| 1.8.0 | 13 March 2022 | Supports passing `.tfvars` and `.json` files to plan and apply command. Usage of `ibmcloud terraform` command displays warning message. The version also supports private {{site.data.keyword.bpshort}} endpoints through command-line, and enhances the tabular view output to list the provisioned resource in {{site.data.keyword.bpshort}} Workspace. |
| 1.7.3 | 4 March 2022 | Supports passing `vars` files to command-line plan command, displays `commit ID` in `ibmcloud schematics workspace get` command, and edited the `ibmcloud schematics workspace state show` command description. |
| 1.7.2 | 17 February 2022 | Supports Linux&trade; arm64 and Mac OS arm64 platform binaries. Fixes related to `stdout/stderr` stream, invalid `TF vars` file and translation are released. |
| 1.7.1 | 11 February 2022 | Support for trace logging and added integration tests for few commands. Fixes to update `env values metadata`, panic for invalid flags, and `ibmcloud schematics workspace output command` is disabled. |
| 1.7.0 | 12 January 2022 | Displays Terraform v11.0 deprecation message after command execution. Fix command-line alias. Remove the appearance of the duplicate strings. Support global time in the log file. |
| 1.6.2 | 2 December 2021 | Support for non-English translations. Fix apply command `--var-file` and actions `--target not setting` argument. Fix pipeline vulnerability.|
| 1.6.1 | 21 October 2021 | Supports `winrm` for {{site.data.keyword.bpshort}} Actions. Added the `--inventory-connection-type`, `--bastion-credential-json` and `--credential-json` option value to the create, and config updates. Updated non-english translations for the command-line. Fixed duplication display of `command-object` argument in `ibmcloud schematics jobs run` interactive mode.|
| 1.6.0 | 29 September 2021 | Support for `linux-ppc64le`, and `linux-s390x` binaries. Lists `Terraform v1.0` in the details panel. Display `Terraform v0.11` depreciation message in {{site.data.keyword.bpshort}} Workspaces page. Fixed the resource query list command returns values as empty string.|
| 1.5.12 | 02 September 2021 | Suppress status message for `--output json` flag.|
| 1.5.11 | 27 August 2021 | Added a flag `--pull-latest` to existing workspace **update** command. Fixed `BNPP` issue. Fixed the locale translations.|
| 1.5.10 | 11 August 2021 | Supports `Terraform v0.15`. Fixed locale translations.|
| 1.5.9 | 13 July 2021 | Fixed locale translations.|
| 1.5.8 | 08 July 2021 | Fixed shared data sets API path. Disabled shared data sets commands.|
| 1.5.7 | 04 June 2021 | Enhanced `ibmcloud schematics state list` command to display as tabular data with a new column `taint` status. Fixed `ibmcloud schematics job run` command with `--input` flag description. Fixed `ibmcloud schematics job run` command with `--output json` flag description. Fixed `ibmcloud schematics action update` command with `--credentials` flag and the locale translations.|
| 1.5.6 | 03 June 2021 | Updated `ibmcloud schematics workspace new` command to support `Terraform v0.14` and the locale translations.|
{: caption="Command line version history" caption-side="bottom"}
