---

copyright:
  years: 2017, 2024
lastupdated: "2024-10-08"

keywords: schematics workspaces, workspaces, schematics, terraform templates, precedence, terraform version

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Terraform version precedence 
{: #sch-wks-tf-version-precedence}

Creating {{site.data.keyword.bpshort}} Workspace, the Terraform version is determined based on the following sources. The precedence order is as follows:

Terraform version declared in create workspace request
:   If the Terraform version is explicitly specified in the create workspace request, then the create workspace Terraform version takes the highest precedence and are used in the workspace execution.

Terraform version in a user's Terraform files
:   If the Terraform version is not specified in the create workspace request, {{site.data.keyword.bpshort}} checks the `required_version` attribute in the user's Terraform configuration files and takes the precedence, if the version is defined and is valid.

Default {{site.data.keyword.bpshort}} Terraform version
:   If the create workspace request or the user's Terraform files do not specify the Terraform version, {{site.data.keyword.bpshort}} falls back to its default Terraform version.
