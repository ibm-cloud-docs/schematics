---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-12"

keywords: ansible playbook, ansible playbook example, iks cluster with ansible playbook, iks cluster example using ansible playbook

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

---

# Automating the application deployment by using {{site.data.keyword.bpshort}} action
{: #iks-cluster}

This playbook demonstrates how you can deploy to the Hackathon starter application, a boilerplate for `Node.js` with a `Mongo` database web application to an {{site.data.keyword.containerlong_notm}} cluster by using {{site.data.keyword.bplong_notm}} actions command line commands and UI. For more information, about Hackathon starter application, see [Hackathon starter](https://github.com/sahat/hackathon-starter/blob/master/README.md).
{: shortdesc}
 
## Pre-requisites
{: #iks-prereq}

The pre-requisite for the use case are:

* {{site.data.keyword.bplong_notm}} login.
* Roles and permissions for service access, see [Managing service access role](/docs/app-configuration?topic=app-configuration-ac-service-access-management).
* Running state of a {{site.data.keyword.containerlong_notm}} instance. For more information, see [Getting started with {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started). Create a cluster configuration provisioned with classic infrastructure, opting for Single zone availability and enabling single worker node with single zone. For any support that you need, reach out the [Getting help and support](/docs/schematics?topic=schematics-schematics-help).
* Schematics plug-in installed. For more information, refer to [installing {{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli). 


## Executing the playbook by using command line
{: #iks-execute}

Follow the steps to achieve the use case:

1. Use the GitHub repository, [Hackathon starter Ansible playbook](https://github.com/Cloud-Schematics/ansible-app-deploy-iks), and view the `site.yaml` file. For more information, about playbook creation refer to [create playbook](/docs/schematics?topic=schematics-create-playbooks). 

2. Create a {{site.data.keyword.bpshort}} action file `action.json` by using the action definition. The example contains the command to create an action. For more information, see [create {{site.data.keyword.bplong_notm}} action by using CLI](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-create-action).

  **Example:**

    ```
    ibmcloud schematics action create -f action.json
    ```
    {: pre}

 **action.json file:**

    ```
    {
      "name": "Hackathon_Starter_Action",
      "description": "This Action will deploy boiler plate code for Hackathon Starter",
      "location": "us-east",
      "resource_group": "Default",
      "source": {
          "source_type" : "git",
          "git" : {
                "git_repo_url": "https://github.com/Cloud-Schematics/ansible-app-deploy-iks.git"
          }
      },
      "command_parameter": "site.yml",
      "tags": [
        "string"
      ],
      "inputs": [
        {
          "name": "cluster_id",
          "value": <Your-Cluster-ID>,
          "metadata": {
            "type": "string",
            "default_value": <Your-Default-Cluster-ID>
          }
        }
      ],
      "source_type": "GitHub" 
    }
    ```
     {: codeblock}

3. Create a job file `job.json` to run the action and following with adding ‘job’ definition to it. Edit the ‘Action ID’ from step 1 and run job.json.

 **Example:**
  
    ```
    ibmcloud schematics job create -f job.json
  
    ```
     {: pre}

  **job.json file:**

    ```
    {
     "command_object": "action",
     "command_object_id": <Action-ID>,
     "command_name": "ansible_playbook_run"
    }
    ```
     {: codeblock}

4. To verify job created successfully run the following command

    ```
      IBMcloud schematics job logs --id <job id>
    ```
     {: pre}  

5. Need to run below kubectl commands in CLI to check the status of nodes, running pods and finally run svc to view external IP address of your service.

    ```
      kubectl get nodes -o wide
      kubectl get pods -o wide
      kubectl get svc
    ```
     {: pre}

 **Example:** 
    
    ```
      my-MacBook-Pro bin % kubectl get svc 
      NAME             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)        AGE
      kubernetes       ClusterIP      172.21.0.1       <none>           443/TCP        14h
      mongo            ClusterIP      172.21.182.239   <none>           27017/TCP      57m
      webapp-service   LoadBalancer   172.21.117.114   169.47.109.178   80:30170/TCP   57m
    ```
     {: pre}
  
6. Now, you can use public IP address and access the URL `https://<public-IPaddress>:30170/` to view the deployed application.


### Executing playbook by using user interface
{: #iks-ui-execute}

Follow the steps to complete the use case:

1. Use the GitHub repository, [Hackathon starter Ansible playbook](https://github.com/Cloud-Schematics/ansible-app-deploy-iks), and view the `site.yaml` file. For more information, about playbook creation refer to [create playbook](/docs/schematics?topic=schematics-create-playbooks). 

2. Ensure the {{site.data.keyword.containerlong_notm}} instance is in running state. For more information,  about creating VPC cluster, see [Creating a VPC Generation 2 compute cluster](/docs/containers?topic=containers-getting-started#vpc-gen2-gs).

3. Create a {{site.data.keyword.bpshort}} action file `action.json` by using the action definition. You need to specify the {{site.data.keyword.cloud_notm}} resource inventory. For more information, about the steps to create the action definition, see [create {{site.data.keyword.bplong_notm}} action by using UI](/docs/schematics?topic=schematics-action-setup#create-action).

   When the action is successful the job is created, you can view the settings and job option to view the  details of the execution.
  {: note}

4. Access your VPC public IP to view the deployed application. For example, `https://<public-IPaddress>:30170/`. 


## What's next?
{: #uc_what's next}

Great job! You successfully deployed into Hackathon starter by using {{site.data.keyword.bplong_notm}} actions. You can now learn how to [secure VPC access with a bastion host and Terraform](https://developer.ibm.com/articles/secure-vpc-access-with-a-bastion-host-and-terraform/)?

