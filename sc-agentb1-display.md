---

copyright:
  years: 2017, 2025
lastupdated: "2025-01-17"

keywords: schematics agent displaying, displaying agent, agent displaying, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Displaying agents
{: #display-agentb1-overview}

You can view an agent by using the agent ID to retrieve the detailed configuration information. You can also list all the agents that are created in your account.
{: shortdesc}

## Displaying the agents through UI
{: #display-agentb1-get-ui}
{: ui}

1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials.
2. Click **{{site.data.keyword.bpshort}}** > **Agents**.
3. Click your agent from the list to view the agent details.

## Displaying the agents through CLI
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
agent-prod-testing-api-dec-24-2   agent-prod-testing-api-dec-24-2.deA.ca07   1.6.0     Create Agent                                  Default          us-south         eu-de                 Active   env:prod, mytest      
agent-testing-prod-cli-dec-27-2   agent-testing-prod-cli-dec-27-2.deA.727f   v1.0.0                                                  Default          us-south         eu-de                 Active                         
agent-testing-prod-cli-dec-27-3   agent-testing-prod-cli-dec-27-3.deA.fd13                                                           Default          us-south         eu-de                 Active                         
agent-testing-prod-cli-dec-27-4   agent-testing-prod-cli-dec-27-4.deA.acd4                                                           Default          us-south         eu-de                 Active                         
agent-testing-prod-cli-dec-27     agent-testing-prod-cli-dec-27.deA.3f7e                                                             Default          jp-tok         eu-de                 Active                         
gsmmar27v1cli-agent-test          gsmmar27v1cli-agent-test.deA.6288                                                                  Default          eu-de            eu-de                 Active                         
gsmmar27v2cli-agent-test          gsmmar27v2cli-agent-test.deA.afcc                                                                  Default          eu-de            eu-de                 Active                         
gsmmar27v3cli-agent-test          gsmmar27v3cli-agent-test.deA.4b56                                                                  Default          eu-de            eu-de                 Active                         
                                  
Showing 1-11 of 11 items
```
{: screen}

## Displaying agent configuration through CLI
{: #display-agentb1-get-cli}
{: cli}

You can use the [agent get](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-get) command to view the configuration of an agent. This command requires `agent_id` as an input argument.
{: shortdesc}

To view the agent get commands, syntax, and option flag details, see [ibmcloud schematics agent get](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-get).
{: important}

Example

```sh
ibmcloud schematics agent get --id agent-testing-prod-cli-dec-27-5.deA.dc97 
```
{: pre}

Output

```text
Retrieving agent...
OK
                    
ID               agent-testing-prod-cli-dec-27-5.deA.dc97   
Name             agent-testing-prod-cli-dec-27-5   
Status           ACTIVE   
Version          1.0.0   
Location         eu-de   
Agent Location   jp-tok   
Resource Group   Default   
                 
Recent Job   Job ID               Status                 Last modified   
DEPLOY       .ACTIVITY.465e9716   Triggered deployment   2023-03-27T12:25:01.239Z 
```
{: screen}

## Displaying agents through API
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

## Displaying agents through Terraform 
{: #display-agent-terraform}
{: terraform}

You can use the `ibm_schematics_agent` data source to fetch information about a {{site.data.keyword.bpshort}} Agent without changing your infrastructure.

Make sure the [{{site.data.keyword.bpshort}} Agent is deployed](/docs/schematics?topic=schematics-deploy-agent-overview&interface=terraform#create-agent-terraform) to list an existing agent.

1. Define `ibm_schematics_agent_deploy` resource in `main.tf` file.

    ```terraform
    data "ibm_schematics_agents" "schematics_agents" {
        name = "MyDevAgent"
    }
    ```
    {: codeblock}

2. Initialize

    ```sh
    terraform init
    ```
    {: pre}

3. Apply

    ```sh
    terraform apply
    ```
    {: pre}

You can check the [{{site.data.keyword.terraform-provider_full_notm}}](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/schematics_agent){: external} documentation for more parameters specific to the data source.
{: note}

## Next steps
{: #agent-delete-nextsteps}

- You can [update an agent](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-update), and [delete an agent](/docs/schematics?topic=schematics-delete-agent-overview&interface=cli)
- You can check out the [agent FAQ](/docs/schematics?topic=schematics-faqs-agent) for any common questions that are related to deleting an agent.
