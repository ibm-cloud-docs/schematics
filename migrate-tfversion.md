---

copyright:
  years: 2017, 2022
lastupdated: "2022-03-04"

keywords: migrating terraform version, terraform version migration for schematics 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Migrating Terraform version in {{site.data.keyword.bpshort}} workspace
{: #migrating-terraform-version}

The driver to migrate might come from business factors such as cost reduction, or consolidation. You also might want to migrate to be more cloud native or adopt new technologies. Regardless of the reason, migration can be as simple as migrating a Terraform version in variable file, or it can be as complex as migrating a piece of your application to a more complex environment where you need to migrate an entire template with all the underlying components.
{: shortdesc}

## Recommendation to upgrade Terraform version 
{: #terraform-version-upgrade}

The table summarizes the recommendations to upgrade to the latest Terraform version.

{{site.data.keyword.bplong_notm}} suggests to use Terraform templates of `terraform_v1.0` and higher version. {{site.data.keyword.bpshort}} supports the stable release of Terraform version 1.1, through `terraform_v1.0`. The terraform template must use the version constraint, such as `>` or `>=` or `~>` for the `required_version` of Terraform, to automatically pick the latest version.
terraform {
  required_version = "~> 1.1"
}
{: note}

|Current Version|	Recommendation|
| ---| ---|
| `v0.11` | Use the [`terraform 0.12checklist`](https://www.terraform.io/language/upgrade-guides/0-12#pre-upgrade-checklist){: external} command to detect and fix to be addressed by referring [v0.12 Upgrade Guide](https://www.terraform.io/language/upgrade-guides/0-12){: external} before upgrading to `v0.12`. **Note** {{site.data.keyword.bpshort}} depreciated `Terraform v0.11`.|
| `v0.12` | Use the [v0.13 upgrade guide](https://www.terraform.io/language/upgrade-guides/0-13){: external} your configuration file. **Note** {{site.data.keyword.bpshort}} started depreciation process of `Terraform v0.12`.|
| `v0.13` | To the latest `Terraform v0.14` upgrade, you must run the `terraform apply` with `Terraform v0.13` to complete its state format upgrades. If you get any errors, refer to, the [v0.14 upgrade guide](https://www.terraform.io/language/upgrade-guides/0-14){: external}.|
| `v0.14` | You can upgrade directly to the latest [`Terraform v1.0`](https://www.terraform.io/language/upgrade-guides/1-1){: external} version and you must run the `terraform apply` with `Terraform v0.14`. If you get any errors, refer to, the [v0.15 upgrade guide](https://www.terraform.io/language/upgrade-guides/0-15){: external}.|
| `v0.15` | You can upgrade directly to the latest [`Terraform v1.0`](https://www.terraform.io/language/upgrade-guides/1-0){: external} version, you must run the `terraform apply` with `terraform v0.15`. `Terraform v1.0` is a continuation of the `v0.15` series, hence `v1.0.0` and later are directly backward-compatible with Terraform v0.15.5.|
{: caption="List of Terraform version" caption-side="bottom"}


## Upgrade Terraform version in {{site.data.keyword.bpshort}} workspace
{: #migrate-steps}

Upgrading the {{site.data.keyword.bpshort}} workspace to use the latest version of the Terraform, may be required to leverage the latest features in Terraform. You must carefully review the [Terraform upgrade guide](https://www.terraform.io/language/upgrade-guides) before attempting to upgrade to the next version. 

The upgrade requires the following steps to support the latest Terraform version in the {{site.data.keyword.bpshort}} workspace.

1. Upgrade the Terraform configuration files to use the newer syntax and semantics.
2. Migrate the Terraform state file to be compatible with the newer version. {{site.data.keyword.bpshort}} does not support built in upgrade of the Terraform version. therefore, you must do the following:

   1. Prepare the upgraded version of Terraform configuration files and Terraform state file, in your local machine.
   2. Create a new {{site.data.keyword.bpshort}} workspace with the new Terraform configuration files and Terraform state file.
   3. Delete the older workspace (without destroying the resources).

Here are the detailed steps that you can follow to upgrade.

1. As a prerequisites, ensure {{site.data.keyword.bpshort}} workspace is created, plan is generated, and applied a job for your resources by using Terraform v0.12.  Ensure Terraform configuration files and Terraform state file, are in a consistent state for Terraform v0.12.
2. Download or clone the Git repository used by your Terraform v0.12 {{site.data.keyword.bpshort}} workspace to your local machine.
3. Change directory to your cloned repository and upgrade your repository to Terraform v0.13 by executing `Terraform v0.13upgrade` command. For more information, see [Upgrading to Terraform v0.13 documentation](https://www.terraform.io/language/upgrade-guides/0-13){: external}. The upgrade command generates a `versions.tf` file.
4. Edit `versions.tf` file to deselect the source parameter and add `source = "IBM-Cloud/ibm"` as shown in the code block.

    `versions.tf` file

    ```terraform
    terraform {
        required_providers {
        ibm = {
          # TF-UPGRADE-TODO
          #
          # No source detected for this provider. You must add a source address
          # in the following format:
          #
          source = "IBM-Cloud/ibm"
          #
          # For more information, see the provider source documentation:
          #
        }
        }
        required_version = ">= 0.13"
    } 
    ```
    {: codeblock}

5. Download the Terraform state file from the existing {{site.data.keyword.bpshort}} workspace, by using [{{site.data.keyword.bpshort}} state file](/docs/schematics?topic=schematics-schematics-cli-reference#state-list) command or through [REST API call](/apidocs/schematics/schematics#get-workspace-state) documentation to your local machine. 

6. Execute the state replace provider command in terminal to update the Terraform version.
    ```sh
    terraform state replace-provider registry.terraform.io/-/ibm registry.terraform.io/ibm-cloud/ibm.
    ```
    {: pre}

7. Verify the updates are made to the `terraform.tfstate` file with Terraform version getting updated from `0.12` to `>= 0.13` and provider updated as `registry.terraform.io/ibm-cloud/ibm`. 
8.	Push the upgraded version of the Terraform configuration files both `terraform.tfstate` and `version.tf` to your Git repository.
9.	Create a {{site.data.keyword.bpshort}} workspace by using the updated Git repository.
10. Generate a [`plan`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-plan) and [`apply`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-apply) the plan to view your workspace with the resources are now using the Terraform v0.13.
11. [Optional] you can delete the {{site.data.keyword.bpshort}} workspace that points to Terraform v0.12. 

    Do not delete or destroy the resources that are used by your workspace.
    {: note}
