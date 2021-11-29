---

copyright:
  years: 2017, 2021
lastupdated: "2021-11-27"

keywords: ansible playbook, ansible playbook example, iks cluster with ansible playbook, iks cluster example by using ansible playbook

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Automating the application deployment by using {{site.data.keyword.bpshort}}
{: #iks-cluster}

The playbook demonstrates how to automate the deployment of a hackathon starter web application. Hackathon starter is a boilerplate web application for `Node.js` and `Mongo` database to an {{site.data.keyword.containerlong_notm}} cluster. For more information, about hackathon starter application development, refer to [Hackathon starter readme document](https://github.com/sahat/hackathon-starter/blob/master/README.md).
{: shortdesc}

## Prerequisites
{: #iks-prereq}

You can execute the use case by using command-line or user interface by completing these prerequisite.

The prerequisites for the use case are:

* {{site.data.keyword.bplong_notm}} login.
* Roles and permissions for service access, see [Managing service access role](/docs/app-configuration?topic=app-configuration-ac-service-access-management#ac-roles-permissions).
* Running state of an {{site.data.keyword.containerlong_notm}} instance. For more information, see [Getting started with {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-getting-started). Create a cluster configuration provisioned with classic infrastructure, opting for Single zone availability and enabling single worker node with single zone. For any support that you need, reach out the [Getting help and support](/docs/schematics?topic=schematics-schematics-help).
* Schematics plug-in installed, for more information, refer to [installing {{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-cli). 

## Executing playbook by using user interface
{: #iks-ui-execute}

After the prerequisite is completed, follow these steps to complete the use case:

1. Use the GitHub repository, [hackathon starter Ansible playbook](https://github.com/Cloud-Schematics/ansible-app-deploy-iks), and view the `site.yaml` file, for more information, about playbook creation, see  [create playbook](/docs/schematics?topic=schematics-create-playbook).

2. Ensure the {{site.data.keyword.containerlong_notm}} instance is in running state. For more information, about creating VPC cluster, see [Creating a VPC Generation 2 compute cluster](/docs/containers?topic=containers-getting-started#vpc-gen2-gs).

3. Create a {{site.data.keyword.bpshort}} action file `action.json` by using the action definition. You need to specify the {{site.data.keyword.cloud}} resource inventory. For more information, about the steps to create the action definition, see [create {{site.data.keyword.bplong_notm}} action by using UI](/docs/schematics?topic=schematics-action-setup#create-action).

    When the action is successful the job is created. You can view the settings and job option of the execution details.
    {: note}

4. Access your VPC public IP to view the deployed application. For example, `https://<public-IPaddress>:30170/`. 

## Executing the playbook by using command-line
{: #iks-execute}

Now, you are ready to complete these steps to execute the use case:

1. Use the GitHub repository, [hackathon starter Ansible playbook](https://github.com/Cloud-Schematics/ansible-app-deploy-iks), and view the `site.yaml` file, for more information, about playbook creation, see  [create playbook](/docs/schematics?topic=schematics-create-playbook). 

2. Create a {{site.data.keyword.bpshort}} action file `action.json` by using the action definition. The example contains the command to create an action. For more information, see [create {{site.data.keyword.bplong_notm}} action by using command-line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-create-action).

    **Example**

    ```sh
    ibmcloud schematics action create -f action.json
    ```
    {: pre}

    **action.json file**

    ```json
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

3. Create a job file `job.json` to run the action and following with adding `job` definition to it. Edit the `Action ID` from step 1 and run job.json.

    **Example**

    ```sh
    ibmcloud schematics job run -f job.json

    ```
    {: pre}

    **job.json file**

    ```json
    {
        "command_object": "action",
        "command_object_id": <Action-ID>,
        "command_name": "ansible_playbook_run"
    }
    ```
    {: codeblock}

4. To verify the job is created successfully run the following command. For more information, about viewing job queue logs, see [Reviewing the {{site.data.keyword.bpshort}} job details](/docs/schematics?topic=schematics-workspace-setup&interface=ui#job-logs).

    ```sh
    ibmcloud schematics job logs --id <job id>
    ```
    {: pre}  

5. Execute the `kubectl` commands in command-line to check the status of the nodes, running pods, and finally `run svc` command to view the external IP address of your service.

    ```sh
    kubectl get nodes -o wide
    kubectl get pods -o wide
    kubectl get svc
    ```
    {: pre}

    **Example** 

    ```text
    my-MacBook-Pro bin % kubectl get svc 
    NAME             TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)        AGE
    kubernetes       ClusterIP      172.21.0.1       <none>           443/TCP        14h
    mongo            ClusterIP      172.21.182.239   <none>           27017/TCP      57m
    webapp-service   LoadBalancer   172.21.117.114   169.47.109.178   80:30170/TCP   57m
    ```
    {: screen}

6. Now, you can use external IP address and access the URL `https://<public-IPaddress>:30170/` to view the deployed application.


## What's next?
{: #uc_what's next}

Great job! You successfully deployed into hackathon starter by using {{site.data.keyword.bplong_notm}} actions. You can now learn how to [secure VPC access with a bastion host and Terraform](https://developer.ibm.com/articles/secure-vpc-access-with-a-bastion-host-and-terraform/)?



