---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-20"

keywords: schematics agent deleting, deleting agent, agent deleting, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deleting agent
{: #delete-agent-overview}

{{site.data.keyword.bpshort}} Agents is a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

When an agent is no longer required, it can be deleted which will terminate billing for all deployed resources. Deleting an agent is a two-step process that first destroys all the associated cloud resources (environment) and second deletes an agent in {{site.data.keyword.bpshort}}.
{: shortdesc}

## Deleting an agent using the CLI
{: #delete-agentb1-cli}
{: cli}

To view the agent delete by using the CLI, use the `ibmcloud schematics agent delete` command. This command requires `agent_id` arguments. For the agent delete command, syntax, and option flag details, see [agent delete](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-delete) command.
{: shortdesc}

Before deleting an agent or a workspace created during agent deployment, you need to destroy the resources of the workspace. For more information about workspace destroy, see [deleting workspace](/docs/schematics?topic=schematics-workspace-setup#del-workspace).
{: important}

```sh
ibmcloud schematics agent delete --id AGENT_ID 
```
{: pre}


## Deleting an agent using the API
{: #delete-agentb1-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. 

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
    "requestid": "9dc76486-ec80-4a26-b159-6e2d46eeb39f",
    "timestamp": "2023-03-16T21:13:20.244850864Z",
    "messageid": "M1074",
    "message": "The requested object cannot be located. Check that the ID is correct and try your request again.",
    "statuscode": 404
}
```
{: screen}
