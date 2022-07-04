---

copyright:
  years: 2017, 2022
lastupdated: "2022-05-17"

keywords: schematics best practices, best practices workspace, security best practice, best practices actions

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Best practices for securing the {{site.data.keyword.bpshort}} objects
{: #bp-secure-objects}

{{site.data.keyword.bplong}} leverages open source projects, such as Terraform, Ansible, {{site.data.keyword.openshiftshort}}, Operators, and Helm, and delivers to you as a managed service. Rather than installing each open source project on your machine, and learning the API or command-line, you declare the tasks that you want to run in {{site.data.keyword.cloud}} and watch {{site.data.keyword.bpshort}} run these tasks for you.

Take time to review the suggested best practices to lower you security risks for all production, staging, and test servers in your cloud infrastructure. This list is an excellent starting point to increase the security of your cloud infrastructure.
{: shortdesc}

## Best practices for creating Terraform Templates or modules in Git repositories
{: #bp-secure-repo}

### What are the best practices that I must follow while developing the Terraform templates, and while publishing the same in the Git repositories?
{: #bp-template-strategy}

Follow these practices while developing, and publishing the Terraform template in the Git repositories.
- Create Terraform template by using `Terraform version1.0` or higher and latest [IBM Cloud provider](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest).
- Create environment variables are created for all your credentials.
- Check whether pre-commit hooks are executed to inspect your code meets Terraform standards. refer to, [sample repository that contains pre-commit hook](https://github.com/terraform-ibm-modules/terraform-ibm-iam/blob/main/.pre-commit-config.yaml).
- Check whether your repository uses Terratest framework to validate your Terraform resources and data source to provision. refer to, [sample validated Terraform repository](https://github.com/terraform-ibm-modules/terraform-ibm-iam/blob/main/.github/workflows/validate_terraform.yml) to run Terratest.
- Check whether your repository contains `gitignore` for any files that are not tracked by git remain untracked.
- Add required license file for your template.
- Do not set your sensitive variable as default in the configuration files.
- Check whether you have marked your secured variables or output as sensitive by default to secure your data.
- Check your script execution do not take more than `60 minutes`, when your template is using `null resources` to provision or configure your resources.
- Do use only [allowed list file extensions](/docs/schematics?topic=schematics-faqs&interface=api#clone-file-extension) in your respository.

### Can I create `tfvars` files with the {{site.data.keyword.cloud}} provider templates?
{: #bp-security-tfvars}

`tfvars` file is a local variables file that you can use to store sensitive information, such as your {{site.data.keyword.cloud_notm}} API key or classic infrastructure user name when your use native Terraform. However, to secure your variable data, you cannot provide `tfvars` file in {{site.data.keyword.bpshort}}.

### How can the Terraform developers ensure that the sensitive data is not leaked in the log files? 
{: #bp-security-leak}

Developer need to check whether the variable or output parameter as a sensitive to ensure that the data is not leaked in the log files.

## Best practices of managing {{site.data.keyword.bpshort}} Workspaces 
{: #bp-workspaces}

### What are the best practices that I must follow while creating a Workspace for the Terraform template?
{: #bp-security-wks}

Follow these practices while creating a Workspace for the Terraform template.
- Check whether you have the [required permissions](/docs/schematics?topic=schematics-access) to create a workspace. 
- Check whether the `location` and the `url` endpoint are pointing to the same region when you create or update the {{site.data.keyword.bpshort}} Workspace. For more information, about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
- Check whether you want to [delete the Workspace](/docs/schematics?topic=schematics-workspace-setup&interface=ui#del-workspace) and destroy the associated cloud resources, or both. This job cannot be undone. If you remove the workspace and keep the cloud resources, you need to manage the resources with the resource list or command-line.
- Do not use one workspace to manage an entire staging or production environment. When you deploy all your {{site.data.keyword.cloud_notm}} resources into a single workspace, it can become difficult for various teams to coordinate updates and manage access for these resources.

### How can I ensure that the sensitive data used by the Terraform automation, do not leak in the logs or outputs?
{: #bp-security-leak-log}

You need to set the variable or output parameter as sensitive to make sure that the data is not leaked in the logs or outputs.

### How can I protect the access to Workspaces and its data?
{: #bp-security-data}

As the account owner or an authorized account administrator, you can assign {{site.data.keyword.iamlong}} (IAM) service access roles to your users. The IAM service access roles determine the actions that you can perform on an {{site.data.keyword.bplong_notm}} resource, such as a Workspace or an Action. To avoid assigning access policies to individual users, consider creating [IAM access groups](/docs/account?topic=account-groups).

Your Workspaces and Actions data store depends on the location where you create your Workspace or an Action. For more information, see [securing your data in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-secure-data).

### How does {{site.data.keyword.bpshort}} protect my Workspace data through Terraform state file, or log files, etc?
{: #bp-protect-data}

Follow these practices to protect your Workspace data through Terraform state file, or log files.
- Use {{site.data.keyword.iamshort}} to control access to a {{site.data.keyword.bpshort}} Workspaces and related {{site.data.keyword.cloud_notm}} resources.
- Secure the source repository for your Terraform template, including access control, security settings, collaboration, and version control.
- Secure the {{site.data.keyword.cloud_notm}} resources that you create by using the security features that are provided by the resource offering.
- Use the provided tools of your {{site.data.keyword.cloud_notm}} resources to apply security patches, access controls, and encryption to your resources.

You need to specify the right [roles and permissions](/docs/schematics?topic=schematics-access#access-setup) to an user who controls the state file. For more information, about {{site.data.keyword.bpshort}} service access roles and required permissions for 
- [Workspace Permissions](/docs/schematics?topic=schematics-access#workspace-permissions) 
- [KMS permissions](/docs/schematics?topic=schematics-access#kms-permissions)

## Best practices of managing {{site.data.keyword.bpshort}} Actions 
{: #bp-actions}

### What are the best practices that I must follow while creating an Action for the Ansible template?
{: #bp-security-ansible}

Follow these practices while creating an {{site.data.keyword.bpshort}} Actions for the Ansible template.
- Check whether the `location` and the `url` endpoint are pointing to the same region when you create or update the {{site.data.keyword.bpshort}} workspace. For more information, about location and endpoint, see [Where is my information stored?](/docs/schematics?topic=schematics-secure-data#pi-location).
- You cannot delete or stop a running job of your {{site.data.keyword.bpshort}} action. To make changes to your action, wait for the job to complete, then change your settings, and click **Check action** or **Run action** again.
- As the account owner or an authorized account administrator, you can assign IAM service access roles to your users. The IAM service access roles determine the actions that you can perform on an {{site.data.keyword.bplong_notm}} resource, such as a Workspace or an Action. To avoid assigning access policies to individual users, consider creating [IAM access groups](/docs/account?topic=account-groups).

### How can I protect the access to Actions and its data?
{: #bp-security-data}

As the account owner or an authorized account administrator, you can assign IAM service access roles to your users. The IAM service access roles determine the actions that you can perform on an {{site.data.keyword.bplong_notm}} resource, such as a workspace or an action. To avoid assigning access policies to individual users, consider creating [IAM access groups](/docs/account?topic=account-groups).

Your Workspaces and Actions data store depends on the location where you create your Workspace or an Action. For more information, see [securing your data in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-secure-data).

### How does {{site.data.keyword.bpshort}} protect my Action data through input credentials state file, or log files, etc?
{: #bp-security-protect}

Follow these practices to protect your Workspace data through input credentials state file, or log files, etc.
- Use Cloud Identity and Access Management to control access to a {{site.data.keyword.bpshort}} Actions.
- Secure the source repository of your Terraform template, including access control, security settings, collaboration, and version control.
- Use the provided tools of your {{site.data.keyword.cloud_notm}} resources to apply security patches, access controls, and encryption to your resources.
- You need to specify the right [roles and permissions](/docs/schematics?topic=schematics-access#access-setup) to an user who controls the state file. For more information, about {{site.data.keyword.bpshort}} service access roles and required permissions for 
- [Action permissions](/docs/schematics?topic=schematics-access#action-permissions)
- [KMS permissions](/docs/schematics?topic=schematics-access#kms-permissions)

## Protecting data of {{site.data.keyword.bpshort}}
{: #bp-protect}

Following are the various ways that {{site.data.keyword.bpshort}} data can be protected 
- Access protection by using {{site.data.keyword.iamshort}}
- Non-repudiation by using {{site.data.keyword.cloudaccesstrailshort}}
- Data protection by using Key Management Systems (KMS)

### Access protection by using Identity and Access Management
{: #bp-security-iam}

Create an IAM access group for your users and assign service access policies to {{site.data.keyword.bplong_notm}} and the resources that you want your users to work with. IAM users are attached to access groups, for more information, refer to, [Setting up access for your users](/docs/schematics?topic=schematics-access#access-setup)

### Non-repudiation by using Activity tracker
{: #bp-security-atracker}

You can use IBM Cloud® Activity Tracker to track and audit how users and applications interact with {{site.data.keyword.bplong_notm}}. You can generate and maintain an audit trail for a {{site.data.keyword.bpshort}} workspace instance events, access, events, and access audit log. For more information, refer to, [Auditing events](/docs/schematics?topic=schematics-at_events).

### Data protection by using KMS
{: #bp-security-data-protection}

You can safeguard and encrypt your information from corruption, compromise, or loss in {{site.data.keyword.bpshort}} by:
- [KMS integration for BYOK or KYOK](/docs/schematics?topic=schematics-kms-integration&interface=ui)
- [Managing data encryption](/docs/schematics?topic=schematics-secure-data#pi-encrypt)
- Restrict network access for all the resources to provision by [Allowing specific IP addresses](/docs/schematics?topic=schematics-allowed-ipaddresses&interface=ui)

## Next steps
{: #bp-security-next-steps}

Check out the [{{site.data.keyword.bpshort}} use cases](/docs/schematics?topic=schematics-how-it-works).
