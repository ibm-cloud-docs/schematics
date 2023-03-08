---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-08"

keywords: schematics agent deleting, deleting agent, agent deleting, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deleting agent
{: #delete-agent-overview}

{{site.data.keyword.bpshort}} Agents is a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

When a agent is no longer required, it can be deleted which will terminate billing for all deployed resources. Deleting an agent is a two-step process that first destroys all the associated cloud resources (environment) and second deletes an agent in {{site.data.keyword.bpshort}}.
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

You can follow these steps to delete the {{site.data.keyword.bpshort}} Agent by using {{site.data.keyword.cloud_notm}} console.

### Verifying agent config deletion 
{: #verify-agentb1-deletion-ui}

## Deleting an agen using the API
{: #delete-agentb1-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. 

### Verifying agent delete using the API
{: #verify-agentb1-delete-api}
