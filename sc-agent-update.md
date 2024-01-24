---

copyright:
  years: 2017, 2024
lastupdated: "2024-01-24"

keywords: schematics agent deploying, deploying agent, agent deploy, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Updating agents
{: #update-agent-overview}

Update an agent configuration in the currently selected {{site.data.keyword.bpshort}} region to work directly in your cloud infrastructure. Updating an agent does not revalidate or redeploy your agent. Select the right agent version to update. You can analyze the activity logs and recover the update.
{: shortdesc}

Following are the scenarios you must use agent upgrade.

- To incorporate the issues, features, or vulnerable images that are released by {{site.data.keyword.bpshort}}. For example, you are using agent version is `1.0.0`. If {{site.data.keyword.bpshort}} releases `1.0.x` version, you can use agent update to upgrade `v1.0.0` - `v1.1.1`.
- To update an agent metadata such as `name`, `description`, `tags`, `resource group`, `version`, and `agent_metadata` attributes.
- You can use `agent update` to revoke the updated version to its existing version.

## Before you begin
{: #update-prereq}

Review and select your agent version to update.
{: shortdesc}

## Updating an agent definition
{: #update-agent-ui}
{: ui}

Update your agent configuration by choosing the cluster and {{site.data.keyword.cos_full_notm}} of your choice.

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Access **Schematics** > [**Agents**](https://cloud.ibm.com/schematics/agents){: external}.
    - Select your agent:
        - Select **Actions** > **Edit Agent**
        - You can edit the **Description**, **Cluster**, **COS instance name**, **COS bucket name**, or **COS bucket region** as in the requirement.
3. Click ***Update and validate** to validate the cluster and {{site.data.keyword.cos_full_notm}} configuration.
4. Click **Deploy** to redeploy an agent.

## Creating an agent definition by using the CLI 
{: #update-agent-cli}
{: cli}

Select the {{site.data.keyword.cloud_notm}} region where you want to update and manage your agent. Set the [CLI region command](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) by running `ibmcloud target -r <region>`. Select the same region as the `location` specified on the `agent create` command. The {{site.data.keyword.cos_full_notm}} bucket location must be of the form `eu-gb` or `us-south` and not a city name. For more information, see [agent update](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-update) command.

Example

```sh
ibmcloud schematics agent update --id AGENT_ID --location <us-south> --agent-location <us-south> --version <1.0.0> --infra-type <ibm_kubernetes> --cluster-id <cg3fgvad0dak571xxx> --cluster-resource-group <Default> --cos-instance-name <agent-cos-instance> --cos-bucket <agent-cos-bucket> --cos-location <us-east> --resource-group <Default>
```
{: pre}


## Verifying agent update
{: #verify-agent-update-cli}
{: cli}

```sh
ibmcloud schematics agent get --id agent-ga-prod-cli-jan-10.soA.cd1c
```
{: pre}


## Updating an agent by using the {{site.data.keyword.bpshort}} API
{: #update-agent-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to update an IAM access token and authenticate with {{site.data.keyword.bpshort}} through the API. For more information, see [Update an agent](/apidocs/schematics/schematics#update-agent-data).

You can use the refresh_token to get a new IAM access token if you IAM token is expired.
{: important}

Example

```json
  curl -X PUT https://schematics.cloud.ibm.com/v2/agents/{agent_id} \-H 'Authorization: Bearer <Auth Key>' -H 'X-Feature-Agents: true' -H 'refresh_token: <refresh_token> ' -d '{
  "name": "AgentName",
  "description": "New Description",
  "resource_group": "Default",
  "tags": [
  "tag1",
  "tag2"
  ],
  "version": "v1.0.0",
  "schematics_location": "us-south",
  "agent_location": "us-south",
  "agent_infrastructure": {
  "infra_type": "ibm_kubernetes",
  "cluster_id": "cluster_id",
  "cluster_resource_group": "Default",
  "cos_instance_name": "blueprint_basic",
  "cos_bucket_name": "sample_bucket_name",
  "cos_bucket_region": "us-east"
  },
  "agent_inputs": [
  {
  "name": "ibmcloud_api_key",
  "value": "<api_key of the account where cluster and cos are present>",
  "metadata": {
  "secure": true
  }
  },
  {
  "name": "ansible_pull_ibmcloud_api_key",
  "value": "jenkins api_key for pulling agents images",
  "metadata": {
  "secure": true
  }
  },
  {   
  "name": "devops_api_key",
  "value": "api_key where you want to create agent and run fvts",
  "metadata": {
  "secure": true
  }
  }
  ],
  "user_state": {
  "state": "enable"
  }
  }'
```
{: codeblock}

Verify that the agent definition is created successfully as shown in the output. Record the agent ID for use in subsequent commands. For example, `agentb1-gsmforvpc.soA.115c`.

Now, run the `agent deploy` API with the `agent ID` to update the {{site.data.keyword.bpshort}} workspace that deploys the agent. The `agent deploy` operation starts both the `agent validate`, and `agent deploy` operations to setup the agent.

Syntax

```json
  PUT /v2/agents/<enter your agentID>/deploy HTTP/1.1
  Host: schematics.cloud.ibm.com
  Content-Type: application/json
  Authorization: Bearer 
```
{: codeblock}