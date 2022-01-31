---

copyright:
  years: 2017, 2022
lastupdated: "2022-01-31"

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
| 1.7.0 | 12 January 2022 | Displays Terraform v11.0 deprecation message after command exection.  \n Fix command-line alias.  \n Remove the appearance of the duplicate strings.  \n  Support globalized time in the log file. |
| 1.6.2 | 2 December 2021 | Supports for non-English translations.  \n Fix apply command `--var-file` and actions `--target not setting` argument.  \n Fix pipeline vulnerability.|
| 1.6.1 | 21 October 2021 | Supports `winrm` for {{site.data.keyword.bpshort}} actions.  \n Added the `--inventory-connection-type`, `--bastion-credential-json` and `--credential-json` option value to the **create**, and **update** commands.:Updated non-english translations for the command-line.  \n Fixed duplication display of `command-object` argument in `ibmcloud schematics jobs run` interactive mode.|
| 1.6.0 | 29 September 2021 | Support for `linux-ppc64le`, and `linux-s390x` binaries.  \n List `Terraform v1.0` while listing workspaces and in details panel.  \n Display `Terraform v0.11` depreciation message in Schematics workspace page.  \n Fixed the resource query list command returns values as empty string.|
| 1.5.12 | 02 September 2021 | Suppress status message for `--output json` flag.|
| 1.5.11 | 27 August 2021 | Added a new flag `--pull-latest` to existing workspace **update** command.  \n Fixed `BNPP` issue.:Fixed the locale translations.|
| 1.5.10 | 11 August 2021 | Supports Terraform v0.15.  \n Fixed locale translations.|
| 1.5.9 | 13 July 2021 | Fixed locale translations.|
| 1.5.8 | 08 July 2021 | Fixed shared datasets API path.  \n Disabled shared datasets commands.|
| 1.5.7 | 04 June 2021 | Enhanced `ibmcloud schematics state list` command to display as tabular data with a new column `taint` status.  \n Fixed `ibmcloud schematics job run` command with `--input` flag description.  \n Fixed `ibmcloud schematics job run` command with `--output json` flag description. :Fixed `ibmcloud schematics action update` command with `--credentials flag.  \n Fixed the locale translations.|
| 1.5.6 | 03 June 2021 | Updated `ibmcloud schematics workspace new` command to support Terraform v0.14.  \n Fixed the locale translations.|
{: caption="Command line version history" caption-side="bottom"}
