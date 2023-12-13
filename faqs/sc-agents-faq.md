---

copyright:
  years: 2017, 2023
lastupdated: "2023-12-13"

keywords: schematics faqs, schematics agents faq, agents faq, agents, artifactory, provider 

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# Agent
{: #faqs-agent}

Answers to common questions about the Agent for {{site.data.keyword.bplong_notm}}.
{: shortdesc}

## What are the updates in the GA agent release?
{: #faqs-agent-update}
{: faq}
{: support}

The following are the features in the agent release.
- Improvements to the agent deployment experience through CLI and UI.
- Support to run Ansible playbooks on the agent.
- Dynamic assignment of workspace or action jobs to the agent.

## What are the costs of installing and using agents?
{: #faqs-agent-cost}
{: faq}
{: support}

The following is the cost break-down for deploying and using a {{site.data.keyword.bpshort}} agent.

The prerequisite infrastructure required to deploy and run an agent is chargeable: 
- Cost of VPC infrastructure elements such as subnet, public gateways.
- Cost of IBM Kubernetes Service (cluster) on VPC, with three-node worker pool.
- Cost of IBM Cloud Object Storage

Agent service execution:
- There is no cost to running jobs on agents.  
- The {{site.data.keyword.bpshort}} version 1 agent capability is a non-chargeable feature. Future versions may be chargeable. 

## Is it possible to install more than one Agent on a cluster?
{: #faqs-agent-install}
{: faq}
{: support}

You can install only one agent on a Kubernetes cluster on {{site.data.keyword.containerlong_notm}}. Additional clusters are required to deploy additional agents. If you attempt to install more than one agent on a cluster, the deploy job will fail with a namespace conflict error.

## What Terraform versions are supported with agents? 
{: #faqs-agent-terraform-versions}
{: faq}
{: support}

Only the two most recent versions of Terraform supported by Schematics are supported with agents. At this time these are version 1.4 and version 1.5. Older versions of Terraform are not supported. Workspaces using older versions of Terraform must be updated to one of the supported versions prior to use with agents. See the instructions [Upgrading to a new Terraform version](docs/schematics?topic=schematics-migrating-terraform-version) for how to upgrade before using agents. 

## Why does workspace execution fail with `terraformx.x: executable file not found in $PATH`
{: #faqs-agent-terraform-version-old}
{: faq}
{: support}

The version of Terraform used by the workspace is not supported with agents. Only the two most recent versions of Terraform supported by Schematics are with agents. Workspaces using older versions of Terraform must be updated to one of the supported versions prior to use with agents. See the instructions [Upgrading to a new Terraform version](docs/schematics?topic=schematics-migrating-terraform-version) for how to upgrade before using agents. 


## What type of {{site.data.keyword.bpshort}} jobs can run in an agent?
{: #faqs-agent-jobs}
{: faq}
{: support}

You can run {{site.data.keyword.bpshort}} workspace Terraform jobs on an agent. You can also run {{site.data.keyword.bpshort}} action jobs, Ansible playbooks on an agent. 

## How can I see the {{site.data.keyword.bpshort}} job results and logs for the jobs running on an agent?
{: #faqs-agent-workload}
{: faq}
{: support}

The workspace job or action job logs are available in the {{site.data.keyword.bpshort}} UI console. You can also access the job logs by using the {{site.data.keyword.bpshort}} workspace API, or CLI.

## How many {{site.data.keyword.bpshort}} jobs can run in parallel on an agent?
{: #faqs-agent-parallel}
{: faq}
{: support}

Currently, an agent can run three {{site.data.keyword.bpshort}} jobs in parallel. Any additional jobs are queued and will run when prior jobs complete execution. 

In future, you will be able to customize an agent to increase the number of job pods to increase the number of jobs that can run concurrently. 

## What is the minimum cluster configuration required in Agent release?
{: #faqs-agent-min-cluster}
{: faq}
{: support}

The agent needs {{site.data.keyword.containerlong_notm}} service with a minimum three worker nodes, with a type of `b4x16` or higher.

## How many workspaces can be assigned to an agent?
{: #faqs-agent-min-wks}
{: faq}
{: support}

Currently, you can assign any number of workspaces to an agent. The workspace jobs are queued to run on the agent, based on the agent assignment policy. The agent periodically polls {{site.data.keyword.bpshort}} for jobs to run, with a polling interval of one minute. By default, the agent runs only three jobs in parallel. The remaining jobs are queued.

## How many jobs can run in parallel on an agent?
{: #faqs-agent-min-job}
{: faq}
{: support}

- {{site.data.keyword.bpshort}} Agent can perform three Git download jobs in parallel.
- {{site.data.keyword.bpshort}} Agent can run three workspace jobs (Terraform commands), in parallel.
- {{site.data.keyword.bpshort}} Agent can run three action jobs (Ansible playbooks), in parallel.

## What is the default polling interval for agents?
{: #faqs-agent-poll-interval}
{: faq}
{: support}

{{site.data.keyword.bpshort}} maintains a queue of jobs that will be ran on an agent. The agent polls for {{site.data.keyword.bpshort}} Jobs, every one minute by default.

## Are there execution timeout limits when working with agents?
{: #faqs-agent-timeout}
{: faq}
{: support}

{{site.data.keyword.bpshort}} agents relax the timeout limitation for local-exec, remote-exec and Ansible playbook execution. These are limited to 60 minutes in the multi-tenant service to ensure fair service utilisation by all users. No duration is applied for jobs executed on agents. Long job execution times will require additional user cluster capacity and worker nodes to ensure timely execution of all jobs on the cluster.    

It is recommended to use a service like [Continous Delivery](docs/ContinuousDelivery?topic=ContinuousDelivery-getting-started) for long running jobs performing software installation tasks. 

## What is the difference between `agent-location` and `location` flag in agent service?
{: #faqs-agent-diff-location}
{: faq}
{: support}

The `--agent-location` parameter is a variable that specifies the region of the cluster where an agent service is deployed. For example, `us-south`. This must match the cluster region. 

The `--location` parameter is a variable that specifies the region that is supported by the {{site.data.keyword.bpshort}} service such as `us-south`, `us-east`, `eu-de`, `eu-gb`. The agent polls {{site.data.keyword.bpshort}} service instance from this location, for workspace or action jobs for processing.

## Can an agent run workspace jobs that are associated with different resource groups?
{: #faqs-agent-diff-rg}
{: faq}
{: support}

Yes, an agent can run workspace or actions jobs associated with any resource group, in an account. Agent (assignment) policies are used to assign the execution of jobs, based on resource group, region, and user tags to a specific agent. 

## Can an agent work with workspaces and actions belonging to different {{site.data.keyword.bpshort}} regions?
{: #faqs-agent-diff-region}
{: faq}
{: support}

Agents deployments are associated with a {{site.data.keyword.bpshort}} home region and geo for job execution. They can only execute workspace or action jobs defined in the same home geo such as, North America, or Europe.

The Agent periodically polls its home {{site.data.keyword.bpshort}} region to fetch and run jobs. It can only execute workspace or action jobs defined for the geo containing its home region. For example, an agent is deployed on a user cluster in Sydney is configured to with `eu-de` as it’s home location. The agent polls for jobs in the Europe geo, containing both the `eu-de` and `eu-gb` regions. To deploy resources using the Sydney agent, workspaces or actions must be created in the `eu-de` or `eu-gb` regions. 

## Is it possible to use an agent to execute jobs for multiple accounts?
{: #faqs-agent-register}
{: faq}
{: support}

No, agents are associated with a single parent {{site.data.keyword.bpshort}} account and can only execute jobs for workspaces or actions belonging to this account. 

## Can an existing workspace run jobs on an agent?
{: #faqs-agent-conf}
{: faq}
{: support}

Yes. Workspaces and actions are selected by policy to execute on agents. A {{site.data.keyword.bpshort}} `agent-selection-policy` will assign existing (or new) workspaces or actions to run on an target agent, if they match the policy attributes for tags, resource-group, location.   

For example, if you have an existing workspace: `wks-0120` with `tag=dev`, and you want the workspace to run on `Agent-1`. Create an `agent-selection-policy` with the rules to pick `Agent-1` when the `tag == dev`. Later, the workspace job such as plan, apply, update will be dynamically routed to run on `Agent-1`.

## What IAM permissions needed to deploy an agent?
{: #faqs-agent-permission}
{: faq}
{: support}

For information about access permissions, see [agent permissions](/docs/schematics?topic=schematics-access#agent-permissions).


## Can I inject self-signed or TLS certificates in {{site.data.keyword.containerlong_notm}} pod or container's trusted CA root certificate store during agent runtime?
{: #faqs-agent-certificate}
{: faq}
{: support}

Yes, follow these steps to inject the certificates into an agent runtime.

In the four `.cer` extension file names ensure that you modify to replace the space with underscore.
{: note}

1. Create a config map by using `.cer` file as shown in the `kubectrl` command.

    ```sh
    kubectl -n schematics-runtime create configmap bnpp-root —-from-file 2014-2044_BNPP_Root.cer
    ```
    {: pre}

    ```sh
    kubectl -n schematics-runtime create configmap bnpp-authentication —-from-file 2014-2029 BNPP_Users_Authentication.cer
    ```
    {: pre}

    ```sh
    kubectl -n schematics-runtime create configmap bnpp-infrastructure —-from-file 2014-2029 BNPP_Infrastructure.cer
    ```
    {: pre}

2. Mount config map file as a volume in a directory `/etc/ssl/certs/` as file `agent-runtime-deployment-certs.yaml` in a shared `bnpp_agent_deployment_files` directory.

The Shared directory `bnpp_agent_deployment_files` has two yaml files named 
    - `agent-runtime-deployment-certs.yaml` and
    - `agent-runtime-deployment.yaml`.

The `agent-runtime-deployment-certs.yaml` file updates the certificates and appends the `agent-runtime-deployment.yaml` file that provides you with the desired deployment details to inject the certificates without any additional changes.

## What attributes of workspaces or actions are used to dynamically select a target agent for execution
{: #agent-dynamic-attribute}
{: faq}
{: support}

The following attributes of a {{site.data.keyword.bpshort}} workspace or action are used to dynamically select the agent instance.
{: shortdesc}

- Resource group
- Location (region)
- Tags

The [Agent assignment policy](/docs/schematics?topic=schematics-policy-manage) for an agent instance determines which agent is selected to run a workspace or action job.

Here is a sample scenario for the usage of tags.

If your organization has three different network isolation zones (such as `Dev`, `HR-Stage`, and `HR-Prod`) and you have installed three agents (one each, for the three network isolation zones). You have defined an `agent-assignment-policy` for the agent running in `Dev`, with the selector as `tags=dev`. All workspaces that have `tags=dev` automatically are bound to the `Dev` agent. In other words, the `Dev` agent is used to download Terraform templates (from the Git repository) and run Terraform jobs. Similarly, the `agent-assignment-policy` can include other attributes of the workspaces to define the agent for job execution.

## How can I enable debug mode in an agent?
{: #faqs-agent-debugmode}
{: faq}
{: support}

You can follow these steps to enable or disable the debug mode of an agent.

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Click **Kubernetes** from the left navigator window, then click **Clusters** 
3. On the **Kubernetes Clusters** page, click your **cluster** > **Kubernetes dashboard**.
    - Click the **default** drop down to view the list of **Namespaces**:
        - In the drop down, type the **{{site.data.keyword.bpshort}}-job-runtime** namespaces.
        - Click **Config Map** from the **Config and Storage**.
        - From the **Config Maps** page. Click the three dots against **schematics-jobrunner-config**.
        - Click **Edit** to view the **Edit a resource** page with the **YAML**, and **JSON** tabs.
        - You can now edit the `JR_LOGGERLEVEL` parameter for job-runner microservice logging. By default the value is `-1` that indicated disable debug to enable you need to edit `JR_LOGGERLEVEL` as `0`.
        - Click **Update** to apply your edits.

## Can I upgrade an agent beta version to an agent General Availability (GA) version?
{: #faqs-agent-upgrade}
{: faq}
{: support}

No, you cannot upgrade agent beta setup to agent GA version.




## Are {{site.data.keyword.bpshort}} Agent the same as Terraform cloud agents?
{: #faqs-agent-terraform-agent}
{: faq}
{: support}

{{site.data.keyword.bpshort}} Agent performs a similar role to [Terraform Cloud agents](https://developer.hashicorp.com/terraform/cloud-docs/agents){: external}.


## Do the agents run on {{site.data.keyword.cloud_notm}} cloud resources?
{: #faqs-agent-run}
{: faq}
{: support}

{{site.data.keyword.bpshort}} Agent can run only workspace and action workloads. For the Beta, the agents are deployed in IBM Cloud {{site.data.keyword.containerlong_notm}} clusters in the user account.

## What are the minimum cluster configurations needed to support 30 jobs on the {{site.data.keyword.bpshort}} agent?
{: #faqs-agent-min-cluster-conf}
{: faq}
{: support}

For the {{site.data.keyword.vsi_is_full}} or {{site.data.keyword.containerlong}} cluster. You need `9` minimum number of nodes, with a `bx2.4x16` flavor, and edit the following agent microservices deployments to have the prescribed replica count.

| Microservice | Number of replicas |
| -- | -- |
| jobrunner | 4 |
| sandbox | 8 |
| runtime-ws | 16 |
{: caption="agent microservice deployments" caption-side="bottom"}

## How can a user identify the job is created by an agent?
{: #faqs-agent-job}
{: faq}
{: support}

You can identify that the workspace is created by an Agent through the workspace job logs.

## Is it possible that a workspace is created by an agent, still do not have a reference in the workspace job log?
{: #faqs-agent-job}
{: faq}
{: support}

No, If a agent creates workspace you must see a reference in the workspace job log. If you don't see the reference, then you must check that your policy validation is failed.

## Can {{site.data.keyword.bpshort}} Agent establish a connection with the private Git instance?
{: #faqs-git-instance-cert}
{: faq}
{: support}

Yes, {{site.data.keyword.bpshort}} Agent establishes a connection with the private Git instance. However, you need to own an SSL certificate and follow these steps in agent micro-services.

1. Establish a connection by configuring SSL certificate in `Jobrunner`, `Sandbox`, and `Runtime-ws` agent micro-services.
2. Configuration should be done by using {{site.data.keyword.containershort_notm}} configmap mounting.
   - create a configmap with the required SSL certificate, for example,

     ```bash
     kubectl -n schematics-job-runtime create configmap mytestcert --from-file cert.pem
     ```
     {: pre}

   - Use configmap as volume and mount as shared in the deployment file in `Jobrunner`, `Sandbox`, and `Runtime-ws` microservices.
     ```text
        apiVersion: apps/v1
        kind: Deployment
        metadata:
        annotations:
        deployment.kubernetes.io/revision: "1"
        kubernetes.io/change-cause: job_runner_1.0
        creationTimestamp: "2023-09-14T12:18:07Z"
        generation: 1
        labels:
        app: jobrunner
        name: jobrunner
        namespace: schematics-job-runtime
        resourceVersion: "23425"
        uid: fa66583a-8bdb-40a1-9b05-df2c2bf56656
        spec:
        progressDeadlineSeconds: 600
        .....
        .....
        volumes:
        - hostPath:
                path: /var/log/at
                type: ""
                name: at-events
        - hostPath:
                path: /var/log/schematics
                type: ""
                name: ext-logs
        - name: mytestcert  #### added as a volume 
                configMap:
                name: mytestcert
                status:
                availableReplicas: 1
                conditions:
        - lastTransitionTime: "2023-09-14T12:18:42Z"
                lastUpdateTime: "2023-09-14T12:18:42Z"
                message: Deployment has minimum availability.
                reason: MinimumReplicasAvailable
                status: "True"
                type: Available
        - lastTransitionTime: "2023-09-14T12:18:07Z"
                lastUpdateTime: "2023-09-14T12:18:42Z"
                message: ReplicaSet "jobrunner-7f9ffdf959" has successfully progressed.
                reason: NewReplicaSetAvailable
                status: "True"
                type: Progressing
                observedGeneration: 1
                readyReplicas: 1
                replicas: 1
                updatedReplicas: 1
     ```
     {: screen}

## Can {{site.data.keyword.bpshort}} Agent update a connection with the private Git instance?
{: #faqs-git-instance-update}
{: faq}
{: support}

Yes, you can update the agent with the metadata to perform catalog on boarding with the private Git instance. Use the sample update API request for reference.

   Perform this step only if an agent does not have metadata.
   {: important}

   ```curl
    curl -X PUT 'https://schematics.cloud.ibm.com/v2/agents/<agent_id\>' \
        -H 'Authorization: Bearer <token\>' \
        -H 'X-Feature-Agents: true' \
        -H 'refresh_token: <refresh_token\>' \
        -d '{
    "agent_metadata": [
            {
                "name": "purpose",
                "value": ["git"] 
            },
            {
                "name": "git_endpoints",
                "value": ["https://myprivate-gitinstance/testrepo"] 
            }
          ]
        }'
    ```
    {: pre}
