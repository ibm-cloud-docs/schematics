---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-27"

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

You can execute the use case by using command line or user interface by completing the provided prerequisite.

* A VSI instance in a running or stopped state, for more information, about VSI instance, refer to [Getting started with VPC](/docs/vpc?topic=vpc-creating-a-vpc-using-the-ibm-cloud-console).
* VSI instance ID and instance IP. You can extract your VSI instance IP, and password from your user account.
* IAM token with access to the instance. The IAM token is optional when the action is running in the same account as of VSI.
* The bearer token for authenticating. You can create the bearer token from command line, for more information, about access and authentication, refer to [access token](/docs/key-protect?topic=key-protect-retrieve-access-token).
* Schematics plug-in installed, for more information, refer to [installing {{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli).

## Executing the playbook
{: #vsi-execute}

After the prerequisite is completed, the steps to complete the use case:

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
{: #uc_what's next}

Great job! You successfully started and stopped a VSI service by using {{site.data.keyword.bplong_notm}} actions. You can now learn how to [secure VPC access with a bastion host and Terraform](https://developer.ibm.com/articles/secure-vpc-access-with-a-bastion-host-and-terraform/)?

