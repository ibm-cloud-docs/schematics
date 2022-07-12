---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-12"

keywords: schematics agents connect, connect agent, register agent

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agents is a [Beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to, the list of [limitations for Agent](/docs/schematics?topic=schematics-agent-beta-limitations) in the Beta release.

# Connecting {{site.data.keyword.bpshort}} Agent
{: #register-agent}

You have successfully set up the {{site.data.keyword.bpshort}} Agents infrastructure and Agents services. The next step is to connect or register your Agent to your {{site.data.keyword.bpshort}} service instance. The diagram depicts the complete {{site.data.keyword.bpshort}} Agents set up flow.
{: shortdesc}

![{{site.data.keyword.bpshort}} Agents set up](images/agents-infra-setup.svg "{{site.data.keyword.bpshort}} Agents set up"){: caption=" " caption-side="center"}

## Connecting Agent through UI
{: #register-ui}
{: ui}

1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials.
2. Navigate to **{{site.data.keyword.bpshort}}** > **Agents**.
3. Select **Location** where you want to connect the Agent.
4. Click **Connect Agent**.
5. In the **Connect an Agent to {{site.data.keyword.bpshort}}** page, enter the input value.
    - **Agent name** - Enter the unique name.
    - **IAM Trusted ID** - Create a trusted ID and link the trusted ID. For more information, see [Trusted Profile ID](/docs/schematics?topic=schematics-agent-trusted-profile).
    - **Resource Group** - Select your resource group and specific resources where you need to connect a Agent. **Note** Check you have the right permissions for the resource group.
6. Click **Connect**.
    - The Agent status will change to **Ready to bind** status.
       The **Ready to bind** status signifies that the Agent is ready for the next step to bind the Workspace. Wait 15-30 minutes to view the Agent status.
       {: note}

7. Optional: From your Agent instance you can click the three dots to perform the following operations.
    - **Edit Agent** to edit the Agent configuration.
    - **Bind Agent** to the {{site.data.keyword.bpshort}} Workspaces.
    - **Pause Agent** to pause the Agent execution.
    - **Delete Agent** to delete a Agent.


## Connecting Agent through CLI
{: #register-cli}
{: cli}

Before your begin
- Setup your [CLI](/docs/schematics?topic=schematics-setup-cli).
- Install [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin).
- You have the right permission to create VPC infrastructure, Kubernetes cluster, and LogDNA services.

Here are the list of commands used to provision the Agent infrastructure.

- Run `ibmcloud schematics workspace new --file https://github.com/Cloud-Schematics/schematics-agents/tree/main/tarfiles/create_agent_infra_workspace.json` command to create the Agent infrastructure workspace.

## Connecting Agent through API
{: #register-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.
2. You have the right permission to create [VPC infrastructure](https://cloud.ibm.com/docs/vpc?topic=vpc-iam-getting-started), [IKS](https://cloud.ibm.com/docs/containers?topic=containers-access_reference) cluster, [LogDDNA](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-iam), and [Activity tracker](https://cloud.ibm.com/docs/activity-tracker?topic=activity-tracker-iam) services.

Here are the list of CURL commands use to register and unregister the Agent. For more information, about the Agents related APIs, refer to, [Agents APIs](cloud.ibm.com/apidocs/schematics/schematics#list-agent). 

1. Run 
   ```sh
    curl -X POST \
    https://schematics.cloud.ibm.com/v2/settings/agents \
    -H 'authorization: ' \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    -H 'github-token: github_token' \
    -d '{
        "name": "devclitestpart",
        "description": "Register agent",
        "resource_group": "Default",
        "tags": [
            "agent"
        ],
        "location": "<Enter your location>",
        "agent_location": "<Enter your agent location>",
        "profile_id": "<Enter you profile_id>",
        "user_state": {
            "state": "enable"
        }
    }'
    ```
    {: pre}

    For more information, about the Agent register, refer to, [ibmcloud schematics agent register](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-register).

2. Get all registered or unregistered Agents in the Account.

   ```sh
    curl -X GET \
    https://schematics.cloud.ibm.com/v2/settings/agents \
    -H 'authorization: ' \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    -H 'github-token: github_token' \
    ```
    {: pre}

   For more information, about the Agent register, refer to, [ibmcloud schematics agents list](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-list).

3. Get the registered Agent details by providing the Agent ID in path parameter.

    ```sh
    curl -X GET \
    https://schematics.cloud.ibm.com/v2/settings/agents/{agent_id} \
    -H 'authorization: ' \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    -H 'github-token: github_token' \
    ```
    {: pre}

    For more information, about the Agent register, refer to, [ibmcloud schematics agents get](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-get).

4. Optional: Update the Agent registration by providing the Agent ID in path parameter.

   ```sh
    curl -X PUT \
    https://schematics.cloud.ibm.com/v2/settings/agents/{agent_id} \
    -H 'authorization: \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    -d '{
        "name": "devclitestpart",
        "description": "Register agent description changet",
        "resource_group": "string",
        "tags": [
            "string"
        ],
        "location": "us-south",
        "agent_location": "us-south",
        "profile_id": "string",
        "user_state": {
            "state": "enable"
        }
    }'
    ```
   {: pre}

    For more information, about the Agent register, refer to, [ibmcloud schematics agents update](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agent-update).

5. Optional: Deregister the Agent by providing the Agent ID in path parameter.

   ```sh
    curl -X DELETE \
    https://schematics.cloud.ibm.com/v2/settings/agents/{agent_id} \
    -H 'authorization: ' \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    -d ''
    ```
   {: pre}

    For more information, about the Agent register, refer to, [ibmcloud schematics agents delete](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-agents-unregister).

## Next steps
{: #connect-nextsteps}

You have completed the Agent connection to your {{site.data.keyword.bpshort}} service instance.
- Now, you need to [Use an Agent](/docs/schematics?topic=schematics-register-agent&interface=ui) to bind the Agent to your Workspace, in order to run the IaC automation in your cluster.
- For any challenges in Agents installation or configuration, refer to, [FAQ about Agent](/docs/schematics?topic=schematics-faqs-agent&interface=cli) and [Troubleshooting guide](/docs/schematics?topic=schematics-agent-crn-not-found&interface=cli).
