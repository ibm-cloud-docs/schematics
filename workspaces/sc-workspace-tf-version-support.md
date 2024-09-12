>---

copyright:
  years: 2017, 2024
lastupdated: "2024-09-12"

keywords: schematics workspaces, workspaces, schematics, terraform templates, precedence, terraform version

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Terraform version precedence 
{: #sch-wks-tf-version-precedence}

In {{site.data.keyword.bpshort}} managing the Terraform version is crucial for helping ensure compatibility and consistency with your IaC deployments. Terraform version lets you specify the versions of Terraform that are allowed for your configuration. The precedence table helps ensure that your Terraform code runs with a compatible version of Terraform, avoiding potential issues due to version mismatches.

| Serial Number | Level | Description |
| -- | -- | --|
| 1 | Terraform block in `versions.tf` file  | When you specify the `required_version` parameter in the `versions.tf` file. For example, `required_version = ">= 1.4.0"`. You can specify Terraform `required_versions` that are higher than the default `MAJOR.MINOR.PATH` in the Terraform configuration file. For example, `required_version = ">= 1.4.0"` or `required_version = ">= 1.2.0"` {{site.data.keyword.bpshort}} considers the provided version as higher precedence. If the `required_version` is deprecated, then {{site.data.keyword.bpshort}} falls back to the default version.|
| 2 | Terraform version set through workspace UI | When the Terraform version is specified in the workspace UI, {{site.data.keyword.bpshort}} considers the specified version to run your workspace.|
| 3 | Terraform block in version | You can define the Terraform version in the version parameter. For example, `version = ">= 1.67.0, < 2.0.0"`. {{site.data.keyword.bpshort}} considers the default version. |
| 4 | Default version | When you select the default version in the template. {{site.data.keyword.bpshort}} considers the default version. |
| 5 | Deprecated version | When you use the deprecated version in the workspace. {{site.data.keyword.bpshort}} considers to the default version. Here you are excepted to get an error when the features in the default version are unsupported. |
{: caption="Compatible version of Terraform for workspace" caption-side="top"}

