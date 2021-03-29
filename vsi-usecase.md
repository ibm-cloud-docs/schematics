---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-29"

keywords: ansible playbook, ansible playbook example, vsi start stop, reboot vsi on {{site.data.keyword.cloud_notm}}

subcollection: schematics

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"} 
{:codeblock: .codeblock}
{:tip: .tip}
{:beta: .beta}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}


# Deploying VSI starts and stops playbook by using {{site.data.keyword.bpshort}}
{: #vsi-start-stop}


This playbook demonstrates how {{site.data.keyword.cloud}} VSI APIs can be used to start, stop, and reboot by using Ansible playbook.

## Prerequisite
{: #vsi-prereq}

Before you can use execute the use case, you must complete the following tasks:

- Create a VPC for Generation 2 compute infrastructure and a virtual server instance in that VPC. For more information, about VSI instance, refer to [Getting started with VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console). Note the private or public IP address of your virtual server instance. 
- Make sure that you have the permissions to [create a {{site.data.keyword.bpshort}} action](/docs/schematics?topic=schematics-access#access-roles). 
- Schematics plug-in installed, for more information, refer to [installing {{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli).
- IAM token with access to the instance. The IAM token is optional when the action is running in the same account as of VSI.
- The bearer token for authenticating. You can create the bearer token from command line, for more information, about access and authentication, refer to [access token](/docs/key-protect?topic=key-protect-retrieve-access-token).

## Executing the a VPC Gen2 Virtual Server playbook by using user interface
{: #ansible-vsi-usecase}

1. From the [{{site.data.keyword.bpshort}} actions](https://cloud.ibm.com/schematics/actions){: external} page, click **Create action**. 
2. Enter a name for your action, resource group, and the region where you want to create the action. Then, click **Create**. 
3. In the **Define your Action action** section, enter `https://github.com/Cloud-Schematics/ansible-is-instance-actions` in the **GitHub or GitLab repository URL** field. 
4. Click **Retrieve playbooks**. 
5. Select the **stop-vsi-playbook.yaml** playbook.
6. Expand the **Advanced options**. 
7. In the **Define your variables** section, enter `intance_ip` as the **key** and the public or private IP address of your VPC Gen2 virtual server as the **value**. 
8. Click **Next**. 
9. Click **Check action** to verify your action details. The **Jobs** page opens automatically and you can view the results of this check by looking at the logs. 
10. Click **Run action** to stop the virtual server instance. You can monitor the progress of this action by reviewing the logs on the **Jobs** page. 
11. Verify that your virtual server instance stopped. 
    1. From the [Virtual server instances for VPC dashboard](https://cloud.ibm.com/vpc-ext/compute/vs){: external}, find your virtual server instance. 
    2. Verify that your instance shows a `Stopped` status. 
12. Optional: Repeat the steps in this getting started tutorial and select the **start-vsi-playbook.yaml** Ansible playbook to start your virtual server instance again. 

Congratulations! You used the built-in Ansible capabilities of {{site.data.keyword.bpshort}} to start and stop a VPC Gen2 virtual server instance. 

## Executing the playbook by using command line
{: #vsi-execute}

Now, you are ready to complete these steps to execute the use case: 

1. Use the GitHub repository, [Ansible playbook with the VSI-related configuration to start or stop VSI](https://github.com/Cloud-Schematics/ansible-is-instance-actions), and view the `YAML` file, for more information, about playbook creation, see  [create playbook](/docs/schematics?topic=schematics-create-playbooks). 

2. Create a {{site.data.keyword.bpshort}} action by using the `playbook` and `YAML` file. The example contains the command to create an action. see [create {{site.data.keyword.bplong_notm}} action by using UI](/docs/schematics?topic=schematics-action-setup#create-action). And see [create {{site.data.keyword.bplong_notm}} action by using command line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-create-action).

  **Example**

  ```
  ibmcloud schematics action create -n <action-name> -r Default -l us-east --tr https://github.com/Cloud-Schematics/ansible-is-instance-actions --pn <playbook-name> --input instance_ip=<ip-appdress> --input bearer_token="<bearer-token>" --json
  ```
  {: pre}
 
   You can provide an optional [GitHub token](https://github.com/settings/tokens){: external}.
   {: note}

3. Run a {{site.data.keyword.bpshort}} job by using the action ID that is created during action creation. The example contains the command to run a job. For more information, refer to [run Ansible playbook](/docs/schematics?topic=schematics-create-playbooks#run-ansible-playbook).

  **Example**

  ```
  ibmcloud schematics job create -c action --cid <job-payload> -n ansible_playbook_run --json
  ```
  {: pre}

4. You can view the job and actions list by executing the [ibmcloud schematics job list](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-list-job) command. 

   **Job list Example**

    ```
    ibmcloud schematics job list
    ```
    You are prompted to `Enter <resource-type>` and `Enter <id>`. Provide the resource type as `actions` and enter your job_ID.

   **Action list Example**

    ```
    ibmcloud schematics action list
    ```

    You can also view these jobs in an [{{site.data.keyword.cloud_notm}} user interface](https://cloud.ibm.com/schematics/actions).


## What's next? 
{: ansible-whats-next}

Now that you ran your operation on an {{site.data.keyword.cloud_notm}} resource, you can explore the following options:
- Learn how to [auto deploy the playbook templates](docs/schematics?topic=schematics-sample_actiontemplates) to create action.
- Explore how to [create auto deploy templates](/docs/schematics?topic=schematics-auto-deploy-url) to create a {{site.data.keyword.bpshort}} 
- You can now learn how to [Provision LAMP stack by using {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-provisioning-lamp-stack)
