---

copyright:
  years: 2017, 2022
lastupdated: "2022-05-13"

keywords: schematics best practices, best practices workspace, security best practice, best practices actions

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Best practices for securing the {{site.data.keyword.bpshort}} objects
{: #bp-secure-objects}

{{site.data.keyword.bplong}} leverages open source projects, such as Terraform, Ansible, Red Hat OpenShift on IBM Cloud, Operators, and Helm, and delivers to you as a managed service. Rather than installing each open source project on your machine, and learning the API or command-line, you declare the tasks that you want to run in {{site.data.keyword.cloud}} and watch Schematics run these tasks for you.

Take time to review the suggested best practices to lower you security risks for all production, staging, and test servers in your cloud infrastructure. This list is an excellent starting point to increase the security of your cloud infrastructure.
{: shortdesc}

## What are the best practices that I must follow while developing the Terraform templates, and while publishing the same in the Git repositories?
{: #bp-security-template}

Follow these practices while developing, and publishing the Terraform template in the Git repositories.
- Create Terraform template by using `Terraform version1.0` or higher and latest [IBM Cloud provider](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest).
- Check whether pre-commit hooks are executed to inspect your code meets Terraform standards. Refer to [sample repository that contains pre-commit hook](https://github.com/terraform-ibm-modules/terraform-ibm-iam/blob/main/.pre-commit-config.yaml).
- Check whether your repository uses Terratest framework to validate your Terraform resources and data source to provision. Refer to [sample validated Terraform repository](https://github.com/terraform-ibm-modules/terraform-ibm-iam/blob/main/.github/workflows/validate_terraform.yml) to run Terratest.
- Check whether your repository contains `gitignore` for any files that are not tracked by git remain untracked.
- Add required license file for your template.
- Do not set your sensitive variable as default in the configuration files.
- Check whether you have marked your secured variables or output as sensitive by default to secure your data.
- Check your script execution do not take more than `60 minutes`, when your template is using `null resources` to provision or configure your resources.
- Do use only [allowed list file extensions](docs/schematics?topic=schematics-faqs#clone-file-extension) in your respository.

## What are the best practices that I must follow while creating a Workspace for the Terraform template?
{: #bp-security-wks}

Follow these practices while creating a Workspace for the Terraform template.
- Check whether you have the [required permissions](/docs/schematics?topic=schematics-access) to create a workspace. 
- Check whether the `location` and the `url` endpoint are pointing to the same region when you create or update the {{site.data.keyword.bpshort}} workspace. For more information, about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
- Check whether you want to [delete the workspace](/docs/schematics?topic=schematics-workspace-setup&interface=ui#del-workspace) and destroy the associated cloud resources, or both. This job cannot be undone. If you remove the workspace and keep the cloud resources, you need to manage the resources with the resource list or command-line.
- Do not use one workspace to manage an entire staging or production environment. When you deploy all your {{site.data.keyword.cloud_notm}} resources into a single workspace, it can become difficult for various teams to coordinate updates and manage access for these resources.

## What are the best practices that I must follow while creating an Action for the Ansible template?
{: #bp-security-ansible}

Follow these practices while creating an Action for the Ansible template.
- Check whether the `location` and the `url` endpoint are pointing to the same region when you create or update the {{site.data.keyword.bpshort}} workspace. For more information, about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
- You cannot delete or stop a running job of your {{site.data.keyword.bpshort}} action. To make changes to your action, wait for the job to complete, then change your settings, and click **Check action** or **Run action** again.
- In the UI console, there is no limit set to display the job logs. Every `30 seconds` the job logs gets automatically refreshed.
- As the account owner or an authorized account administrator, you can assign IAM service access roles to your users. The IAM service access roles determine the actions that you can perform on an {{site.data.keyword.bplong_notm}} resource, such as a workspace or an action. To avoid assigning access policies to individual users, consider creating [IAM access groups](/docs/account?topic=account-groups).

## How can I ensure that the sensitive data used by the Terraform automation, do not leak in the logs or outputs?
{: #bp-security-leak-log}

You need to set the variable or output parameter as sensitive to make sure that the data is not leaked in the logs or outputs.

## How can I protect the access to Workspaces & Action and the data stored by them?
{: #bp-security-data}

As the account owner or an authorized account administrator, you can assign IAM service access roles to your users. The IAM service access roles determine the actions that you can perform on an {{site.data.keyword.bplong_notm}} resource, such as a workspace or an action. To avoid assigning access policies to individual users, consider creating [IAM access groups](/docs/account?topic=account-groups).

Your Workspaces and Actions data store depends on the location where you create your Workspace or an Action. For more information, see [securing your data in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-secure-data).

## How is my Workspace and Action data protected by {{site.data.keyword.bplong_notm}}? How is the state file generated by Terraform protected by {{site.data.keyword.bplong_notm}}?
{: #bp-security-statefile}

Follow these practices to protect your Workspace and Action's data.
- Use Cloud Identity and Access Management to control access to a {{site.data.keyword.bpshort}} workspace and related {{site.data.keyword.cloud_notm}} resources.
- Secure the source repository for your Terraform template, including access control, security settings, collaboration, and version control.
- Secure the {{site.data.keyword.cloud_notm}} resources that you create by using the security features that are provided by the resource offering.
- Use the provided tools of your {{site.data.keyword.cloud_notm}} resources to apply security patches, access controls, and encryption to your resources.

You need to specify the right roles and permissions to an user who controls the state file. For more information, about {{site.data.keyword.bpshort}} service access roles and required permissions for 
- [Workspace Permissions](/docs/schematics?topic=schematics-access#workspace-permissions) 
- [Action permissions](/docs/schematics?topic=schematics-access#action-permissions)
- [KMS permissions](/docs/schematics?topic=schematics-access#kms-permissions)

## Does {{site.data.keyword.cloud_notm}} operators have access to my Workspace & Action data stored in {{site.data.keyword.bpshort}}? What can I do to protect the access with my own keys?
{: #bp-security-protect}

No, as the account owner or an authorized account administrator, you need to assign IAM service access roles to your {{site.data.keyword.cloud_notm}} operators. The IAM service access roles determine the jobs that you can perform on an {{site.data.keyword.bplong_notm}} resource, such as a workspace or an action. To avoid assigning access policies to individual users, consider creating [Workspace permission](/docs/schematics?topic=schematics-access&interface=ui#workspace-permissions), or [Action permission](/docs/schematics?topic=schematics-access&interface=ui#action-permissions).

You can protect the keys access by using [KMS permission](/docs/schematics?topic=schematics-access&interface=ui#kms-permissions).

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
