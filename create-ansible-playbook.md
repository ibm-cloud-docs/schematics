---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-18"

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

Ansible playbook is a set of instructions that you can configure to run on a single target or groups of target hosts. It includes tasks, roles, policies, or steps to deploy your resources in the target hosts. You can run your {{site.data.keyword.cloud_notm}} resources in the order in which you want to execute them. For example, you can include instructions for installing more software on a virtual server, or specify resource operations, such as reloading or taking down a virtual server instance. Ansible playbooks must be stored in a GitHub or GitLab repository so that, you can run them in {{site.data.keyword.bpshort}}. For more information, to write you playbook using Ansible, refer to [Writing your playbook](https://www.ansible.com/blog/getting-started-writing-your-first-playbook){: external}.
{: shortdesc}

The steps to create an Ansible playbook for {{site.data.keyword.cloud_notm}}.

1. Create an YAML file that contains the target host, variables, roles, tasks, files, playbook directories, services, and configure. Store the YAML file in the GitHub repository. For more information, about YAML syntax, refer to [YAML Syntax](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html){: external}.  Refer to [a sample yaml file](https://github.com/Cloud-Schematics/ansible-is-instance-actions){: external} that describes how to run the {{site.data.keyword.cloud_notm}} VSI API to run VSI actions.

2. Download the required services from the GitHub repository that you need to configure. For example, [IKS configured template](https://github.com/ibm-cloud-architecture/iks_vpc_lab/tree/master/03-iks_cluster){: external} from the GitHub repository.

3. Create an IAM access token for your {{site.data.keyword.cloud_notm}} Account. To create IAM access token, use `export IBMCLOUD_API_KEY=<ibmcloud_api_key>` For more information, about creating IAM access token, refer to [IAM access token](/apidocs/iam-identity-token-api#gettoken-apikey-delegatedrefreshtoken) and [Create API key](/docs/account?topic=account-userapikey#create_user_key). You can set the environment values by exporting the access token. Command to export `IC_IAM_TOKEN` is `export ACCESS_TOKEN=<access_token>`  and export `IC_IAM_REFRESH_TOKEN` is `export REFRESH_TOKEN=<refresh_token>`.

4. Create {{site.data.keyword.bplong_notm}} workspace. For more information, about workspace create, refer to [workspace setup](/docs/schematics?topic=schematics-workspace-setup).


## Running Ansible playbooks for {{site.data.keyword.cloud_notm}}
{: #run-ansible-playbook}

{{site.data.keyword.bpshort}} action reads the playbook and executes the configured task and targeted hosts by using IBM Cloud Schematics. Following steps to be followed to run the Ansible playbook in {{site.data.keyword.bplong_notm}}.

1. Create a Schematics action by using [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/schematics/overview){: external} account or through command line. 



2. You can execute the Ansible job in the Schematics action. For more information, refer to [job commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-job-commands).



3. Verify your progress in the Ansible logs by using a `get` request to the logs. For more information, refer to [log commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs-job).

For the beta release, you can run the playbook only through command line. For more information, about the command line commands, refer to [Action commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-action-commands), and [Job commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-job-commands).
{: important}

