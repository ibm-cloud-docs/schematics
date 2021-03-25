---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-25"

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


Ansible playbook is a set of instructions that you can configure to run on a single target or groups of target hosts. It includes tasks, roles, policies, or steps to deploy your resources in the target hosts. You can run your {{site.data.keyword.cloud}} resources in the order in which you want to execute them. For example, you can include instructions for installing more software on a virtual server, or specify resource operations, such as reloading or taking down a virtual server instance. Ansible playbooks must be stored in a GitHub or GitLab repository so that, you can run them in {{site.data.keyword.bpshort}}. For more information, to write your playbook by using Ansible, refer to [Writing your playbook](https://www.ansible.com/blog/getting-started-writing-your-first-playbook){: external}.
{: shortdesc}

## Planning your Ansible playbook
{: #plan-ansible-playbook}

In order to create a playbook, you need to list the following requirement.
- The target or group of target hosts with the IP address to run through the Ansible playbook. 
- Plan the tasks to provision infrastructure, deploy applications you want to perform regularly through roles. For more information, about roles and its usage, see [referencing {{site.data.keyword.bpshort}} roles in playbook](#schematics-roles).
- Plan for a comprehensive package of automation by using multiple playbooks, roles through collections. For more information, about roles and its usage, see [referencing {{site.data.keyword.bpshort}} collections in playbook](#schematics-collections).

## Creating your Ansible playbook
{: #create-playbook}

The steps to create an Ansible playbook for {{site.data.keyword.cloud_notm}}.

1. Create a YAML file that contains all the target host, roles, tasks, files, policies, steps you want to configure and deploy your resources. For more information, about YAML syntax, refer to [YAML Syntax](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html){: external}.  Refer to [a sample YAML file](https://github.com/Cloud-Schematics/ansible-app-deploy-iks/blob/master/site.yml){: external} that describes how to use Ansible playbook to deploy the Hackathon starter web application.

   You can configure the tasks in `requirements.yaml` file and store in the [roles](#schematics-roles) folder for your playbook execution. You can also configure all of your automation and autoscaling topologies in the `requirements.yaml` file and store in the [collections](#schematics-collections) folder for your playbook execution.
   {: note}

2. Store the YAML files in your Git repository. For more information, about the location to store the YAML files, refer to [a file structure](https://github.com/Cloud-Schematics/ansible-app-deploy-iks){: external} that describes the how to structure the directories and files to deploy the application.

3. Create an IAM access token for your {{site.data.keyword.cloud_notm}} Account. To create IAM access token, use `export IBMCLOUD_API_KEY=<ibmcloud_api_key>` from the command line to set up the enviroment variable. For more information, to create an {{site.data.keyword.cloud_notm}} API Key, refer to [create API key](/docs/account?topic=account-userapikey#create_user_key) by using UI. For more information, about creating IAM access token, refer to [IAM access token](/apidocs/iam-identity-token-api#gettoken-apikey-delegatedrefreshtoken). You can set the environment variables by exporting the access token. Command to export access_token and refresh_token through command line are:
  ```
  export ACCESS_TOKEN=<access_token>
  export REFRESH_TOKEN=<refresh_token>
  ```
  {: pre}
4. Create {{site.data.keyword.bplong_notm}} workspace action for your Git repository. The workspace action creates the environment for the Ansible playbook. For more information, see [workspace action creation by using command line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-action) or [workspace action creation by using user interface](/docs/schematics?topic=schematics-workspace-setup#create-workspace).

### What's next?
{: #what's-next-create} 

Create an action and run your Ansible playbook with an action.

## Running Ansible playbooks for {{site.data.keyword.cloud_notm}}
{: #run-ansible-playbook}

In order to run your playbook, you need to create {{site.data.keyword.bpshort}} action, and within the action you can run the playbook. {{site.data.keyword.bpshort}} action reads the playbook and executes the configured task and targeted hosts by using {{site.data.keyword.bplong_notm}}. 
{: shortdesc}

Follow these steps to run the Ansible playbook in {{site.data.keyword.bplong_notm}}.

1. Create a Schematics action by using [user interface](/docs/schematics?topic=schematics-action-setup#create-action) or [command line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-create-action). 

   **Syntax for the command line**
   ```
   ibmcloud schematics action create --name ACTION_NAME [--description DESCRIPTION] --location GEOGRAPHY --resource-group RESOURCE_GROUP [--template GIT_TEMPLATE_REPO] [--playbook-name PLAYBOOK_NAME] [--bastion BASTION_HOST_IP_ADDRESS] [--target-ini TARGET_HOSTS_FILE] [--credential CREDENTIAL_FILE_NAME] [--input INPUT_VARIABLES_LIST] [--input-file INPUT_VARIABLE_FILE_PATH] [--env ENV_VARIABLES_FILE_PATH] [--github-token GITHUB_ACCESS_TOKEN] [--output OUTPUT] [--file FILE_NAME] [--json] [--no-prompt]
   ```
   {: pre} 

2. You can execute the job in the Schematics action. For more information, about command line job commands, refer to [job commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-job-commands).

   **Syntax for the command line**

   ```
   ibmcloud schematics job create --command-object COMMAND_OBJECT_TYPE --command-object-id COMMAND_OBJECT_ID --command_name COMMAND_NAME --playbook-name PLAYBOOK_NAME [--command-options COMMAND_OPTIONS] [--input INPUT_VARIABLES_LIST] [--input-file INPUT_VARIABLES_FILE_PATH] [--env ENV_VARIABLES_LIST] [--env-file ENV_VARIABLES_FILE_PATH] [--result-format RESPONSE_OUTPUT_FORMAT] [--file FILE_NAME] [--no-prompt]
   ```
   {: pre}

3. Verify your progress in the logs by using user interface. For more information, about command line log commands, refer to [log commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs-job).


## Referencing {{site.data.keyword.bpshort}} roles for the playbook
{: #schematics-roles}

Ansible Galaxy is a tool to retrieve the Ansible roles from the requirements file and invoke your Ansible playbook to setup the configured resources. This is used to streamline your automation tasks, even the fresh system administrator can start automating by using Ansible.

{{site.data.keyword.bplong_notm}} supports `/roles` to specify the requirements to process in through `requirements.yaml` or `requirements.yml` file. The requirements file uses the Ansible Galaxy repository to execute the process and invokes your Ansible playbook from the Git repository to execute the configured resources.

For more information, about the sample Git repository that installs `kubectl` on virtual machine by using Ansible Galaxy role, refer to [Ansible playbook by using Ansible Galaxy](https://github.com/Cloud-Schematics/ansible-kubectl){: external}. Following is the directory structure of the Git repository supporting Ansible Galaxy.
{: shortdesc}

The roles directory can have a sub directory such as **/roles/web/** or **/roles/db/** describing the tasks in the 'main.yaml' file.
{: note}

```
   ├── kubectl.yaml
   ├── README.md
   ├── roles
      └── requirements.yaml or requirements.yml
```
{: screen}

**Sample**

The sample `requirements.yaml` file for installing `kubectl` on virtual machine from Ansible Galaxy role `andrewrothstein.kubectl`. 

```
---
roles:
  - name: andrewrothstein.kubectl
```
{: pre}

The sample `kubectl.yaml` playbook to invoke the role from your Git repository. 

```
---
- hosts: all
  roles:
    - role: andrewrothstein.kubectl
```
{: pre}


## Referencing {{site.data.keyword.bpshort}} collections for the playbook
{: #schematics-collections}

Ansible collections includes playbooks, roles, modules, and plug-ins. As modules move from the core Ansible repository into collections, the module documentation move the collections pages. For more information, about Ansible collections, refer to [Collection support](https://docs.ansible.com/ansible-tower/latest/html/userguide/projects.html#collections-support){: external}.

{{site.data.keyword.bplong_notm}} supports Ansible collections by processing the requirements through `requirements.yaml` or `requirements.yml` file. The `requirements.yaml` file is stored in the `/collections` folder of your Git repository. It’s designed to be the comprehensive package for all of your automation by using one or more playbooks, and roles. It logs all your jobs, and allows REST API calls.  Provisioning callbacks provide great support for autoscaling topologies. Collections are stored in the Ansible root folder as `ansiblecollections`.
{: shortdesc}

The sample folder structure with the collections structure in the Git repository is:

```
   ├── site.yaml
   |——- roles
      └── requirements.yaml 
   ├── collections    
      └── requirements.yaml or requirements.yml       
   |── tasks                
   |── main.yml
```
{: screen}
