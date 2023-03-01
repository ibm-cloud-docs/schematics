---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-01"

keywords: schematics agent planning, planning agent, agent planning, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Planning agent
{: #plan-agent-overview}

Planning to create an agent by using {{site.data.keyword.bpshort}}. 

Before you begin

Before you can use an agent, you must complete the following tasks:

- The agent needs {{site.data.keyword.containerlong_notm}} or {{site.data.keyword.redhat_openshift_full}} services with minimum three worker nodes, with a flavor of b4x16 or higher.
- You need to have administrator access, when you are accessing the resources such as {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.redhat_openshift_notm}}, {{site.data.keyword.cos_full_notm}}, and so on.
- You need full permission to access the {{site.data.keyword.bpshort}} Workspace from other {{site.data.keyword.cloud_notm}} account.

## Creating an agent through the CLI 
{: #create-agent-cli}
{: cli}

Follow the steps to create an agent. For a complete options list, see [agent create](/docs/schematics?topic=schematics-schematics-cli-reference&interface=ui#schematics-agent-create) command.
{: shortdesc}

Before your begin

- To work with {{site.data.keyword.bpshort}} Agent, the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version must be greater than the `1.12.7`.
- Install or update the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version that is greater than the `1.12.7`.

For all the agent commands, syntax, and option flag details, see the section [agent commands](/docs/schematics?topic=schematics-schematics-cli-reference#agent-cmd).
{: important}

Full `agent create` command syntax:

```sh
ibmcloud schematics agent create --name AGENT_NAME --location LOCATION --agent-location AGENT_LOCATION --version VERSION --infra-type INFRA_TYPE --cluster-id CLUSTER_ID --cluster-resource-group CLUSTER_RESOURCE_GROUP --cos-id COS_ID --cos-bucket COS_BUCKET --cos-location COS_LOCATION --resource-group RESOURCE_GROUP [--description DESCRIPTION] [--plan-only] [--plan-apply] [--tags TAGS] [--file FILE] [--output OUTPUT]
```
{: pre}

Example

```sh
ibmcloud schematics agent create command to be placed after testing.
```
{: pre}

### Verifying an agent through the CLI
{: #verify-agent-create-ui}

Follow the steps to verify an agent was created successfully.

```sh
ibmcloud schematics agent get --id <AGENT_ID>
```
{: pre}

Output

```text
Need to add the output after testing.
```
{: screen}

Here are the steps to verify your blueprint config creation was successful.
{: shortdesc}

## Creating an agent through the API
{: #create-agent-api}
{: cli}

### Verifying an agent through the API
{: #verify-agent-create-api}

## Next steps
{: #agent-create-nextsteps}

The next step is to deploy an agent.
