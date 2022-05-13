---

copyright:
  years: 2017, 2022
lastupdated: "2022-05-13"

keywords: schematics best practices, best practices workspace, security best practice, best practices actions

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Best practices for securing the {{site.data.keyword.bpshort}} objects
{: #bp-secure-workspace}

Take time to review the suggested best practices to lower you security risks for all production, staging, and test servers in your cloud infrastructure. This list is an excellent starting point to increase the security of your cloud infrastructure.
{: shortdesc}

## What makes a Terraform template secure?
{: #bp-template-strategy}

- Create environment variables for your account authentication
- Check your Terraform template contains `gitignore` for any files that are not tracked by `git` remain untracked.
- Do not set your sensitive variable as default in the configuration files.
- Ensure you have marked your secured variables or output as sensitive by default to secure your data.
- Always specify the Terraform version and the {{site.data.keyword.cloud}} provider version in your `versions.tf`.
- Check your script execution do not take more than `60 minutes`, when your template is using `null resources` to provision or configure your resources.
- Do use only [allowed list file extensions] in your respository.

## How can the Terraform developers ensure that the sensitive data is not leaked in the log files? 
{: #bp-security-leak}

You need to check whether the variable or output parameter as a sensitive to ensure that the data is not leaked in the log files.

## Can I use `tfvars` files with the {{site.data.keyword.cloud}} provider templates?
{: #bp-security-tfvars}

`tfvars` file is a local variables file that you can use to store sensitive information, such as your IBM Cloud API key or classic infrastructure user name when your use native Terraform. However, to secure your variable data, you cannot provide `tfvars` file in {{site.data.keyword.bpshort}}.

## How can I protect unauthorized users from accessing sensitive data from the state file?
{: #bp-security-protect}

You need to specify the right roles and permissions to an user who controls the state file. For more information, about {{site.data.keyword.bpshort}} service access roles and required permissions forrefer to 
- [Workspace Permissions](/docs/schematics?topic=schematics-access#workspace-permissions) 
- [Action permissions](/docs/schematics?topic=schematics-access#action-permissions)
- [KMS permissions](/docs/schematics?topic=schematics-access#kms-permissions)

## Identity and access management
{: #bp-security-iam}

Create an IAM access group for your users and assign service access policies to {{site.data.keyword.bplong_notm}} and the resources that you want your users to work with. IAM users are attached to access groups, for more information, refer to [Setting up access for your users](/docs/schematics?topic=schematics-access#access-setup)


## Networking
{: #bp-security-networking}

Customize settings to ensure protection against unwanted access and encrypt to your workspaces, and actions.
- [KMS integration for BYOK or KYOK](/docs/schematics?topic=schematics-kms-integration&interface=ui)
- [Activity tracking integration](/docs/schematics?topic=schematics-at-integration&interface=ui)
- [Monitoring integration](/docs/schematics?topic=schematics-monitoring-integration&interface=ui)
- [Logging integration](/docs/schematics?topic=schematics-logging-integration&interface=ui)


## Data protection
{: #bp-security-data-protection}

Safeguard your information from corruption, compromise, or loss. Confirm that all attached and unattached data disks are encrypted.
- [KMS integration for BYOK or KYOK](/docs/schematics?topic=schematics-kms-integration&interface=ui)
- [Managing data encryption](/docs/schematics?topic=schematics-secure-data#pi-encrypt)
- Restrict network access for all the resources to provision by [Allowing specific IP addresses](/docs/schematics?topic=schematics-allowed-ipaddresses&interface=ui)

## Next steps
{: #bp-account-security-next-steps}

Check out [Getting started with Security and Compliance Center](/docs/security-compliance?topic=security-compliance-getting-started).
