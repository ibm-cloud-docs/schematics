---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-16"

keywords: schematics ansible, schematics action, create schematics actions, run ansible playbooks

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

# Creating and running Ansible playbooks for {{site.data.keyword.cloud_notm}}
{: #create-playbooks}

   The open beta release of Ansible support is now available in {{site.data.keyword.bplong_notm}} to IBM users. Contact your IBM Cloud Schematics Technical Offering Manager [Sai Vennam](mailto:svennam@us.ibm.com), if you are interested in getting early access to this beta offering. For more information, see [Beta limitations](/docs/schematics?topic=schematics-schematics-limitations#beta-limitations).
   {: beta}

Ansible playbook is a set of instructions that you can configure to run on a single target or groups of target hosts. It includes tasks, roles, policies, or steps to deploy your resources in the target hosts. You can run your {{site.data.keyword.cloud}} resources in the order in which you want to execute them. For example, you can include instructions for installing more software on a virtual server, or specify resource operations, such as reloading or taking down a virtual server instance. Ansible playbooks must be stored in a GitHub or GitLab repository so that, you can run them in {{site.data.keyword.bpshort}}. For more information, to write your playbook by using Ansible, refer to [Writing your playbook](https://www.ansible.com/blog/getting-started-writing-your-first-playbook){: external}.
{: shortdesc}

The steps to create an Ansible playbook for {{site.data.keyword.cloud_notm}}.

1. Create a YAML file that contains all the target host, roles, tasks, files, policies, steps you want to configure and deploy your resources. For more information, about YAML syntax, refer to [YAML Syntax](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html){: external}.  Refer to [a sample yaml file](https://github.com/Cloud-Schematics/ansible-app-deploy-iks/blob/master/site.yml){: external} that describes how to use Ansible playbook to deploy the Hackathon starter web application.

   You can also create `requirements.yaml` file and store in the [roles directory](/docs/schematics?topic=schematics-getting-started-ansible#ansible-galaxy) for setting the configuration for playbook execution. Then, create `requirements.yaml` file and store in the [collections directory](/docs/schematics?topic=schematics-getting-started-ansible#ansible-collections) for all of your automation and autoscaling topologies.
   {: note}

2. Store the YAML files in your Git repository. For more information, about the location to store the yaml files, refer to [a file structure](https://github.com/Cloud-Schematics/ansible-app-deploy-iks){: external} that describes the how to structure the directories and files to deploy the application.

3. Create an IAM access token for your {{site.data.keyword.cloud_notm}} Account. To create IAM access token, use `export IBMCLOUD_API_KEY=<ibmcloud_api_key>` from the command line to set up the enviroment variable. For more information, to create an {{site.data.keyword.cloud_notm}} API Key, refer to [create API key](/docs/account?topic=account-userapikey#create_user_key) by using UI. For more information, about creating IAM access token, refer to [IAM access token](/apidocs/iam-identity-token-api#gettoken-apikey-delegatedrefreshtoken). You can set the environment variables by exporting the access token. Command to export access_token and refresh_token through command line are:
 - `export ACCESS_TOKEN=<access_token>`  
 - `export REFRESH_TOKEN=<refresh_token>`

4. Create {{site.data.keyword.bplong_notm}} workspace action by using your Git repository to create the environment. For more information, about workspace creation, refer to [workspace setup](/docs/schematics?topic=schematics-workspace-setup).


## Running Ansible playbooks for {{site.data.keyword.cloud_notm}}
{: #run-ansible-playbook}

{{site.data.keyword.bpshort}} action reads the playbook and executes the configured task and targeted hosts by using IBM Cloud Schematics. Follow these steps to run the Ansible playbook in {{site.data.keyword.bplong_notm}}.

1. Create a Schematics action by using [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/schematics/overview){: external} account or through command line. 



2. You can execute the job in the Schematics action. For more information, about command line job commands, refer to [job commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-job-commands).



3. Verify your progress in the logs by using a `get` request to the logs. For more information, about command line log commands, refer to [log commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs-job).

