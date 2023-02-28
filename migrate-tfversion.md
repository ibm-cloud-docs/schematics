---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-28"

keywords: migrating terraform version, terraform version migration for schematics 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Updating Terraform version  
{: #migrating-terraform-version}

The open source IaC tools that are used by {{site.data.keyword.bpshort}} evolve, with new versions of Terraform and Helm, and the supporting Terraform providers. Over time it is necessary for long lived workspace environments to be upgraded to utilize the latest version of Terraform as older versions are depreciated and go out of support.    
{: shortdesc}

All {{site.data.keyword.bpshort}} users are encourage to regularly upgrade to the latest Terraform version to ensure continuity of operations and support. {{site.data.keyword.bpshort}} follows the Hashicorp support model for Terraform releases and depreciates versions to the in line with Hashicorp. 

Terraform v1.0 was a major release for Terraform, marking the transition to a stable 1.x release. Hashicorp made [compatibility promises for the 1.x releases](https://developer.hashicorp.com/terraform/language/v1-compatibility-promises), that for core features, no additional changes are required to upgrade through the 1.x releases. 

In short, you aim to make upgrades between v1.x releases straightforward, requiring no changes to your configuration, no extra commands to run upgrade steps, and no changes to any automation you've set up around Terraform.

The Terraform v1.x series actively maintains for at least 18 months after v1.0.
{: note}

Upgrade at the 1.x releases requires no specific {{site.data.keyword.bpshort}} workspace actions. Review the Terraform [upgrade guides](https://developer.hashicorp.com/terraform/language/v1.1.x/upgrade-guides){: external} for release specific changes that might require TF config/template updates.  

## Upgrading the Terraform template version 1.x and above
{: #terraform-version-upgrade1x}

Since Terraform 1.0, {{site.data.keyword.bpshort}} Workspaces can be updated to more recent 1.x releases, via a simple change to the Workspace version. To update from 0.x releases refer to section [Upgrading the Terraform template version 0.x](/docs/schematics?topic=schematics-migrating-terraform-version#terraform-version-upgrade0x)


{{site.data.keyword.bpshort}} supports `Terraform_v1.x` and plans to make releases available `30-45 days` after general availability. It is recommended that Terraform templates use a version range constraint, such as, `>`, `>=`, or `~>` for the `required_version` parameter in the `versions.tf` of Terraform template, that allows upgrade for minor and patch releases. This allows {{site.data.keyword.bpshort}} to automatically adopt the latest patch or minor release of Terraform version as set by the Workspace version.   

```terraform
terraform {
required_version = "~> 1.1"
}
```
{: pre}

### Updating the Workspace Terraform 1.x version
{: #terraform-version-upgrade1x-process}

The in use version of Terraform for a Workspace can be updated via the {{site.data.keyword.bpshort}} Workspace [Update API](https://cloud.ibm.com/apidocs/schematics/schematics#replace-workspace){: external}.


The workspace terraform version parameter is of the form `terraform_v1.1` or `terraform_v1.2`

1. Select the workspace to be updated and verify that it is in `Normal` state and that a Plan operation does not generate any proposed resource changes. Save the `workspace_id` and note the region the workspace is hosted in. This is the prefix to the `workspace_id`. 
2. Update the Workspace terraform version using the {{site.data.keyword.cloud_notm}} CLI:
   - Login to the {{site.data.keyword.cloud_notm}} CLI with `ibmcloud login` 
   - Set the CLI target region with `ibmcloud target -r <region>` to be the same as the workspace you are updating. 
   - Generate an IAM oauth token to use with the {{site.data.keyword.bpshort}} API, with the command `ibmcloud iam oauth-tokens` 
   - Copy the token data and insert in to the following command text, replacing the string `<token-data>`, set `<terraform_version>` to the required Terraform version and the `<workspace_id>`:  
   - Execute the following `cURL` command:

    ```sh
    curl --request PUT --url https://schematics.cloud.ibm.com/v1/workspaces/<workspace_id> -H  "Authorization: Bearer <token-data>" -d '{"template_data":[{"type":"<terraform_version>"}]}'
    ```
    {: pre}

3. Verify in the Workspace settings page the TF version is now set to the desired version. 
4. Run a Generate Plan operation against the workspace. Validate that the command runs successfully without error and no unexpected messages are logged. The Plan should result in no proposed changes to the resources.  
5. Run a Apply Plan operation against the workspace. Validate that the command runs successfully without error and no unexpected messages are logged.
6. You have now successfully upgraded. 


## Upgrading the Terraform template version 0.x to 1.x
{: #terraform-version-upgrade0x}

At 0.x releases, Terraform version upgrade is a stepwise process, upgrading through each release. Upgrading does not support upgrading across multiple releases and must be performed, release by release. Some updates will require execution of Terraform `upgrade` commands to modify the config files also changes to the Terraform state file. These steps cannot be performed within {{site.data.keyword.bpshort}}. Workspace Terraform templates must be upgraded using a local copy of Terraform. Follow the steps below for upgrading the 0.x releases. 

| Version |	Recommendation|
| --- | --- |
| `v0.12` | Review the [v0.13 upgrade guide](https://developer.hashicorp.com/terraform/language/v1.1.x/upgrade-guides/0-13){: external} and follow the instructions [Upgrading a Terraform v0.12 Workspace to v0.13](/docs/schematics?topic=schematics-migrating-terraform-version#migrate-steps12). {{site.data.keyword.bpshort}} has deprecated `Terraform v0.12`.|
| `v0.13` | For the `Terraform v0.14` upgrade, you must run `terraform apply` with `Terraform v0.14` to complete its state format upgrades. If you get any errors, see the [v0.14 upgrade guide](https://developer.hashicorp.com/terraform/language/v1.1.x/upgrade-guides/0-14){: external}. Follow the instructions upgrade-13-to10|
| `v0.14` | You can upgrade directly to the [`Terraform v1.0`](https://developer.hashicorp.com/terraform/language/v1.1.x/upgrade-guides/1-1){: external} version. Review the [v0.15 upgrade guide](https://developer.hashicorp.com/terraform/language/v1.1.x/upgrade-guides/0-15){: external}.|
| `v0.15` | You can upgrade directly to the [`Terraform v1.0`](https://developer.hashicorp.com/terraform/language/v1.1.x/upgrade-guides/1-0){: external} version. Review the [v1.0 upgrade guide](https://developer.hashicorp.com/terraform/language/v1.1.x/upgrade-guides/1-0){: external}.|
{: caption="Terraform versions list" caption-side="bottom"}

## Upgrading a Terraform v0.12 Workspace to v0.13 
{: #migrate-steps12}

Upgrading a `v0.12` Workspace to use the `v0.13` version of Terraform is a multi-step task. You must carefully review the [Terraform upgrade guide](https://developer.hashicorp.com/terraform/language/upgrade-guides) for the related version upgrade. 

Use the following steps to upgrade to the current Terraform version in the {{site.data.keyword.bpshort}} Workspace.

1. Upgrade the Terraform configuration files to use the newer syntax and semantics.
2. Migrate the Terraform state file to be compatible with the newer version. {{site.data.keyword.bpshort}} does not support built in modification the Terraform state file. Therefore, you need to follow these steps.

   1. Prepare the upgraded version of Terraform configuration files and [Terraform state file](/docs/schematics?topic=schematics-schematics-cli-reference#state-file-cmds), in your local machine.
   2. [Create the new {{site.data.keyword.bpshort}} Workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) with the new Terraform configuration files and Terraform state file.
   3. [Delete the older workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-delete) without destroying the resources.

The following are the detailed steps to upgrade from 0.12 to 0.13:

1. Check whether your {{site.data.keyword.bpshort}} workspaces at `v0.12` has resources, the last apply was successful and the workspace is in `normal` state. Verify the Terraform configuration files and Terraform state file, are in a consistent state for `Terraform v0.12`.
2. Download or clone the Git repository that is used by your `Terraform v0.12` {{site.data.keyword.bpshort}} workspace to your local machine.
3. Install Terraform 0.13 on your local machine. 
4. Change directory to your cloned repository and upgrade your config files to `Terraform v0.13` by running the `Terraform v0.13upgrade` command. For more information, see [Upgrading to `Terraform v0.13` documentation](https://developer.hashicorp.com/terraform/language/v1.1.x/upgrade-guides/0-13){: external}. The upgrade command generates a `versions.tf` file with a `terraform` configuration block. 
5. Edit the `versions.tf` file to set the source paramater to `source = "IBM-Cloud/ibm"` as shown in the code block.

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

6. Download the Terraform state file from the existing {{site.data.keyword.bpshort}} Workspace, by using [{{site.data.keyword.bpshort}} state pull](/docs/schematics?topic=schematics-schematics-cli-reference#state-pull) command.

    When, the workspace is created with the `tfstate` {{site.data.keyword.bpshort}} considers it as a secure file. Also, you cannot pull the created `tfstate` file through UI. You need to use the command line to [pull](/docs/schematics?topic=schematics-schematics-cli-reference#state-pull) the state file and [create workspace](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new).
    {: important}
    
    Copy the downloaded state file as `terraform.tfstate` into the Terraform execution folder. 

7. Run the state replace provider command in on the command-line to update the {{site.data.keyword.cloud_notm}} provider version in the state file. 
    ```sh
    terraform state replace-provider registry.terraform.io/-/ibm registry.terraform.io/ibm-cloud/ibm.
    ```
    {: pre}

8. Verify that the updates are made to the `terraform.tfstate` file with Terraform version update from `0.12` to `>= 0.13` and the provider that is updated as `registry.terraform.io/ibm-cloud/ibm`.

9.	Push the upgraded TF config files and `version.tf` back to your Git repository.
10. Copy the content of modified `terraform.tfstate` file to `state.json` file.
11. Create or update a `workspace.json` file as shown in the code block.

        ```json
        {
        "name": "gb",
        "type": [
            "terraform_v0.13"
        ],
        "description": "migration workspace",
        "template_repo": {
            "url": "Provide your Git repository link"
        },
        "workspace_status" : {
            "frozen": false
        },
        "template_data": [{
            "folder": ".",
            "type": "terraform_v0.13"
        }]
        }
        ```
        {: codeblock}

12. Run these commands through command-line to create a new Terraform `v0.13` workspace 
    -  `ibmcloud schematics workspace new --file workspace.json --state state.json`.
    -  `ibmcloud schematics workspace get --id  <workspace-id> --json`.
        If your workspace status is not `inactive`, wait for few seconds and retry the command.
        {: note}

    - `ibmcloud schematics plan id <workspace id>`.
    - `ibmcloud schematics job get --id <job-id form plan> --json`.
        If your workspace plan status is not `success`, wait for few seconds and retry the command.
        {: note}
   
    - `ibmcloud schematics apply --id <workspace id>`.
    - `ibmcloud schematics job get --id <job-id from apply> --json`.

    
13. [Optional] you can delete the {{site.data.keyword.bpshort}} workspace that uses `Terraform v0.12`. 

    Do not destroy the resources that are used by your old workspace.
    {: note}

## Upgrade Terraform template from `v0.13` and higher to `v1.0`
{: #upgrade-13-to10}

Versions 0.13 through 0.15 require a stepwise upgrade, 0.13 to 0.14, 0.14 to 0.15, 0.15 to 1.0.  

The process is the same for each version step. It is mandatory that a Terraform Apply is run after each version change. This updates the Terraform state file with schema changes related to that version and that version only. After successfully upgrading a single version, the next version update can be performed.   

1. Read the Terraform [upgrade guide](https://developer.hashicorp.com/terraform/language/v1.1.x/upgrade-guides){: external} for the release and implement any required config changes.  
2. Follow the process outlined in [Upgrading the Terraform template version 1.x and above](/docs/schematics?topic=schematics-migrating-terraform-version#terraform-version-upgrade1x-process) to upgrade a single version to the target version.  
3. Verify in the Workspace settings page the TF version is now set to the desired version. 
4. Run a Generate Plan operation against the workspace. Validate that the command runs successfully without error and no unexpected messages are logged. The Plan should result in no proposed changes to the resources.  
5. Run a Apply Plan operation against the workspace. This step is **mandatory** to perform a Terraform state file update. Validate that the command runs successfully without error and no unexpected messages are logged.
6. You have now successfully upgraded a single version step. 
