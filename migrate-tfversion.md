---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-23"

keywords: migrating terraform version, terraform version migration for schematics 

subcollection: schematics

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

---

# Migrating Terraform version in {{site.data.keyword.bpshort}} workspace
{: #migrating-terraform-version}

The driver to migrate might come from business factors such as cost reduction, or consolidation. You also might want to migrate to be more cloud native or adopt new technologies. Regardless of the reason, migration can be as simple as migrating a Terraform version in variable file, or it can be as complex as migrating a piece of your application to a more complex environment where you need to migrate an entire template with all of the underlying components.
{: shortdesc}

## Steps to migrate or upgrade
{: #migrate-steps}

The steps to frame your migration journey to support the latest Terraform version in the {{site.data.keyword.bpshort}} workspace.
{: shortdesc}

1. Provision the resources with Terraform apply for Terraform v0.12, so that your state file is generated.
2. Move Terraform v0.12 to Terraform v0.13.
3. Upgrade your repository with Terraform v0.13. This generates a `versions.tf` file. You need to edit by uncommenting the source parameter in the `versions.tf` file as shown in the code block.

  `versions.tf` file

    ```
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
          # https://www.terraform.io/docs/language/providers/configuration.html#provider-source
        }
      }
      required_version = ">= 0.13"
    } 
    ```
    {: codeblock}
4. Run the state command 
  
  ```
  terraform state replace-provider registry.terraform.io/-/ibm registry.terraform.io/ibm-cloud/ibm.

  ```
  {: pre}

5. Run Terraform plan and apply commands. For more information, refer [provisioning {{site.data.keyword.cloud_notm}} commands](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-manage_resources#provision_resources).

### Additional steps
{: #additional-steps}

Along with the given steps, you need to ensure these steps are followed:

1. Download state file from existing workspace using the [POSTMAN](/docs/schematics#get-workspace-template-state) URL to access the Terraform statefile or through local workspace environment. For more information, refer to [Terraform state file commands](/docs/schematics?topic=schematics-schematics-cli-reference#statefile-cmds).
2. Delete an [existing workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-delete). You should not destroy the resources.
3. Migrate the configuration files and state file to higher version of Terraform, refer to [step 1 till step 5](#migrate-steps).
4. Create a workspace with the migrated configuration file and the state file for the migration to be successful.

