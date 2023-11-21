---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-20"

keywords: schematics agent deploying, deploying agent, agent deploy, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bplong_notm}} Agent beta-1 and beta-2 delivers a simplified agent installation process and policy for agent assignment. You can review the [beta-1 release](/docs/schematics?topic=schematics-schematics-relnotes&interface=cli#schematics-mar2223) documentation and explore. 
{: attention}

{{site.data.keyword.bpshort}} Agents is a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta1-limitations) that is available for evaluation and testing purposes. It is not intended for production usage.
{: beta}

# Deploying agents
{: #deploy-agent-overview}

Agents for {{site.data.keyword.bplong}} extend its ability to work directly with your cloud infrastructure on your private network or in any isolated network zones. 
{: shortdesc}

Follow the steps below to deploy and configure a {{site.data.keyword.bpshort}} agent. 

1. [Create an agent definition](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli#apply-agent-cli) to manage the agent deployment.
    This step initializes {{site.data.keyword.bpshort}} with the agent configuration that will subsequently be used to deploy your agent to its target location. 
2. [Deploy the agent](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli#create-agent-cli) using the [ibmcloud schematics agent plan](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-plan) and [ibmcloud schematics agent apply](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-apply) CLI commands or the corresponding APIs.

## Before you begin
{: #deploy-prereq}

Review and complete the steps described in [preparing for agent deployment](/docs/schematics?topic=schematics-plan-agent-overview). After creation of the cluster, COS instance and bucket gather the following information as an input to deploy your agent to your target location. 
{: shortdesc}

- A short description of the network zones and infrastructure accessible to the agent. 
- The `cluster ID`, `cluster resource group` and `region` of the Kubernetes cluster the agent will deployed on.
- the `COS instance name`, `COS bucket name` of the {{site.data.keyword.objectstorageshort}} bucket used for agent temporary data storage. The resource group and region of the COS instance and bucket must be the same as the cluster. 

## Creating an agent definition using the CLI 
{: #create-agent-cli}
{: cli}

As the first step, you must create an agent definition in your {{site.data.keyword.cloud_notm}} account, with the configuration that will be used to deploy the agent. For a complete list of an `agent create` options, see [ibmcloud schematics agent create](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-agent-create) command.
{: shortdesc}

To deploy a {{site.data.keyword.bpshort}} agent, the {{site.data.keyword.cloud_notm}} CLI [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be `1.12.9` or higher. 
{: important}

Before starting the deployment the cluster, COS instance and bucket must have been created. 
{: note}

Select the {{site.data.keyword.cloud_notm}} region where you wish to define and manage your agent from. Set the CLI region by running [`ibmcloud target -r <region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command. This must be the same region as the `location` specified on the `agent create` command. 

Example `agent create` syntax. The text between <> should be replaced with your values:

```sh
ibmcloud schematics agent create --name <agent-testing-prod-cli-mar-27-5> --location <eu-de> --agent-location <Frankfurt MZR> --version <1.0.0> --infra-type <ibm_kubernetes> --cluster-id <cb1c2dus01uf9mc0hkbg> --cluster-resource-group  <job-runner> --cos-instance-name <COSForAgentLogging> --cos-bucket <agentlogs> --cos-location <eu-de> --resource-group <Default>
```
{: pre}


Output

```text
Creating agent...
OK
                    
ID               agent-testing-prod-cli-mar-27-5.deA.dc97   
Name             agent-testing-prod-cli-mar-27-5   
Status           ACTIVE   
Version          1.0.0   
Location         eu-de   
Agent Location   Frankfurt MZR  
Resource Group   aac37f57b20142dba1a435c70aeb12df 
```
{: screen}

Record the `Agent ID` for use in subsequent commands. To display the agent details you can use the agent get command.

Example

```sh
ibmcloud schematics agent get --id agent-testing-prod-cli-mar-27-5.deA.dc97 
```
{: pre}

Output

```text
Retrieving agent...
OK
                    
ID               agent-testing-prod-cli-mar-27-5.deA.dc97   
Name             agent-testing-prod-cli-mar-27-5   
Status           ACTIVE   
Version          1.0.0   
Location         eu-de   
Agent Location   Frankfurt MZR  
Resource Group   Default 
```
{: screen}

## Verifying prerequisites for agent deployment using the CLI
{: #verify-agent-cli}
{: cli}

You can verify the agent definition and cluster availability using the agent plan command. This performs a prerequisite check of the target agent infrastructure. The command takes the `Agent ID` as input returned by the `agent create` command. The output of the agent plan command displays the list of relevant Kubernetes and agent property names, the expected value, actual value, and the result as `PASS` or `FAIL`.

Example

```sh
ibmcloud schematics agent plan --id agent-testing-prod-cli-mar-27-5.deA.dc97  
```
{: pre}

Output

```text
Initiating agent plan...
Job ID	.ACTIVITY.600cadf9
```
{: screen}

Example

```sh
ibmcloud schematics agent get --id agent-testing-prod-cli-mar-27-5.deA.dc97   
```
{: pre}

Output

```text
Retrieving agent...
OK
                    
ID               agent-testing-prod-cli-mar-27-5.deA.dc97    
Name             agent-testing-prod-cli-mar-27-5   
Status           ACTIVE   
Version             
Location         eu-de   
Agent Location   Frankfurt MZR  
Resource Group   Default   
                 
Recent Job   Job ID               Status                             Last modified   
PRS          .ACTIVITY.600cadf9   Triggered pre-requisite scanning   0001-01-01T00:00:00.000Z 
```
{: screen}

## Deploying an agent using the CLI
{: #apply-agent-cli}
{: cli}

You use the agent definition to deploy the agent with the `agent apply` command. The `agent apply` command takes the `Agent ID` as input. You can upgrade an existing deployment by using the force deploy option.

The agent deployment will take several minutes to complete. 
{: shortdesc}

```sh
ibmcloud schematics agent apply --id agent-testing-prod-cli-mar-27-5.deA.dc97  
```
{: pre}

Output

```text
Initiating agent apply...
Job ID	.ACTIVITY.465e9716
```
{: screen}

Example

```sh
ibmcloud schematics agent get --id agent-testing-prod-cli-mar-27-5.deA.dc97 
```
{: pre}

Output

```text
Retrieving agent...
OK
                    
ID               agent-testing-prod-cli-mar-27-5.deA.dc97   
Name             agent-testing-prod-cli-mar-27-5   
Status           ACTIVE   
Version          1.0.0   
Location         eu-de   
Agent Location   Frankfurt MZR  
Resource Group   Default   
                 
Recent Job   Job ID               Status                 Last modified   
DEPLOY       .ACTIVITY.465e9716   Triggered deployment   2023-03-27T12:25:01.239Z 
```
{: screen}

## Verifying the agent deployment using the CLI
{: #d-agent-cli}
{: cli}

You can verify the health of the recently deployed agent using the `agent health` command. The command takes the `Agent ID` as input. The output displays the list of relevant Kubernetes and agent health property names, the expected value, actual value, and the result as `PASS` or `FAIL`.

Example

```sh
ibmcloud schematics agent health --id agent-testing-prod-cli-mar-27-5.deA.dc97  
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
ibmcloud schematics agent get --id agent-testing-prod-cli-mar-27-5.deA.dc97  
```
{: pre}

Output

```text
Retrieving agent...
OK
                    
ID               agent-testing-prod-cli-mar-27-5.deA.dc97   
Name             agent-testing-prod-cli-mar-27-5   
Status           ACTIVE   
Version             
Location         eu-de   
Agent Location   Frankfurt MZR  
Resource Group   Default   
                 
Recent Job   Job ID                             Status                   Last modified   
DEPLOY       f5c6987ce53032547b6d5d5f870dfe5f   Job Success               0001-01-01T00:00:00.000Z   
HEALTH       .ACTIVITY.f6f77588                 Triggered health check   2023-03-27T12:31:15.326Z 
```
{: screen}

In addition, you can use the Kubernetes CLI (kubectl) or Kubernetes dashboard for your cluster to view the status and logs of the agent related microservices, its Pods, Deployment, Configmap, and Cluster bindings in the namespaces, `schematics-agent-observe`, `schematics-sandbox`, `schematics-runtime` and `schematics-job-runtime`

## Creating an agent using the {{site.data.keyword.bpshort}} API
{: #create-agent-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to create an IAM access token and authenticate with {{site.data.keyword.bpshort}} via the API. For more information, see [Create a agent](/apidocs/schematics/schematics_internal_v1#create-agent-data) using API.

Example

```json
  POST /v2/agents HTTP/1.1
  Host: schematics.cloud.ibm.com
  Content-Type: application/json
  Authorization: Bearer 

  {
    "name": "agentb1-gsmforvpc-mar17",
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
        "cos_instance_name": "agent-beta-1-cos-instance",
        "cos_bucket_name": "agent-beta-1-cos-bucket"
    },
    "user_state": {
        "state": "enable"
    }
}
```
{: codeblock}

Verify that the agent definition is created successfully as shown in the output. Record the agent ID for use in subsequent commands. For example, `agentb1-gsmforvpc-mar17.soA.115c`.
{: shortdesc}

Output

```text
  {
      "name": "agentb1-gsmforvpc-mar17",
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
          "set_by": "geetha_sathyamurthy@in.ibm.com",
          "set_at": "2023-03-16T18:08:18.399224788Z"
      },
      "agent_crn": "crn:v1:bluemix:public:schematics:eu-de:a/1f7277194bb748cdb1d35fd8fb85a7cb:9ae7be42-0d59-415c-a6ce-0b662f520a4d:agent:agentb1-gsmforvpc-mar17.soA.115c",
      "id": "agentb1-gsmforvpc-mar17.soA.115c",
      "created_at": "2023-03-16T18:08:18.39924616Z",
      "creation_by": "geetha_sathyamurthy@in.ibm.com",
      "updated_at": "0001-01-01T00:00:00Z",
      "system_state": {
          "status_code": "draft"
      },
      "agent_kpi": {}
  }
```
{: screen}

Now, call the _agent deploy_ API with the `agent ID` to create the {{site.data.keyword.bpshort}} workspace that will deploy the agent. The _agent deploy_ operation, invokes both the _agent plan_, and _agent apply_ operations to setup the agent.

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
  PUT /v2/agents/agentb1-gsmforvpc-mar17.soA.115c/deploy HTTP/1.1
  Host: schematics.cloud.ibm.com
  Content-Type: application/json
  Authorization: Bearer 
```
{: codeblock}

Output 

```text
{
    "workspace_id": "eu-de.workspace.agentb1-gsmforvpc-mar17-deploy.13a324df",
    "job_id": ".ACTIVITY.7f40fdc0",
    "updated_at": "2023-03-16T18:13:27.217864196Z",
    "updated_by": "geetha_sathyamurthy@in.ibm.com",
    "status_code": "PENDING",
    "status_message": "Triggered deployment"
}
```
{: screen}

## Next steps
{: #agent-create-nextsteps}

The next step is to [create an agent assignment policy](/docs/schematics?topic=schematics-policy-manage) for the newly deployed agent.  The agent assignment policy is used by {{site.data.keyword.bpshort}} to dynamically route the Git repo download jobs, Workspace or Terraform jobs, and Action or Ansible jobs to an agent.

You can check out the [agent FAQ](/docs/schematics?topic=schematics-faqs-agent&interface=ui) for any common questions related to an agent.

When the agent is no longer required, it can be removed following the steps in [delete an agent](/docs/schematics?topic=schematics-delete-agent-overview&interface=ui).
