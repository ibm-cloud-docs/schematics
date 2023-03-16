---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-16"

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

### Verifying an agent deletion 
{: #verify-agentb1-delete-cli}

The delete CLI command does not wait for successful job completion and returns immediately. 

The status of the delete operation can be monitored by using the `agent job get` command. The following command runs a `agent job get` for the JOB ID. The job ID is displayed in the delete output. 

## Deleting an agent config using the UI 
{: #delete-agentb1-ui}
{: ui}


### Verifying agent deletion 
{: #verify-agentb1-deletion-ui}

## Deleting an agent using the API
{: #delete-agentb1-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. 

Syntax

```curl
curl -X DELETE https://us-east.schematics.cloud.ibm.com/v2/agents/<enter your agent ID> \
    -H 'Authorization: <enter your authorization token>' \
    -H 'X-Feature-Agents: true' \
    -H 'refresh_token: <enter your refresh token>'
```
{: codeblock}

```curl
curl -X DELETE https://us-east.schematics.cloud.ibm.com/v2/agents/agent-beta1-testing.soA.748e \
    -H 'Authorization: ' \
    -H 'X-Feature-Agents: true' \
    -H 'refresh_token: '
```
{: codeblock}


### Verifying agent deletion through API
{: #verify-agentb1-deletion-api}

You can retrieve the agent ID that are deleted by using _agent get_ API. 
{: shortdesc}

Syntax

```json
GET /v2/agents/<agent ID>/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>

```
{: codeblock}

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
