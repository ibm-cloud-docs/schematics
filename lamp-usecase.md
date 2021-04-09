---

copyright:
  years: 2017, 2021
lastupdated: "2021-04-09"

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


# Provisioning LAMP stack by using {{site.data.keyword.bpshort}} 
{: #provisioning-lamp-stack}


 This playbook demonstrates to provision multitier VPC Bastion host on {{site.data.keyword.cloud_notm}}. By provisioning, you can deploy a simple web application that display the text messages, links with a list of database names.  

These playbooks are tested on CentOS 7.x. It is recommended you use `CentOS` or `RHEL` to test these modules. 
{: note}

## Prerequisite
{: #lamp-stack-prereq}

Before you can use execute the use case, you must complete the following tasks:

- [{{site.data.keyword.bplong_notm}}](https://cloud.ibm.com/schematics) login.
- Roles and permissions for service access, see [Managing service access role](/docs/app-configuration?topic=app-configuration-ac-service-access-management).
- [SSH Key on {{site.data.keyword.cloud_notm}}](/docs/ssh-keys?topic=ssh-keys-adding-an-ssh-key).
- Create a VPC for Generation 2 compute infrastructure and a virtual server instance in that VPC cluster, see [Creating a VPC Generation 2 compute cluster](/docs/containers?topic=containers-getting-started#vpc-gen2-gs)
- [multitier VPC Bastion Host](https://github.com/Cloud-Schematics/multitier-vpc-bastion-host).

**What is bastion host?**

Bastion host is a VSI instance that is provisioned with a public IP address and can be accessed through SSH. Once set up, the bastion host acts as a jump server to allow secure connection to instances provisioned without a public IP address. For more information, to securely access remote instances with a bastion host, refer to [Securely access remote instances with a bastion host](/docs/solution-tutorials?topic=solution-tutorials-vpc-secure-management-bastion-server).

Schematics actions use bastion hosts to allow Ansible to securely provision software and applications on target VSIs.

## Executing the playbook by using user interface
{: #lamp-stack-executeui}

Now, you are ready to complete these steps to execute the use case: 

1. Use the GitHub repository, [Ansible playbook for the LAMP stack components](https://github.com/Cloud-Schematics/lamp-simple), and view the `YAML` file, for more information, about playbook creation, see [create playbook](/docs/schematics?topic=schematics-create-playbooks).

2. Ensure the {{site.data.keyword.containerlong_notm}} instance is in running state. For more information,  about creating VPC cluster, see [Creating a VPC Generation 2 compute cluster](/docs/containers?topic=containers-getting-started#vpc-gen2-gs).

3. Create a {{site.data.keyword.bpshort}} action file `action.json` by using the action definition. You need to specify the {{site.data.keyword.cloud}} resource inventory, Bastion host public IP, {{site.data.keyword.cloud_notm}} inventory host groups, and SSH key. For more information, about the steps to create the action definition, see [create {{site.data.keyword.bplong_notm}} action by using UI](/docs/schematics?topic=schematics-action-setup#create-action).

   When the action is successful the job is created, you can view the settings and job option to view the  details of the execution.
  {: note}

4. Access your VPC public IP to view the deployed application. For example, `https://<PUBLIC_IP>/index.php`.

## Executing the playbook by using command line
{: #lamp-stack-execute}

Now, you are ready to complete these steps to execute the use case:

1. Use the GitHub repository, [Ansible playbook for the LAMP stack components](https://github.com/Cloud-Schematics/lamp-simple), and view the `YAML` file, for more information, about playbook creation, see  [create playbook](/docs/schematics?topic=schematics-create-playbooks). 

2. Create a {{site.data.keyword.bpshort}} action by using the `playbook` and `YAML` file. The example contains the command to create an action. For more information, see [create {{site.data.keyword.bplong_notm}} action by using UI](/docs/schematics?topic=schematics-action-setup#create-action). And see [create {{site.data.keyword.bplong_notm}} action by using command line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-create-action).

  **Example**

  ```
  ibmcloud schematics action create -n <action-name> -r Default -l us-east --bastion <bastion-ip> --bastion-credential <ssh-key-file-path> --target <target-webserver-ip-address> --target-credential <ssh-key-path> --tr https://github.com/Cloud-Schematics/lamp-simple -g <github-token> --pn site.yml --input upassword=Abc@123abc --input dbname=foodb --input dbuser=newUser --input mysql_port=3306 --input httpd_port=80 --json
  ```
  {: pre}

3. Run a {{site.data.keyword.bpshort}} job by using the action ID that is created during action creation. The example contains the command to run a job. For more information, refer to [run job](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-run-job).

 **Example**

  ```
 ibmcloud schematics job run -c action --cid <action-id> -n ansible_playbook_run --json
  ```
  {: pre}

4. You can run `curl http://<PUBLIC_IP/index.php` after logging in to the deployed machine. You can see a simple test page and a list of databases that are retrieved from the database server.

## What's next?
{: #uc_what_is_next}

Great job! You successfully deployed LAMP stack by using {{site.data.keyword.bplong_notm}} actions. You can now 
- learn how to [secure VPC access with a bastion host and Terraform](https://developer.ibm.com/articles/secure-vpc-access-with-a-bastion-host-and-terraform/).
- Learn how to [auto deploy the playbook templates](/docs/schematics?topic=schematics-sample_actiontemplates) to create action.
- Explore how to [create auto deploy templates](/docs/schematics?topic=schematics-auto-deploy-url) to create a {{site.data.keyword.bpshort}} 


