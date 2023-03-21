---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-21"

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

Example

```sh
ibmcloud schematics agent get --id gsmmar2cliv2-agent-test.deA.391b
```
{: pre}

Output

```text
Retrieving agent...
OK
                    
ID               gsmmar2cliv2-agent-test.deA.391b   
Name             gsmmar2cliv2-agent-test   
Status           ACTIVE   
Version             
Location         eu-de   
Agent Location   us-south   
Resource Group   Default   
                 
Recent Job   Job ID                             Status                             Last modified   
PRS          .ACTIVITY.64d87b58                 Triggered pre-requisite scanning   0001-01-01T00:00:00.000Z   
DEPLOY       .ACTIVITY.9f4e2a83                 Triggered deployment               0001-01-01T00:00:00.000Z   
HEALTH       852b6992c5aab753b159d3fa3204168e   Job Success                         2023-03-21T12:25:25.183Z 
```
{: screen}

Example to view the list of agents in your account.

```sh
ibmcloud schematics agent list
```
{: pre}

Output

```text
Retrieving agents...
OK
Name                      ID                                 Version   Description    Resource Group   Agent Location   Schematics location   Status   Tags               Agent health   
agent-harini-test-eu      agent-harini-test-eu.deA.509c                Create Agent   Default          us-south         eu-de                 Active   Env:stage, beta1      
gsmmar2cli-agent-test     gsmmar2cli-agent-test.deA.ea4f     0.0.1                    Default          eu-de            eu-de                 Active                         
gsmmar2cliv2-agent-test   gsmmar2cliv2-agent-test.deA.391b                            Default          us-south         eu-de                 Active                         
                          
Showing 1-3 of 3 items

```
{: screen}

## Displaying an agent using the API
{: #display-agentb1-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. 

Example to list all the agents in your account.

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

The _agent get_ displayed the detailed information based on your agent ID.

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
