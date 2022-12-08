---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-07"

keywords: schematics faqs, infrastructure as code, iac, schematics actions faq, action faq,

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# Actions
{: #actions-faq}

Answers to common questions about the {{site.data.keyword.bplong_notm}} Actions are classified into following section.
{: shortdesc}

## Why is my success action job execution displays DEPRECATION WARNING message?
{: #deprecation-warn-faq}
{: faq}
{: support}

In the action settings page you, need to set the input variable as `ansible_python_interpreter = auto` as shown in the screen capture to avoid `DEPRECATION WARNING` message.

![Configuring input variable to silence warning message](../images/advanced_inputvariable.png "Embedded {{site.data.keyword.bplong_notm}} service flow"){: caption="Configuring input variable to silence warning message" caption-side="bottom"}

## How can I resolve that might not run action error while provisioning `WinRM` by using {{site.data.keyword.bpshort}} action?
{: #winrm-faq}
{: faq}
{: support}

 ```text
 Error: 2021/12/06 10:15:49 Terraform apply | Error: Error running command 'ANSIBLE_FORCE_COLOR=true ansible-playbook ansible.yml --inventory-file='inventory.yml' --extra-vars='{"ansible_connection":"winrm","ansible_password":"password","ansible_user":"administrator","ansible_winrm_server_cert_validation":"ignore"}' --forks=15 --user='root' --ssh-extra-args='-p 22 -o ConnectTimeout=120 -o ConnectionAttempts=3 -o StrictHostKeyChecking=no'': exit status 2. Output:
  2021/12/06 10:15:49 Terraform apply | PLAY [Please wait and have a coffee! The show is about to begin....] ***********
  2021/12/06 10:15:49 Terraform apply |
  2021/12/06 10:15:49 Terraform apply | TASK [Gathering Facts] *********************************************************
  2021/12/06 10:15:49 Terraform apply | fatal: [161.156.161.7]: FAILED! => {"msg": "winrm or requests is not installed: No module named 'winrm'"}
  2021/12/06 10:15:49 Terraform apply |
  2021/12/06 10:15:49 Terraform apply | PLAY RECAP *********************************************************************
  2021/12/06 10:15:49 Terraform apply | 161.156.161.7              : ok=0    changed=0    unreachable=0    failed=1    skipped=0    rescued=0    ignored=0
  2021/12/06 10:15:49 Terraform apply |
  2021/12/06 10:15:49 Terraform apply |
  2021/12/06 10:15:49 Terraform apply |
  2021/12/06 10:15:49 Terraform apply |   with null_resource.schematics_for_windows,
  2021/12/06 10:15:49 Terraform apply |   on schematics.tf line 2, in resource "null_resource" "schematics_for_windows":
  2021/12/06 10:15:49 Terraform apply |    2:   provisioner "ansible" {
  2021/12/06 10:15:49 Terraform apply |
  2021/12/06 10:15:50 Terraform APPLY error: Terraform APPLY errorexit status 1
  2021/12/06 10:15:50 Could not execute action
```
{: screen}

WinRM is not supported by {{site.data.keyword.bpshort}} Terraform Ansible provisioner. Alternatively you can use the {{site.data.keyword.bpshort}} Actions to run the Ansible playbooks with WinRM. The {{site.data.keyword.bpshort}} Actions support [WinRM](/docs/schematics?topic=schematics-action-working).

## When are the new Terraform and Ansible versions added to {{site.data.keyword.bpshort}}?
{: #new-versions}
{: faq}
{: support}

After new Terraform and Ansible versions are released by the community, the IBM team begins hardening and testing the release for {{site.data.keyword.bpshort}}. Availability of new versions depends on the results of these tests, community updates, security patches, and technology changes between versions. Make sure that your Terraform templates and Ansible playbooks are compatible with one of the supported versions so that you can run them in {{site.data.keyword.bpshort}}. For more information, see [Upgrading the Terraform template version](/docs/schematics?topic=schematics-migrating-terraform-version) and [{{site.data.keyword.bpshort}} runtime tools](/docs/schematics?topic=schematics-sch-utilities#terraform-runtime-agent-job).

## Can I run Ansible playbooks with {{site.data.keyword.bpshort}}?
{: #ansible-playbooks}
{: faq}
{: support}

Yes, you can run Ansible playbooks against your {{site.data.keyword.cloud_notm}} by using the {{site.data.keyword.bpshort}} Actions or Ansible provisioner in your Terraform configuration file. For example, use the Ansible provisioner to deploy software on {{site.data.keyword.cloud_notm}} resources or set actions against your resources, such as shutting down a virtual server instance. For more information, see [sample Ansible playbook templates for {{site.data.keyword.bpshort}} Actions](/docs/schematics?topic=schematics-sample_actiontemplates).

