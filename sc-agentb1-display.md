---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-20"

keywords: schematics agent displaying, displaying agent, agent displaying, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Displaying agent
{: #display-agentb1-overview}

{{site.data.keyword.bpshort}} Agents is a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

When an agent is no longer required, it can be retrieved, analyzed to delete. You can view an agent with the specific agent ID to see the detailed information of an agent. You can also list all the agents that are created in your account by using _agent list_.
{: shortdesc}

## Displaying an agent using the CLI
{: #display-agentb1-cli}
{: cli}

To view the agent by using the CLI, use the `ibmcloud schematics agent get` command. This command requires `agent_id` arguments.
{: shortdesc}

To view the agent get commands, syntax, and option flag details, see [ibmcloud schematics agent get](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agents-get).
{: important}

```sh
ibmcloud schematics agent get --id AGENT_ID 
```
{: pre}


## Displaying an agent using the API
{: #display-agentb1-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. 

Example

```json
LIST /v2/agents/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>

```
{: codeblock}

Output

```text
{
    "name": "agent-beta1-testing",
    "description": "Create Agent",
    "resource_group": "schematics-prod",
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
    "created_at": "2023-03-16T06:12:13.684112846Z",
    "creation_by": "geetha_sathyamurthy@in.ibm.com",
    "updated_at": "0001-01-01T00:00:00Z",
    "system_state": {
        "status_code": "draft"
    },
    "agent_kpi": {
        "availability_indicator": "normal",
        "lifecycle_indicator": "consistent",
        "percent_usage_indicator": "30%"
    }
}
```
{: screen}

The _agent list_ get lists the detailed information with respect to an agent ID.

Example

```json
GET /v2/agents/agent-beta1-testing.soA.748e/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>

```
{: codeblock}

Output

```text
{
    "name": "agent-beta1-testing",
    "description": "Create Agent",
    "resource_group": "schematics-prod",
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
    "created_at": "2023-03-16T06:12:13.684112846Z",
    "creation_by": "geetha_sathyamurthy@in.ibm.com",
    "updated_at": "0001-01-01T00:00:00Z",
    "system_state": {
        "status_code": "draft"
    },
    "agent_kpi": {
        "availability_indicator": "normal",
        "lifecycle_indicator": "consistent",
        "percent_usage_indicator": "30%"
    }
}
```
{: screen}
