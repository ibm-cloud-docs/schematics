---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-23"

keywords: schematics agent deleting, deleting agent, agent deleting, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deleting an agent
{: #delete-agent-overview}

{{site.data.keyword.bpshort}} Agents is a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

When an agent is no longer required, it can be removed along with the cluster it is hosted on. 
{: shortdesc}

## Deleting an agent using the CLI
{: #delete-agentb1-cli}
{: cli}

To delete an agent using the CLI, use the `ibmcloud schematics agent delete` command. This command requires an `agent_id` as input argument. For the agent delete command, syntax, and option flag details, see [agent delete](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-delete) command.
{: shortdesc}

Before de an agent or the workspace created during agent deployment, you need to destroy the resources of the workspace. For more information about workspace destroy, see [deleting workspace](/docs/schematics?topic=schematics-workspace-setup#del-workspace).
{: important}

Example

```sh
ibmcloud schematics agent delete --id gsmmar2cliv2-agent-test.deA.391b 
```
{: pre}

Output

```text
Do you really want to delete the agent gsmmar2cliv2-agent-test.deA.391b? [y/N]> y
Deleting Agent...
Agent gsmmar2cliv2-agent-test.deA.391b deleted successfully
OK
```
{: screen}

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
