---

copyright:
  years: 2017, 2025
lastupdated: "2025-01-17"

keywords: schematics agent deleting, deleting agent, agent deleting, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deleting an agent
{: #delete-agent-overview}

When an agent is no longer needed, you can delete the agent.



## Deleting an agent through UI
{: #delete-agentb1-ui}
{: ui}

1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials.
2. Navigate to **{{site.data.keyword.bpshort}}** > **Agents**.
3. Click the `...` dots > **Delete** agent.
4. In the Delete Agent pop-up window, type `<your agent name>` to confirm. **Note** the deleted agent action cannot be undone.
5. Click **Delete**.

## Deleting an agent through CLI
{: #delete-agentb1-cli}
{: cli}

You can delete an agent or uninstall an agent from the Kubernetes cluster by using an [agent delete](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-delete) command. This command requires an `AGENT_ID` as input argument.
{: shortdesc}

Example

```sh
ibmcloud schematics agent delete --id agent-ga-prod-cli-jan-10.soA.cd1c
```
{: pre}

Output

```text
Do you really want to delete the agent agent-ga-prod-cli-jan-10.soA.cd1c? [y/N]> y
Deleting Agent...
Agent agent-ga-prod-cli-jan-10.soA.cd1c deleted successfully
OK
```
{: screen}

When the agent is deleted, you can expect:

- All the agent microservices are uninstalled.
- All the agent related Kubernetes resources, such as Pods, Deployment, Configmap, and Namespaces are unavailable.
- All the credentials that are created as part of the agent deployment, such as HMAC credentials for the {{site.data.keyword.objectstorageshort}} bucket, trusted profile to communicate with {{site.data.keyword.bpshort}} are unavailable.
- All the information about the agent in the {{site.data.keyword.bpshort}} instance of your {{site.data.keyword.cloud_notm}} account is deleted.

When the agent is deleted, the following do not happen:

- The Kubernetes cluster is not destroyed. The cluster continues to incur costs until it is deleted. 
- The {{site.data.keyword.objectstorageshort}} instance and the {{site.data.keyword.objectstorageshort}} bucket are deleted.
- The agent assignment policy for the agent is not deleted.

## Deleting an agent through API
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

- You can view the [list of agents](/docs/schematics?topic=schematics-display-agentb1-overview&interface=cli), and the [agent assignment policy](/docs/schematics?topic=schematics-policy-manage&interface=cli).
- You can check out the [agent FAQ](/docs/schematics?topic=schematics-faqs-agent) for any common questions that are related to delete an agent.
