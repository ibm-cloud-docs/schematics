---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-16"

keywords: schematics agent deploying, deploying agent, agent deploy, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deploying agent
{: #deploy-agent-overview}

{{site.data.keyword.bpshort}} Agents is a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agent](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

{{site.data.keyword.bplong}} Agent extends {{site.data.keyword.bpshort}} ability to work directly with your private cloud infrastructure on your private network. 
{: shortdesc}

Deploying an agent involves the following steps:

- Configure your network so that your agent communicate with your {{site.data.keyword.cloud_notm}} infrastructure setup through [ibmcloud schematics agent create](docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-create).
- Deploys your agent by invoking [ibmcloud schematics agent plan](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-plan) and [ibmcloud schematics agent apply](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-apply) commands.
- Creates a trusted profile 
- Chooses a service endpoint that your agent uses to communicate with {{site.data.keyword.bpshort}} Workspace.
- Activates your agent.

The pre-requisite to deploy an Agent is as follows:

- Account owner or an Administrator of the {{site.data.keyword.iamlong}} enabled service 
- An existing [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters), [{{site.data.keyword.vpc_full}}](/docs/openshift?topic=openshift-cluster-create-vpc-gen2&interface=ui) clusters, or {{site.data.keyword.redhat_openshift_full}} cluster service access.
- A dedicated {{site.data.keyword.cloud_notm}} {{site.data.keyword.objectstorageshort}} bucket.
- Operator role to the {{site.data.keyword.bplong_notm}}.

## Deploying an agent through the CLI 
{: #deploy-agent-cli}
{: cli}

### Verifying an agent through the CLI
{: #verify-agent-create-ui}

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
    "name": "agent-beta1-testing",
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
        "cluster_id": "<enter your cluster ID>",
        "cluster_resource_group": "<enter your cluster resource group",
        "cos_instance_name": "<enter your COS instance name>",
        "cos_bucket_name": "<enter your COS bucket name>",
        "cos_bucket_region": "<enter your COS bucket region>",
        "cos_resource_group": "<enter your COS resource group>"

    },
    "user_state": {
        "state": "enable"
    }
}
```
{: codeblock}


### Verifying an agent through the API
{: #verify-agent-create-api}

Verify that the agent is created successfully as shown in the output.
{: shortdesc}

Output

```text
{
    "name": "agent-beta1-testing",
    "description": "Create Agent",
    "resource_group": "ba8818923fe846e6a8fb5c6d72075768",
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
        "set_at": "2023-03-16T06:12:13.684097462Z"
    },
    "agent_crn": "crn:v1:bluemix:public:schematics:us-south:a/c19ef85117044059a3be5e45d6dc1cf6:347160c0-dca9-49e8-a292-9c980c7f8c47:agent:agent-beta1-testing.soA.748e",
    "id": "agent-beta1-testing.soA.748e",
    "created_at": "2023-03-16T06:12:13.684112846Z",
    "creation_by": "geetha_sathyamurthy@in.ibm.com",
    "updated_at": "0001-01-01T00:00:00Z",
    "system_state": {
        "status_code": "draft"
    },
    "agent_kpi": {}
}
```
{: screen}

## Next steps
{: #agent-create-nextsteps}

The next step is to delete an agent.
