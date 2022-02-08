---

copyright:
  years: 2017, 2022
lastupdated: "2022-02-08"

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
| 1.7.0 | 12 January 2022 | Displays Terraform v11.0 deprecation message after command execution. Fix command-line alias. Remove the appearance of the duplicate strings.  Support globalized time in the log file. |
| 1.6.2 | 2 December 2021 | Supports for non-English translations. Fix apply command `--var-file` and actions `--target not setting` argument. Fix pipeline vulnerability.|
| 1.6.1 | 21 October 2021 | Supports `winrm` for {{site.data.keyword.bpshort}} actions. Added the `--inventory-connection-type`, `--bastion-credential-json` and `--credential-json` option value to the **create**, and **update** commands. Updated non-english translations for the command-line. Fixed duplication display of `command-object` argument in `ibmcloud schematics jobs run` interactive mode.|
| 1.6.0 | 29 September 2021 | Support for `linux-ppc64le`, and `linux-s390x` binaries. List `Terraform v1.0` while listing workspaces and in details panel. Display `Terraform v0.11` depreciation message in Schematics workspace page. Fixed the resource query list command returns values as empty string.|
| 1.5.12 | 02 September 2021 | Suppress status message for `--output json` flag.|
| 1.5.11 | 27 August 2021 | Added a new flag `--pull-latest` to existing workspace **update** command. Fixed `BNPP` issue.:Fixed the locale translations.|
| 1.5.10 | 11 August 2021 | Supports Terraform v0.15. Fixed locale translations.|
| 1.5.9 | 13 July 2021 | Fixed locale translations.|
| 1.5.8 | 08 July 2021 | Fixed shared datasets API path. Disabled shared datasets commands.|
| 1.5.7 | 04 June 2021 | Enhanced `ibmcloud schematics state list` command to display as tabular data with a new column `taint` status. Fixed `ibmcloud schematics job run` command with `--input` flag description. Fixed `ibmcloud schematics job run` command with `--output json` flag description. :Fixed `ibmcloud schematics action update` command with `--credentials flag. Fixed the locale translations.|
| 1.5.6 | 03 June 2021 | Updated `ibmcloud schematics workspace new` command to support Terraform v0.14. Fixed the locale translations.|
{: caption="Command line version history" caption-side="bottom"}
