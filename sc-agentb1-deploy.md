---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-20"

keywords: schematics agent deploying, deploying agent, agent deploy, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deploying agent
{: #deploy-agent-overview}

{{site.data.keyword.bpshort}} Agents is a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agent](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

{{site.data.keyword.bplong}} Agent extends {{site.data.keyword.bpshort}} ability to work directly with your private cloud infrastructure on your private network. Deploy an agent has a dependency command called _agent create_ and _agent deploy_ also is a multiple process command that runs _agent plan_ and _agent apply_ command.
{: shortdesc}

Consider the following steps to deploy the {{site.data.keyword.bpshort}} agent each time.

- Agent create: Initializes the {{site.data.keyword.bpshort}} information for an agent to deploy.
- Agent deploy: Creates {{site.data.keyword.bpshort}} Workspace, invokes [ibmcloud schematics agent plan](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-plan), and [ibmcloud schematics agent apply](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-apply) commands.
- [Define the policy for the agent](/docs/schematics?topic=schematics-policy-manage&interface=ui).
- [Check the health of an agent](/docs/schematics?topic=schematics-agent-health&interface=cli).

## Before your begin
{: #deploy-prereq}

Consider the following pre-requisite steps to deploy an agent.

- Create an [authorization token](docs/account?topic=account-serviceauth&interface=ui#auth-cli) and refresh token in the service endpoint that you want to run your agent. 
- Fetch an existing [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters), [{{site.data.keyword.vpc_full}}](/docs/openshift?topic=openshift-cluster-create-vpc-gen2&interface=ui) clusters, or {{site.data.keyword.redhat_openshift_full}} cluster ID. For example, `cluster ID`, `cluster resource group`.
- Fetch an existing resources such as {{site.data.keyword.cos_full_notm}}, {{site.data.keyword.cloud_notm}} {{site.data.keyword.objectstorageshort}} bucket of the specific region details. For example, `COS instance name`, `COS bucket name`, `COS bucket region`, `COS resource group`.

## Deploying an agent through the CLI 
{: #deploy-agent-cli}
{: cli}

Create your agent by using CLI. Create For a complete listing of _agent create_ options, see [ibmcloud schematics agent create](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-agent-create) command.
{: shortdesc}

To work with {{site.data.keyword.bpshort}} Agent, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.12.7`.
{: important}

Example

```sh
ibmcloud schematics agent create --name testagentname --location us-south --agent-location us-south --version 0.0.1 --infra-type ibm_kubernetes --cluster-id cfgh000000000vfq2ugp0 --cluster-resource-group Default --cos-instance-name agents-cos-instancename --cos-bucket agent-cos-bucketname --cos-location us-south --resource-group Default
```
{: pre}

Output

```text
Creating agent...
OK
                    
ID               magent.soA.94f1   
Name             magent   
Status           ACTIVE   
Version          0.0.1   
Location         us-south   
Agent Location   us-south   
Resource Group   Default 
```
{: screen}

Now, you can use the `agent ID` to call an _agent deploy_ command to create the {{site.data.keyword.bpshort}} workspace. The _agent deploy_ inturn invokes _agent plan_, and _agent apply_ command to setup an agent.

Example

```sh
ibmcloud schematics agent deploy --id magent.soA.94f1 
```
{: pre}

Output

```text
{
    "workspace_id": "us-south.workspace.agent-testagentname",
    "job_id": ".ACTIVITY.3fe6a2e3",
    "updated_at": "2023-03-19T15:55:30.8050000",
    "updated_by": "test@in.ibm.com",
    "status_code": "PENDING",
    "status_message": "Triggered deployment"
}
```
{: screen}


## Deploying an agent through the API
{: #create-agent-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, see [Create a agent](apidocs/schematics/schematics_internal_v1#create-agent-data) by using API.

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

Verify that the agent is created successfully as shown in the output. Extract the agent ID. For example, `agentb1-gsmforvpc-mar17.soA.115c`.
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

Now, you can use the `agent ID` to call an _agent deploy_ API to create the {{site.data.keyword.bpshort}} workspace. The _agent deploy_ inturn invokes _agent plan_, and _agent apply_ command to setup an agent.

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

## Verifying an agent
{: #verify-agent}

1. The _agent deploy_ command creates a workspace in your {{site.data.keyword.cloud_notm}} account.
    - Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
    - Access **Schematics** > **Workspaces** > [**workspace**](https://cloud.ibm.com/schematics/workspaces){: external}.
    - In **workspace List** section:
        - Click your **Workspace** to view the Job logs, where _agent plan_ and _agent apply_ are run.
        - Click **Settings** to view the **Catalog offering** binding.
        - Click the **Catalog offering** name to view the {{site.data.keyword.cloud_notm}} catalog list.
          
          Observe the Job logs that creates the set of namespaces, deployments for job runtime, replica sets, schematics job runner config maps and storage, trusted profiles, binds a cluster binding, deploys microservices by using your cluster, binds {{site.data.keyword.cloud_notm}} catalog offerings, and many more Terraform resources.
          {: note}

## Next steps
{: #agent-create-nextsteps}

When an agent is up and running, you can check out the following administration tasks:
1. Access Kubernetes dashboard to view the services that are created.
    - Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
    - Access **Kubernetes** > **Clusters**.
    - Click [...] three dots against your cluster name.
    - Click **Kubernete dashboard**.
    - Click the **Namespaces** drop down to view the set of namespaces by name schematics. For example, `schematics-agent-observe`, `schematics-job-runtime`, `schematics-runtime`, and so on.
    - In each namespaces, you can view the **Workloads** > **Deployments**, **Jobs**, **Pods**, **Replica Sets**, and **Cluster** binding information.

You can check out the [agent FAQ](/docs/schematics?topic=schematics-faqs-agent&interface=ui) for any common questions related to an agent.

Then, you can see the step to [delete an agent](/docs/schematics?topic=schematics-delete-agent-overview&interface=ui).
