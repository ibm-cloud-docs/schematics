---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-23"

keywords: schematics agent deploying, deploying agent, agent deploy, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deploying agents
{: #deploy-agent-overview}

{{site.data.keyword.bpshort}} Agents is a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agent](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

{{site.data.keyword.bplong}} Agents extend {{site.data.keyword.bpshort}}'s ability to work directly with your private cloud infrastructure on your private network. Deploying an agent is a multi-step process. 
{: shortdesc}

Follow the steps below to deploy and configure a {{site.data.keyword.bpshort}} agent. 

- [Create an agent definition](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli#deploy-agent-cli) to manage deploying the agent. This step initializes an {{site.data.keyword.bpshort}} definition with the information required to deploy your agent using {{site.data.keyword.bpshort}} _agent plan_ and _agent apply_ operations. 
- [Deploy the agent](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli#create-agent-cli) using the CLI or API, by invoking the [ibmcloud schematics agent plan](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-plan) and [ibmcloud schematics agent apply](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-apply) commands.
- [Define the agent policies](/docs/schematics?topic=schematics-policy-manage&interface=ui).
- [Check the health of the agent](/docs/schematics?topic=schematics-agent-health&interface=cli).

## Before your begin
{: #deploy-prereq}

Complete the following pre-requisite steps to deploy your agent.

- Select an existing [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters), [{{site.data.keyword.vpc_full}}](/docs/openshift?topic=openshift-cluster-create-vpc-gen2&interface=ui) cluster, or {{site.data.keyword.redhat_openshift_full}} cluster ID to host the agent deployment. Record the following key information about the cluster for later use, `cluster ID`, `cluster resource group`.
- Select existing resources such as an {{site.data.keyword.cos_full_notm}} instance and {{site.data.keyword.objectstorageshort}} bucket for the specified region. Record the following key information about the COS resources for later use, `COS instance name`, `COS bucket name`, `COS bucket region`, `COS resource group`.
- If deploying your agent using the {{site.data.keyword.bpshort}} API, generate the necessary [IAM authorization token](docs/account?topic=account-serviceauth&interface=ui#auth-cli) and refresh tokens to authenticate the API request.   


## Deploying an agent using the CLI 
{: #deploy-agent-cli}
{: cli}

Create the agent definition using the CLI. 

For a complete listing of _agent create_ options, see the [ibmcloud schematics agent create](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-agent-create) command.
{: shortdesc}

To deploy a {{site.data.keyword.bpshort}} agent, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.12.7`.
{: important}

Example

```sh
ibmcloud schematics agent create --name gsmmar2cliv2-agent-test --location eu-de --agent-location us-south --version 0.0.1 --infra-type ibm_kubernetes --cluster-id cgcnl5f20077ild56eng --cluster-resource-group Default --cos-instance-name agent-cos-storage --cos-bucket agent-cos-bucket --cos-location jp-tok --resource-group Default
```
{: pre}

Output

```text
Creating agent...
OK
                    
ID               gsmmar2cliv2-agent-test.deA.391b   
Name             gsmmar2cliv2-agent-test   
Status           ACTIVE   
Version          0.0.1   
Location         eu-de   
Agent Location   us-south   
Resource Group   b9b7892b87734a8b814342a6adef361d  
```
{: screen}

Record the `Agent ID` for use in subsequent commands.  

After creating the agent definition, the agent can be deployed using the _agent plan_ and _agent apply_ 
commands. The _agent plan_ command performs prerequisite checks on the agent definition and enviromnment before deployment. The command takes the `Agent ID` as input. 

Example

```sh
ibmcloud schematics agent plan --id gsmmar2cliv2-agent-test.deA.391b  
```
{: pre}

Output

```text
Running plan...
Plan ID: .ACTIVITY.5a78e7a5


                                   
Agent settings                  
ID                              gsmmar2cliv2-agent-test.deA.391b   
Name                            gsmmar2cliv2-agent-test   
Version                            
Location                        eu-de   
Agent Location                  us-south   
Resource Group                  Default   
User status                     ACTIVE   
System status                   draft   
Agents Jobs                     
Plan Job                        
Job ID                          .ACTIVITY.5a78e7a5   
Job status                      PENDING   
Job last validation timestamp   0001-01-01T00:00:00.000Z   
Job output                      Triggered pre-requisite scanning   
Job log URL                     https://eu-de.schematics.test.cloud.ibm.com/v1/workspaces/eu-de.workspace.gsmmar2cliv2-agent-test-prs.30e0d302/runtime_data/353c2b14-aa75-40/log_store   
                                   
OK
```
{: screen}

Now, use the `agent ID` with the _agent apply_ command to deploy the agent, or upgrade an existing deployment using the force deploy option.

Example

```sh
ibmcloud schematics agent apply --id gsmmar2cliv2-agent-test.deA.391b  
```
{: pre}

Output

```text
Running apply...
Apply ID: .ACTIVITY.fc7a33f3
Agent settings                  
ID                              gsmmar2cliv2-agent-test.deA.391b   
Name                            gsmmar2cliv2-agent-test   
Version                            
Location                        eu-de   
Agent Location                  us-south   
Resource Group                  Default   
User status                     ACTIVE   
System status                   draft   
Agents Jobs                     
Deploy Job                      
Job ID                          .ACTIVITY.fc7a33f3   
Job status                      PENDING   
Job last validation timestamp   2023-03-21T11:49:19.577Z   
Job output                      Triggered deployment   
Job log URL                     https://eu-de.schematics.test.cloud.ibm.com/v1/workspaces/eu-de.workspace.gsmmar2cliv2-agent-test-deploy.89fa2a8e/runtime_data/87794039-fc82-47/log_store   
                                   
OK
```
{: screen}

## Deploying an agent using the {{site.data.keyword.bpshort}} API
{: #create-agent-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to create an IAM access token and authenticate with {{site.data.keyword.bpshort}} via the API. For more information, see [Create a agent](apidocs/schematics/schematics_internal_v1#create-agent-data) using API.

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
    "schematics_location": "us-south",
    "agent_location": "us-south",
    "agent_infrastructure": {
        "infra_type": "ibm_kubernetes",
        "cluster_id": "cg3fgvad0dak571op4g0",
        "cluster_resource_group": "Default",
        "cos_instance_name": "agent-beta-1-cos-instance",
        "cos_bucket_name": "agent-beta-1-cos-bucket",
        "cos_bucket_region": "us-east",
        "cos_resource_group": "Default"
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
      "schematics_location": "us-south",
      "agent_location": "us-south",
      "user_state": {
          "state": "enable",
          "set_by": "geetha_sathyamurthy@in.ibm.com",
          "set_at": "2023-03-16T18:08:18.399224788Z"
      },
      "agent_crn": "crn:v1:bluemix:public:schematics:us-south:a/1f7277194bb748cdb1d35fd8fb85a7cb:9ae7be42-0d59-415c-a6ce-0b662f520a4d:agent:agentb1-gsmforvpc-mar17.soA.115c",
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

Now, call the _agent deploy_ API with the `agent ID` to create the {{site.data.keyword.bpshort}} workspace that will deploy the agent. The _agent deploy_ operation, inturn invokes both the _agent plan_, and _agent apply_ operations to setup the agent.

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
    "workspace_id": "us-south.workspace.agentb1-gsmforvpc-mar17-deploy.13a324df",
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

When your agent is up and running, you can perform the following administration tasks:
1. Access the Kubernetes dashboard to view the services that are created.
    - Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
    - Access **Kubernetes** > **Clusters**.
    - Click [...] three dots against your cluster name.
    - Click **Kubernete dashboard**.
    - Click the **Namespaces** drop down to view the set of namespaces by name schematics. For example, `schematics-agent-observe`, `schematics-job-runtime`, `schematics-runtime`, and so on.
    - In each namespace, you can view the **Workloads** > **Deployments**, **Jobs**, **Pods**, **Replica Sets**, and **Cluster** binding information.

You can check out the [agent FAQ](/docs/schematics?topic=schematics-faqs-agent&interface=ui) for any common questions related to an agent.

When the agent is no longer required, it can be removed following the steps in [delete an agent](/docs/schematics?topic=schematics-delete-agent-overview&interface=ui).
