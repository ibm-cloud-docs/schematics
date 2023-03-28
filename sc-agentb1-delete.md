---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-28"

keywords: schematics agent deleting, deleting agent, agent deleting, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deleting an agent
{: #delete-agent-overview}

{{site.data.keyword.bpshort}} Agents is a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

When an agent is no longer required, you can do one of the following to either cordon off the agent or uninstall the agent.
- Disable the agent 
- Delete the agent 

## Disabling an agent using the CLI
{: #disable-agentb1-cli}
{: cli}

You can disable or cordon off an agent using the [agent update](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-update) command. This command requires an `AGENT_ID` as input argument, and the `USER_STATE` as **disable**. Once the agent is disabled, the workspace or action jobs are not routed to that agent, the existing jobs runs to completion. The agent assignment policy for the  agent is automatically disabled.

Example

```sh
ibmcloud schematics agent update --id agent-testing-prod-cli-mar-27-5.deA.dc97 -s disable
```
{: pre}

Output

```text
Initiating agent update...
Job ID	.ACTIVITY.4654016
```
{: screen}

## Deleting an agent using the CLI
{: #delete-agentb1-cli}
{: cli}

You can delete an agent or uninstall an agent from the Kubernetes cluster by using an [agent delete](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-delete) command. This command requires an `AGENT_ID` as input argument.
{: shortdesc}

Example

```sh
ibmcloud schematics agent delete --id agent-testing-prod-cli-mar-27-5.deA.dc97
```
{: pre}

Output

```text
Do you really want to delete the agent agent-testing-prod-cli-mar-27-5.deA.dc97? [y/N]> y
Deleting Agent...
Agent agent-testing-prod-cli-mar-27-5.deA.dc97 deleted successfully
OK
```
{: screen}

When the agent is deleted, you can expect:
- All the agent microservices are uninstalled.
- All the agent related Kubernetes resources, such as Pods, Deployment, Configmap, and Namespaces are destroyed.
- All the credentials that are created as part of the agent deployment, such as HMAC credentials for the {{site.data.keyword.objectstorageshort}} bucket, trusted profile to communicate with {{site.data.keyword.bpshort}} are destroyed.
- All the information about the agent in the {{site.data.keyword.bpshort}} instance, of your {{site.data.keyword.cloud_notm}} account are deleted.

When the agent is deleted, the following will not happen:
- The Kubernetes cluster will not be destroyed.
- The {{site.data.keyword.objectstorageshort}} instance and the {{site.data.keyword.objectstorageshort}} bucket will not be destroyed.
- The agent assignment policy for the agent is not deleted.

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

## Next steps
{: #agent-delete-nextstep}

- You can continue to view the [list of agents](/docs/schematics?topic=schematics-display-agentb1-overview&interface=cli), and the [agent assignment policy](/docs/schematics?topic=schematics-policy-manage&interface=cli).
- You can check out the [agent FAQ](/docs/schematics?topic=schematics-faqs-agent) for any common questions related to deleting an agent.
