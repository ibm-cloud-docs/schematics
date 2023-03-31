---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-31"

keywords: schematics agent displaying, displaying agent, agent displaying, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bplong_notm}} Agent beta-1 delivers a simplified agent installation process. You can review the [beta-1 release](/docs/schematics?topic=schematics-schematics-relnotes&interface=cli#schematics-mar2223) documentation and explore. 
{: attention}

{{site.data.keyword.bpshort}} Agent is a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta1-limitations) that is available for evaluation and testing purposes. It is not intended for production usage.
{: beta}

# Displaying agents
{: #display-agentb1-overview}

You can view an agent with a specific agent ID to retrieve the detailed configuration information. You can also list all the agents that are created in your account by using the command _agent list_.
{: shortdesc}

## Displaying the list of agents using UI
{: #display-agentb1-get-ui}
{: ui}

Currently, you can only create an {{site.data.keyword.bpshort}} Agent via CLI or API. Follow the steps to view the list of agents that are deployed in your account through [CLI](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli) and [API](/apidocs/schematics/schematics#create-agent-data).

   1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials.
   2. Navigate to **{{site.data.keyword.bpshort}}** > **Agents**.
   3. Select your Agent from the list, and use the `...` dots to perform **Delete Agent** operation.

## Displaying the list of agents using the CLI
{: #display-agentb1-list-cli}
{: cli}

You can display the list of agents in your account by using the [agent list](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-list) CLI command.
{: shortdesc}

Example

```sh
ibmcloud schematics agent list
```
{: pre}

Output

```text
Retrieving agents...
OK
Name                              ID                                         Version   Description                                   Resource Group   Agent Location   Schematics location   Status   Tags               Agent health   
Agent-UI-Final-Testing            Agent-UI-Final-Testing.deA.3dfb                                                                    cli-testing      eu-de                                  Active                         
Vishwa-CLI-Testing                Vishwa-CLI-Testing.deA.ef99                                                                        cli-testing      eu-de                                  Active                         
agent-5-multipod                  agent-5-multipod.deA.3d58                            Srikar testing multiple pods in agent-5 ...   job-runner       eu-de                                  Active   agent_register        
agent-prod-testing-api-mar-24-2   agent-prod-testing-api-mar-24-2.deA.ca07   1.6.0     Create Agent                                  Default          us-south         eu-de                 Active   env:prod, mytest      
agent-testing-prod-cli-mar-27-2   agent-testing-prod-cli-mar-27-2.deA.727f   v1.0.0                                                  Default          us-south         eu-de                 Active                         
agent-testing-prod-cli-mar-27-3   agent-testing-prod-cli-mar-27-3.deA.fd13                                                           Default          us-south         eu-de                 Active                         
agent-testing-prod-cli-mar-27-4   agent-testing-prod-cli-mar-27-4.deA.acd4                                                           Default          us-south         eu-de                 Active                         
agent-testing-prod-cli-mar-27     agent-testing-prod-cli-mar-27.deA.3f7e                                                             Default          jp-tok         eu-de                 Active                         
gsmmar27v1cli-agent-test          gsmmar27v1cli-agent-test.deA.6288                                                                  Default          eu-de            eu-de                 Active                         
gsmmar27v2cli-agent-test          gsmmar27v2cli-agent-test.deA.afcc                                                                  Default          eu-de            eu-de                 Active                         
gsmmar27v3cli-agent-test          gsmmar27v3cli-agent-test.deA.4b56                                                                  Default          eu-de            eu-de                 Active                         
                                  
Showing 1-11 of 11 items
```
{: screen}

## Displaying agent configuration using CLI
{: #display-agentb1-get-cli}
{: cli}

You can view the configuration of a single agent by using the [agent get](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agents-get) command. This command requires `agent_id` as an input argument.
{: shortdesc}

To view the agent get commands, syntax, and option flag details, see [ibmcloud schematics agent get](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agents-get).
{: important}

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
Agent Location   jp-tok   
Resource Group   Default   
                 
Recent Job   Job ID               Status                 Last modified   
DEPLOY       .ACTIVITY.465e9716   Triggered deployment   2023-03-27T12:25:01.239Z 
```
{: screen}

## Displaying agents using the API
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
    "agent_location": "jp-tok",
    "user_state": {
        "state": "enable",
        "set_by": "test@in.ibm.com",
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

The _agent get_ operation returns the agent configuration information based on your agent ID.

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
        "set_by": "test@in.ibm.com",
        "set_at": "2023-03-16T06:12:13.684097462Z"
    },
    "agent_crn": "crn:v1:bluemix:public:schematics:us-south:a/c19ef85117044059a3be5e45d6dc1cf6:347160c0-dca9-49e8-a292-9c980c7f8c47:agent:agent-beta1-testing.soA.748e",
    "created_at": "2023-03-16T06:12:13.684112846Z",
    "creation_by": "test@in.ibm.com",
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

## Next steps
{: #agent-delete-nextsteps}

- You can see [update an agent](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-update), and [delete an agent](/docs/schematics?topic=schematics-delete-agent-overview&interface=cli)
- You can check out the [agent FAQ](/docs/schematics?topic=schematics-faqs-agent) for any common questions related to deleting an agent.
