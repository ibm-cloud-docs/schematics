---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-18"

keywords: ansible playbook, ansible playbook example, lamp stack, VSI by using Ansible,

subcollection: schematics

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:beta: .beta}
{:table: .aria-labeledby="caption"} 
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}

---

# Provisioning LAMP stack by using {{site.data.keyword.bpshort}} action
{: #provisioning-lamp-stack}

   The open beta release of Ansible support is now available in {{site.data.keyword.bplong_notm}} to IBM users. Contact your IBM Cloud Schematics Technical Offering Manager [Sai Vennam](mailto:svennam@us.ibm.com), if you are interested in getting early access to this beta offering. For more information, see [Beta limitations](/docs/schematics?topic=schematics-schematics-limitations#beta-limitations).
   {: beta}

 This playbook demonstrates to provision Multitier VPC Bastion host on {{site.data.keyword.cloud_notm}}. By provisioning, you can deploy a simple web application that display the text messages, links with a list of database names.  

These playbooks are tested on CentOS 7.x. It is recommended you use `CentOS` or `RHEL` to test these modules. 
{: note}

## Pre-requisite
{: #lamp-stack-prereq}

You can execute the use case by using command line or user interface by completing the provided pre-requisite.

The pre-requisite for the use case are:
* {{site.data.keyword.bplong_notm}} login.
* Roles and permissions for service access, see [Managing service access role](/docs/app-configuration?topic=app-configuration-ac-service-access-management).
* [SSH Key on {{site.data.keyword.cloud_notm}}](/docs/ssh-keys?topic=ssh-keys-adding-an-ssh-key).
* [Multitier VPC Bastion Host](https://github.com/Cloud-Schematics/multitier-vpc-bastion-host).

**What is bastion host?**

Bastion host is an VSI instance that is provisioned with a public IP address and can be accessed via SSH. Once set up, the bastion host acts as a jump server allowing secure connection to instances provisioned without a public IP address. For more information, to securely access remote instances with a bastion host, refer to [Securely access remote instances with a bastion host](/docs/solution-tutorials?topic=solution-tutorials-vpc-secure-management-bastion-server).

Schematics actions use bastion hosts to allow Ansible to securely provision software and applications on target VSIs.


## Executing the playbook by using command line
{: #lamp-stack-execute}

After the pre-requisite is completed, follow these steps to complete the use case:

1. Use the GitHub repository, [Ansible playbook for the LAMP stack components](https://github.com/Cloud-Schematics/lamp-simple), and view the `YAML` file. For more information, about playbook creation refer to [create playbook](/docs/schematics?topic=schematics-create-playbooks). 

2. Create a {{site.data.keyword.bpshort}} action by using the `playbook` and `YAML` file. The example contains the command to create an action. For more information, see [create {{site.data.keyword.bplong_notm}} action by using UI](/docs/schematics?topic=schematics-action-setup#create-action). And see [create {{site.data.keyword.bplong_notm}} action by using CLI](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-create-action)

  **Example**

  ```
  ibmcloud schematics action create -n <action-name> -r Default -l us_east --bastion <bastion-ip> --bastion-credential <ssh-key-file-path> --target <target-webserver-ip-address> --target-credential <ssh-key-path> --tr https://github.com/Cloud-Schematics/lamp-simple -g <github-token> --pn site.yml --input upassword=Abc@123abc --input dbname=foodb --input dbuser=newUser --input mysql_port=3306 --input httpd_port=80 --json
  ```
  {: pre}

3. Run a {{site.data.keyword.bpshort}} job by using the action ID that is created during action creation. The example contains the command to run a job. For more information, refer to [run job](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-create-job).

 **Example**

  ```
 ibmcloud schematics job create -c action --cid <action-id> -n ansible_playbook_run --json
  ```
  {: pre}

4. You can run `curl http://<PUBLIC_IP/index.php` after logging into the machine where you have deployed LAMP. You can see a simple test page and a list of databases retrieved from the database server.


## Executing the playbook by using user interface
{: #lamp-stack-executeui}

After the pre-requisite is completed, follow these steps to complete the use case:

1. Use the GitHub repository, [Ansible playbook for the LAMP stack components](https://github.com/Cloud-Schematics/lamp-simple), and view the `YAML` file. For more information, about playbook creation refer to [create playbook](/docs/schematics?topic=schematics-create-playbooks).

2. Ensure the {{site.data.keyword.containerlong_notm}} instance is in running state. For more information,  about creating VPC cluster, see [Creating a VPC Generation 2 compute cluster](/docs/containers?topic=containers-getting-started#vpc-gen2-gs).

3. Create a {{site.data.keyword.bpshort}} action file `action.json` by using the action definition. You need to specify the {{site.data.keyword.cloud_notm}} resource inventory, Bastion host public IP, {{site.data.keyword.cloud_notm}} inventory host groups, and SSH key. For more information, about the steps to create the action definition, see [create {{site.data.keyword.bplong_notm}} action by using UI](/docs/schematics?topic=schematics-action-setup#create-action).

   When the action is successful the job is created, you can view the settings and job option to view the  details of the execution.
  {: note}

4. Access your VPC public IP to view the deployed application. For example, `https://<PUBLIC_IP>/index.php`.

## What's next?
{: #uc_what's next}

Great job! You successfully deployed LAMP stack by using {{site.data.keyword.bplong_notm}} actions. You can now learn how to [secure VPC access with a bastion host and Terraform](https://developer.ibm.com/articles/secure-vpc-access-with-a-bastion-host-and-terraform/).

