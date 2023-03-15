---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-15"

keywords: schematics agent deploying, deploying agent, agent deploy, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Deploying agent
{: #deploy-agent-overview}

{{site.data.keyword.bpshort}} Agents is a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

{{site.data.keyword.bplong}} Agent extends {{site.data.keyword.bpshort}} ability to work directly with your private cloud infrastructure on your private network. 
{: shortdesc}

Deploying an agent involves the following steps:

- Configure your network so that your agent communicate with your {{site.data.keyword.cloud_notm}} infrastructure setup through [ibmcloud schematics agent create](docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-create).
- Deploys your agent by invoking [ibmcloud schematics agent plan](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-plan) and [ibmcloud schematics agent apply](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-apply) commands.
- Creates a trusted profile 
- Chooses a service endpoint that your agent uses to communicate with {{site.data.keyword.bpshort}} Workspace.
- Activates your agent.

The pre-requisite to deploy an Agent is as follows:

- Account owner or an Administrator of the {{site.data.keyword.iamlong}} enabled service 
- An existing [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters), [{{site.data.keyword.vpc_full}}](/docs/openshift?topic=openshift-cluster-create-vpc-gen2&interface=ui) clusters, or {{site.data.keyword.redhat_openshift_full}} cluster service access.
- A dedicated {{site.data.keyword.cloud_notm}} {{site.data.keyword.objectstorageshort}} bucket.
- Operator role to the {{site.data.keyword.bplong_notm}}.

## Deploying an agent through the CLI 
{: #deploy-agent-cli}
{: cli}

### Verifying an agent through the CLI
{: #verify-agent-create-ui}

## Deploying an agent through the API
{: #create-agent-api}
{: api}

### Verifying an agent through the API
{: #verify-agent-create-api}

## Next steps
{: #agent-create-nextsteps}

The next step is to delete an agent.
