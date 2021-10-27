---

copyright:
  years: 2017, 2021
lastupdated: "2021-10-27"

keywords: schematics command-line reference, schematics commands, schematics command-line, schematics reference, command-line, change log, command-line releases

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# CLI version history 
{: #cli_version-releases}

Find a summary of changes for each version of {{site.data.keyword.bpshort}} CLI plug-in. Be sure to keep your CLI up-to-date so that you can use all of the available commands and their options.
{: shortdesc}

| Version | Release date | Changes |
| ----- | ------- | -------------- |
| 1.6.1 | 21 October 2021 | <ul><li>Supports `winrm` for {{site.data.keyword.bpshort}} actions. Added the `--inventory-connection-type`, `--bastion-credential-json`, and `--credential-json` option value to the **create**, and **update** commands.</li><li>Updated non-english translations for the command-line. </li><li>Fixed duplication display of `command-object` argument in `ibmcloud schematics jobs run` interactive mode.</li></ul>|
| 1.6.0 | 29 September 2021 | <ul><li>Support for `linux-ppc64le`, and `linux-s390x` binaries. </li><li>List `Terraform v1.0` while listing workspaces and in details panel.</li><li>Display `Terraform v0.11` depreciation message in Schematics workspace page.</li><li>Fixed the resource query list command returns values as empty string.</li></ul>|
| 1.5.12 | 02 September 2021 | <ul><li>Suppress status message for `--output json` flag. </li></ul>|
| 1.5.11 | 27 August 2021 | <ul><li>Added a new flag `--pull-latest` to existing workspace **update** command. </li><li>Fixed `BNPP` issue.</li><li>Fixed the locale translations.</li></ul>|
| 1.5.10 | 11 August 2021 | <ul><li>Supports Terraform v0.15</li><li>Fixed locale translations.</li></ul>|
| 1.5.9 | 13 July 2021 | <ul><li>Fixed locale translations.</li></ul>|
| 1.5.8 | 08 July 2021 | <ul><li>Fixed shared datasets API path.</li><li>Disabled shared datasets commands.</li></ul>|
| 1.5.7 | 04 June 2021 | <ul><li>Enhanced `ibmcloud schematics state list` command to display as tabular data with a new column `taint` status.</li><li>Fixed `ibmcloud schematics job run` command with `--input` flag description.</li><li>Fixed `ibmcloud schematics job run` command with `--output json` flag description.</li><li>Fixed `ibmcloud schematics action update` command with `--credentials flag.</li><li>Fixed the locale translations.</li></ul>|
| 1.5.6 | 03 June 2021 | <ul><li>Updated `ibmcloud schematics workspace new` command to support Terraform v0.14.</li><li>Fixed the locale translations.</li></ul>|
