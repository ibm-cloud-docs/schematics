---

copyright:
  years: 2017, 2025
lastupdated: "2025-09-24"

keywords: schematics agent deploying, deploying agent, agent deploy, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deploying agents
{: #deploy-agent-overview}

Create an agent registration in the selected {{site.data.keyword.bplong}} region to work directly in your cloud infrastructure on private network or isolated network zones.
{: shortdesc}

Follow the steps to create and deploy an agent.

1. [Create an agent definition](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli#apply-agent-cli) to manage the agent deployment.
    This step initializes {{site.data.keyword.bpshort}} with the agent configuration that is used to deploy your agent to its target location.
2. [Deploy the agent](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli#create-agent-cli) by using the [`ibmcloud schematics agent validate`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-plan) and [`ibmcloud schematics agent deploy`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-apply) CLI commands or the corresponding APIs.

## Before you begin
{: #deploy-prereq}

Review and complete the steps that are described in [preparing for agent deployment](/docs/schematics?topic=schematics-plan-agent-overview). After creation of the cluster, the {{site.data.keyword.cos_full_notm}} instance, and the {{site.data.keyword.cos_full_notm}} bucket, gather the following information as an input to deploy your agent to its target location.
{: shortdesc}

- The cluster, {{site.data.keyword.cos_full_notm}} instance, and {{site.data.keyword.cos_full_notm}} bucket are created in the same resource group.
- Record the `cluster ID`, `cluster resource group`, and `region` of the {{site.data.keyword.containershort}} cluster the agent deploys.
- The `{{site.data.keyword.cos_full}} instance name`, `{{site.data.keyword.cos_full_notm}} bucket name` of the {{site.data.keyword.objectstorageshort}} bucket is used for agent temporary data storage. The resource group and region of the {{site.data.keyword.cos_full_notm}} instance and bucket must be the same as the cluster.
- Optional - if you need to update the proxy server to an Agent microservices, refer to [configuring {{site.data.keyword.bpshort}} agents to a proxy server](/docs/schematics?topic=schematics-proxy-agent-overview).
- Optional - if you are using a private Git instance, you need to establish the connection with an agent through certificate. For more information, see [steps to associate an agent with private Git instance](/docs/schematics?topic=schematics-faqs-agent&interface=cli#faqs-git-instance-cert).

   You need to see that the `Cluster`, and the `{{site.data.keyword.cos_full_notm}} instance` are in the same resource group.
   {: important}

## Creating an agent definition
{: #create-agent-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![hamburger icon](images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > **Extensions** > [**Create Agent**](https://cloud.ibm.com/schematics/agents/create){: external}.
    - In **Define agent details** section:
        - Enter a unique **Agent name**.
        - Select **Location** and **Resource group** from the drop-down option.
        - Enter **Tags**, and **Description** for the agent.
    - In **Assign to cluster** section:
        - Select the `{{site.data.keyword.containerlong_notm}}` or the `{{site.data.keyword.redhat_openshift_notm}}` service.
        - Select your cluster name.
        - In the **Define COS Instance**
            - Enter the **COS instance name**
            - Enter the **COS bucket name**
            - Enter the **COS bucket region**
3. Click **Define**.
4. Click **Validate** to validate the cluster and {{site.data.keyword.cos_full_notm}} configuration.
5. Click **Deploy** to deploy an agent.

## Creating an agent definition through CLI
{: #create-agent-cli}
{: cli}

As the first step, you must create an agent definition in your {{site.data.keyword.cloud_notm}} account, with the configuration that is used to deploy the agent. For a complete list of an `agent create` options, see [ibmcloud schematics agent create](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-agent-create) command.
{: shortdesc}

Select the {{site.data.keyword.cloud_notm}} region where you want to define and manage your agent from. Set the [CLI region command](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) by running `ibmcloud target -r <region>`. The region must be the same region as the `location` specified on the `agent create` command. The {{site.data.keyword.cos_full_notm}} bucket location must be of the form `eu-gb` or `us-south` and not a city name.

Example `agent create` syntax. The text between `< >` must add with your values:

```sh
ibmcloud schematics agent create --name <agent-ga-prod-cli-jan-10> --location <us-south> --agent-location <us-south> --version <1.0.0> --infra-type <ibm_kubernetes> --cluster-id <cg3fgvad0dak571xxx> --cluster-resource-group <Default> --cos-instance-name <agent-cos-instance> --cos-bucket <agent-cos-bucket> --cos-location <us-east> --resource-group <Default>
```
{: pre}

Output

```text
Creating agent...
OK

ID               agent-ga-prod-cli-jan-10.soA.cd1c
Name             agent-ga-prod-cli-jan-10
Status           Defined
Version          1.0.0
Location         us-south
Agent Location   us-south
Resource Group   aac37f57b20142dba1a435c70aeb12df
Metadata         [Metadata]
                 - [git]
                 - [github.com]
```
{: screen}

Record the `Agent ID` for use in subsequent commands. To display the agent details, you can use the agent get command.

Example

```sh
ibmcloud schematics agent get --id agent-ga-prod-cli-jan-10.soA.cd1c
```
{: pre}

Output

```text
Retrieving agent...
OK

ID               agent-ga-prod-cli-jan-10.soA.cd1c
Name             agent-ga-prod-cli-jan-10
Status           ACTIVE
Version          1.0.0
Location         us-south
Agent Location   us-south
Resource Group   Default
Metadata         [Metadata]
                 - [git]
                 - [github.com]
```
{: screen}

## Verifying prerequisites for agent deployment through CLI
{: #verify-agent-cli}
{: cli}

You can verify the agent definition and cluster availability by using the agent validate command. The validate does a prerequisite check of the target agent infrastructure. The command takes the `Agent ID` as input returned by the `agent create` command. The output of the agent validates command displays the list of relevant Kubernetes and agent property names, the expected value, actual value, and the result as `PASS` or `FAIL`.

Example

```sh
ibmcloud schematics agent validate --id agent-ga-prod-cli-jan-10.soA.cd1c
```
{: pre}

Output

```text
Initiating agent validate...
Job ID	.ACTIVITY.600cadf9

Polling status...
Status	job_pending
Status	job_in_progress
Status	job_in_progress
Status	job_in_progress
Status	job_finished
```
{: screen}

Example

```sh
ibmcloud schematics agent get --id agent-ga-prod-cli-jan-10.soA.cd1c
```
{: pre}

Output

```text
Retrieving agent...
OK

ID               agent-ga-prod-cli-jan-10.soA.cd1c
Name             agent-ga-prod-cli-jan-10
Status           ACTIVE
Version
Location         us-south
Agent Location   us-south
Resource Group   Default

Recent Job   Job ID                             Status                  Last modified
DEPLOY       -                                  Deploy in progress      2024-01-10T09:54:32.607Z
VALIDATE     8b168c1e0e4b35708e95c2af9a99d9d4   Successful validation   2024-01-10T09:53:48.435Z
```
{: screen}

## Deploying an agent through CLI
{: #apply-agent-cli}
{: cli}

You use the agent definition to deploy the agent with the `agent deploy` command. The `agent deploy` command takes the `Agent ID` as input. You can upgrade an existing deployment by using the `force deploy` option.

The agent deployment takes several minutes to complete.
{: shortdesc}

```sh
ibmcloud schematics agent deploy --id agent-ga-prod-cli-jan-10.soA.cd1c
```
{: pre}

Output

```text
Initiating agent deploy...
Job ID	.ACTIVITY.465e9716
```
{: screen}

Example

```sh
ibmcloud schematics agent get --id agent-ga-prod-cli-jan-10.soA.cd1c
```
{: pre}

Output

```text
Retrieving agent...
OK

ID               agent-ga-prod-cli-jan-10.soA.cd1c
Name             agent-ga-prod-cli-jan-10
Status           ACTIVE
Version          1.0.0
Location         us-south
Agent Location   us-south
Resource Group   Default

Recent Job   Job ID               Status                 Last modified
DEPLOY       .ACTIVITY.465e9716   Triggered deployment   2024-01-10T10:20:48.435Z
VALIDATE     8b168c1e0e4b35708e   Successful validation   2024-01-10T09:53:48.435Z
```
{: screen}

## Verifying the agent deployment through CLI
{: #d-agent-cli}
{: cli}

You can verify the health of the recently deployed agent by using the `agent health` command. The command takes the `Agent ID` as input. The output displays the list of relevant Kubernetes with the agent health property names, the expected value, actual value, and the result as `PASS` or `FAIL`.

Example

```sh
ibmcloud schematics agent health --id agent-ga-prod-cli-jan-10.soA.cd1c
```
{: pre}

Output

```text
Initiating agent health...
Job ID	.ACTIVITY.f6f77588
```
{: screen}

Example

```sh
ibmcloud schematics agent get --id agent-ga-prod-cli-jan-10.soA.cd1c
```
{: pre}

Output

```text
Retrieving agent...
OK

ID               agent-ga-prod-cli-jan-10.soA.cd1c
Name             agent-ga-prod-cli-jan-10
Status           ACTIVE
Version
Location         us-south
Agent Location   us-south
Resource Group   Default

Recent Job   Job ID                             Status                   Last modified
DEPLOY       f5c6987ce53032547b6d5d5f870dfe5f   Job Success               0001-01-01T00:00:00.000Z
HEALTH       .ACTIVITY.f6f77588                 Triggered health check   2023-03-27T12:31:15.326Z
```
{: screen}

In addition, you can use the Kubernetes CLI (kubectl) or Kubernetes dashboard of your cluster to view the status and logs of the agent-related microservices, pods, deployment, configmap, and Cluster bindings in the namespaces, `schematics-agent-observe`, `schematics-sandbox`, `schematics-runtime` and `schematics-job-runtime`.

## Creating an agent through API
{: #create-agent-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to create an IAM access token and authenticate with {{site.data.keyword.bpshort}} through the API. For more information, see [Create an agent](/apidocs/schematics/schematics#create-agent-data) by using API.

Example

```json
  POST /v2/agents HTTP/1.1
  Host: schematics.cloud.ibm.com
  Content-Type: application/json
  Authorization: Bearer

  {
    "name": "agentb1-gsmforvpc",
    "description": "Create Agent",
    "resource_group": "Default",
    "tags": [
        "env:prod",
        "mytest"
    ],
    "version": "v1.0.0",
    "schematics_location": "eu-de",
    "agent_location": "Frankfurt MZR",
    "agent_infrastructure": {
        "infra_type": "ibm_kubernetes",
        "cluster_id": "cg3fgvad0dak571op4g0",
        "cluster_resource_group": "Default",
        "cos_instance_name": "agent-cos-instance",
        "cos_bucket_name": "agent-cos-bucket"
    },
    "user_state": {
        "state": "enable"
    }
}
```
{: codeblock}

Verify that the agent definition is created successfully as shown in the output. Record the agent ID for use in subsequent commands. For example, `agentb1-gsmforvpc.soA.115c`.
{: shortdesc}

Output

```text
  {
      "name": "agentb1-gsmforvpc",
      "description": "Create Agent",
      "resource_group": "aac37f57b20142dba1a435c70aeb12df",
      "tags": [
          "env:prod",
          "mytest"
      ],
      "version": "v1.0.0",
      "schematics_location": "eu-de",
      "agent_location": "Frankfurt MZR",
      "user_state": {
          "state": "enable",
          "set_by": "xxxx@in.ibm.com",
          "set_at": "2023-03-16T18:08:18.399224788Z"
      },
      "agent_crn": "crn:v1:bluemix:public:schematics:eu-de:a/1f7277194bb748cdxxxxxxxxxxx42-0d59-415c-a6ce-0b662f520a4d:agent:agentb1-gsmforvpc.soA.115c",
      "id": "agentb1-gsmforvpc.soA.115c",
      "created_at": "2023-03-16T18:08:18.39924616Z",
      "creation_by": "xxxxx@in.ibm.com",
      "updated_at": "0001-01-01T00:00:00Z",
      "system_state": {
          "status_code": "draft"
      },
      "agent_kpi": {}
  }
```
{: screen}

Now, run the `agent deploy` API with the `agent ID` to create the {{site.data.keyword.bpshort}} workspace that deploys the agent. The `agent deploy` operation starts both the `agent validate`, and `agent deploy` operations to setup the agent.

Syntax

```json
  PUT /v2/agents/<enter your agentID>/deploy HTTP/1.1
  Host: schematics.cloud.ibm.com
  Content-Type: application/json
  Authorization: Bearer
```
{: codeblock}

Example

```json
  PUT /v2/agents/agentb1-gsmforvpc.soA.115c/deploy HTTP/1.1
  Host: schematics.cloud.ibm.com
  Content-Type: application/json
  Authorization: Bearer
```
{: codeblock}

Output

```text
{
    "workspace_id": "eu-de.workspace.agentb1-gsmforvpc-deploy.1xxxxdf",
    "job_id": ".ACTIVITY.7f40fdc0",
    "updated_at": "2023-03-16T18:13:27.217864196Z",
    "updated_by": "xxxx@in.ibm.com",
    "status_code": "PENDING",
    "status_message": "Triggered deployment"
}
```
{: screen}

## Creating an agent through Terraform
{: #create-agent-terraform}
{: terraform}

To create the {{site.data.keyword.bpshort}} Agent deployment using Terraform, define the `ibm_schematics_agent_deploy` resource in your Terraform configuration file. Complete the following steps to create the {{site.data.keyword.bpshort}} Agent.

1. Install the [Terrafrom CLI](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started).
2. [Configure the {{site.data.keyword.terraform-provider_full_notm}} plug-in](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started#install_provider-step).
3. [Test your configuration](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-getting-started#test-terraform-template).
4. Define `ibm_schematics_agent` resource in `main.tf` file.

    ```terraform
    resource "ibm_schematics_agent" "schematics_agent_instance" {
    agent_infrastructure {
            infra_type = "ibm_kubernetes"
            cluster_id = "cluster_id"
            cluster_resource_group = "cluster_resource_group"
            cos_instance_name = "cos_instance_name"
            cos_bucket_name = "cos_bucket_name"
            cos_bucket_region = "cos_bucket_region"
    }
    agent_location = "us-south"
    agent_metadata {
            name = "purpose"
            value = ["git", "terraform", "ansible"]
    }
    description = "Create Agent"
    name = "MyDevAgent"
    resource_group = "Default"
    schematics_location = "us-south"
    tags = ["agent-MyDevAgent"]
    version = "1.0.0"
    }
    ```
    {: codeblock}

5. Initialize

    ```sh
    terraform init
    ```
    {: pre}

6. Apply

    ```sh
    terraform apply
    ```
    {: pre}

7. Use ibm_schematics_agent_deploy resource to deploy an agent.

    ```terraform
    resource "ibm_schematics_agent_deploy" "schematics_agent_deploy_instance" {
    agent_id = "agent_id"
    }
    ```
    {: codeblock}

You can check the [{{site.data.keyword.terraform-provider_full_notm}}](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/schematics_agent_deploy){: external} documentation for more parameters specific to the resource.
{: note}

## Note
{: #agent-create-note}

After the Agent deployment is completed for `ca-mon`, it will initially show an error status. To resolve this, you need to [create a  Virtual Private Endpoint Gateway (VPE Gateway)](/docs/vpc?topic=vpc-vpc-reference#command-examples-endpoint-gateway-create) for the Schematics `private` region by targeting the `kube-vpeg-<cluster_IDxxxx>` security group in the `schematics-runtime` namespace. This process takes around 5 minutes. Once completed, the Agent deployment status changes to complete.


## Next steps
{: #agent-create-nextsteps}

Deploying and configuration of an agent are complete.

- If you are using a private Git instance, then establish the connection with an agent through certificate. For more information, see [steps to associate an agent to connect](/docs/schematics?topic=schematics-faqs-agent&interface=cli#faqs-git-instance-cert).
- For configuring and provisioning your infrastructure by using, refer to [agent policies](/docs/schematics?topic=schematics-policy-manage). The agent policy is used by {{site.data.keyword.bpshort}} to dynamically route the Git repo download jobs, Workspace or Terraform jobs, and Action or Ansible jobs to an agent.
- Manage your [agent and Kubernetes cluster](/docs/schematics?topic=schematics-configure-k8s-cluster).
- You can check out the [agent FAQ](/docs/schematics?topic=schematics-faqs-agent&interface=ui) for any common questions that are related to agents.
- When the agent is no longer in need, it can be removed following the steps in [delete an agent](/docs/schematics?topic=schematics-delete-agent-overview&interface=ui).
