---

copyright:
  years: 2017, 2023
lastupdated: "2023-12-07"

keywords: schematics agent, agent policy, policies

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Agent policies
{: #policy-manage}

Agent (assignment) policies tell {{site.data.keyword.bpshort}} which agent to use to run Terraform and Ansible jobs in a specific network zone. Each agent has one or more policies associated with it to identify the workspace and action jobs that are run on the agent. For example agents may exist in and jobs can be executed in the following isolated zones:

- cloud regions (region-1, region-2, region-3)
- VPC zones for the application layer, data layer, management layer
- cloud-vendors or on-premises
- departmental zones in your organization such as `HR`, `Finance`, `Manufacturing`
{: shortdesc}

Only a single policy can be associated with a workspace or action. Policy creation fails if there is an existing policy that targets the same workspaces or actions.  

You can create, update, and delete an `agent assignment policy` by using the {{site.data.keyword.bpshort}} [policy commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-policy-create) CLI. 

The `agent-assignment-policy` for an agent is defined by using the following attributes of a workspace or action. The selection attributes can be a combination of the following flags:

- `tags` – workspaces or actions with matching user tags are selected.
- `locations` – workspaces or actions in the matching {{site.data.keyword.bpshort}} location are selected.
- `resource-groups` - workspaces or actions with the matching resource-group are selected.

If the selection policy for `agent-1` specified tags=[`dev`] and resource-group=[`rg-2`], {{site.data.keyword.bpshort}} automatically routes workspace jobs including Git download, Terraform plan, apply, destroy jobs for all workspaces that match the `tags`, and `resource-group` criteria, to execute on `agent-1`.
{: example}





## Creating an agent policy using the UI
{: #agentb1-createpolicy-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Access **Schematics** > **Policies** > [**Create policy**](https://cloud.ibm.com/schematics/policies){: external}.
    - In **Create a policy** section:
        - Enter unique **Policy name**.
        - Enter **Description**.
        - Select **Policy type** as Agent assignment policy.
        - Select **Location**, and **Resource group** from the drop down option.
        - Enter **Tags** for the agent.
        - Click **Next**.
    - In **Policy parameters** section:
        - Select your **Agent** from the drop down list.
        - In **Define policy attributes** section.
          - Select **Object type** as `workspace` or `action`.
          - Select **Resource group**.
          - Select **Object location**.
          - Enter **Object tags**.
          - Click **Next**.
        - In the **Policy preview** section:
            - Select the workspaces that needs to be part of your policy.
3. Click **Create**.

## Listing all policies using the UI
{: #agentb1-listpolicy-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Access **Schematics** > **Policies**.

## Displaying a policy using the UI
{: #agentb1-getpolicy-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Access **Schematics** > **Policies**.
3. Click your policy from the list to view the policy details.
4. In the **Assigned agent** window, click **Agent details** to view your agent configurations.

## Updating an agent policy using the UI
{: #agentb1-updatepolicy-ui}
{: ui}

You can update an agent policy to change the selection tags, or description, by referencing the agent with the `AGENT_ID`.

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Access **Schematics** > **Policies**.
3. Click your policy from the list to view the policy details.
4. Click **Actions** > **Edit policy** to update the parameters.

## Deleting a policy using the UI
{: #agentb1-deletepolicy-ui}
{: ui}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Access **Schematics** > **Policies**.
3. Click your policy from the list to view the policy details.
4. Click **Actions** > **Delete policy** to delete the parameters.

## Creating an agent policy using the CLI
{: #agentb1-createpolicy-cli}
{: cli}

Create your agent policy using the CLI. For the complete list of agent policy options, see the [policy commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-policy-create) doc.
{: shortdesc}

Before you begin:

- Install or update the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version to be `1.12.12` or higher.
- Select the {{site.data.keyword.cloud_notm}} region where the agent is defined. Set the CLI region by running [`ibmcloud target -r <region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command.
- Check that you have the [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create an agent policy.
- Create an agent policy file 

### Defining a JSON policy file
{: #agent-policy-json}

A sample JSON policy file is provided here. Replace the `<...>` placeholders with your actual values. 

- The agent jobs are to be run on is defined by using the `target` block.
- The attributes to select the workspace or actions to run on the agent are defined by the `parameter` block. 

Policy JSON files can be edited in any editor or IDE. They must be valid JSON.  
 

**Policy JSON file syntax:**
```json
{
    "target": {
		"selector_kind": "ids",
		"selector_ids": [
			"<agent id>"
		]
	},
	"parameter": {
		"agent_assignment_policy_parameter": {
			"selector_kind": "scoped",
			"selector_scope": [{
				"kind": "workspace",
				"tags": [
					"<user_tag>"
				],
				"resource_groups": [
					"<resource_group>"
				],
				"locations": [
					"<region>"
				]
			}]
		}
	}
}

```
{: codeblock}

Example

```json
{
    "target": {
		"selector_kind": "ids",
		"selector_ids": [
			"agent-prod-live.deA.e055"
		]
	},
	"parameter": {
		"agent_assignment_policy_parameter": {
			"selector_kind": "scoped",
			"selector_scope": [{
				"kind": "workspace",
				"tags": [
					"live-prod"
				],
				"resource_groups": [
					"Default"
				],
				"locations": [
					"eu-de"
				]
			}]
		}
	}
}
```
{: codeblock}

### Create agent policy
{: #agent-policy-CLI}


Example

```sh
ibmcloud schematics policy create --name agent-policy-testing-cli-mar-27 --kind agent_assignment_policy --location eu-de --resource-group Default --target-file policy.json
```
{: pre}

Output

```text
Creating policy...
                    
ID               agent-policy-testing-cli-mar-27.deP.c737   
Name             agent-policy-testing-cli-mar-27   
Description         
Kind             agent_assignment_policy   
Location         eu-de   
Resource Group   aac37f57b20142dba1a435c70aeb12df   
Target              
Tags             [TAGS]   
                  - workspace-policy:prod
```
{: screen}

## Listing all policies using the CLI
{: #agentb1-listpolicy-cli}
{: cli}

You can display the list of policies defined in your account using the [policy list](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-policy-list) command.

Example

```sh
ibmcloud schematics policy list
```
{: pre}

Output

```text
Retrieving policies...
OK
Name                                          ID                                                     Description                                   Kind   Tags   
agent-policy-testing-cli-mar-27               agent-policy-testing-cli-mar-27.deP.c737                                                                    workspace-policy:prod   
policy-023e7204-c33d-49b8-a9f3-695ff085290d   policy-023e7204-c33d-49b8-a9f3-695ff085290d.gbP.8b3c   Created agent-assignment-policy for the ...             
policy-067dfb28-928b-4e90-ad2b-9d26343a1ceb   policy-067dfb28-928b-4e90-ad2b-9d26343a1ceb.deP.796d   Created agent-assignment-policy for 

                                                                                                     
Showing 1-3 of 3 items
```
{: screen}

## Displaying a policy using the CLI
{: #agentb1-getpolicy-cli}
{: cli}

You can view the configuration of a single agent policy with the [policy get](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-policy-get) command.

Example

```sh
ibmcloud schematics policy get agent-policy-testing-cli-mar-27.deP.c737
```
{: pre}

Output

```text
Enter id> agent-policy-testing-cli-mar-27.deP.c737
Retrieving policy...
                    
ID               agent-policy-testing-cli-mar-27.deP.c737   
Name             agent-policy-testing-cli-mar-27   
Description         
Kind             agent_assignment_policy   
Location         eu-de   
Resource Group   aac37f57b20142dba1a435c70aeb12df   
Target              
Tags             [TAGS]   
                  - workspace-policy:prod
```
{: screen}


## Updating an agent policy using the CLI
{: #agentb1-updatepolicy-cli}
{: cli}

You can update an agent policy to change the selection tags, or description, by referencing the agent with the `AGENT_ID` input argument.

```sh
ibmcloud schematics policy update --id agent-policy-testing-cli-mar-27.deP.c737 --kind agent_assignment_policy --resource-group Default --tags workspace-policy:prod --description testing-policy-cli --tags jobtag
```
{: pre}

```sh
Updating policy...
                    
ID               agent-policy-testing-cli-mar-27.deP.c737   
Name             agent-policy-testing-cli-mar-27   
Description      testing-policy-cli   
Kind             agent_assignment_policy   
Location         eu-de   
Resource Group   Default   
Target              
Tags             [TAGS]   
                  - workspace-policy:prod	   
                  - jobtag
```
{: screen}

After updating, retrieve the policy to confirm the changes. 

Example

```sh
ibmcloud schematics policy get --id agent-policy-testing-cli-mar-27.deP.c737
```
{: pre}

Output

```text
Retrieving policy...
                    
ID               agent-policy-testing-cli-mar-27.deP.c737   
Name             agent-policy-testing-cli-mar-27   
Description      testing-policy-cli   
Kind             agent_assignment_policy   
Location         eu-de   
Resource Group   Default   
Target              
Tags             [TAGS]   
                  - workspace-policy:prod	   
                  - jobtag	   
                    
```
{: screen}

## Deleting a policy using the CLI
{: #agentb1-deletepolicy-cli}
{: cli}

You can [delete a policy](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-policy-delete), passing the `AGENT_ID` input argument.

```sh
ibmcloud schematics policy delete --id agent-policy-testing-cli-mar-27.deP.c737 
```
{: pre}

```sh
Do you really want to delete the policy? [y/N]> y
Initiating policy delete...
```
{: screen}

## Agent policy creation using the API
{: #agentb1-policydm-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information about agent policy API, see [agent policy APIs](/apidocs/schematics/schematics) job status.
{: shortdesc}

### Example to create a policy using the API
{: #agentb1-createpolicy-api}

```json
POST /v2/settings/policies HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>
{
    "name": "policy-1",
    "description": "Policy for job execution of secured workspaces on agent1",
    "resource_group": "Default",
    "tags": [
      "policy:secured-job"
    ],
    "location": "us-south",
    "kind": "agent_assignment_policy",
    "target": {
      "selector_kind": "ids",
      "selector_ids": [
        "agent5.8442"
      ]
    },
    "parameter": {
      "agent_assignment_policy_parameter": {
        "selector_kind": "scoped",
        "selector_scope": [
          {
            "kind": "workspace",
            "tags": [
              "env:dev",
              "k8s"
            ],
            "resource_groups": [
              "test"
            ],
            "locations": [
              "us-south"
            ]
          }
        ]
      }
    }
  }
```
{: pre}

### Example to get a policy using the API
{: #agentb1-getpolicy-api}

```json
GET /v2/settings/policies/<your policy_id> HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
X-ENABLE-POLICIES: true
Authorization: Bearer <auth_token>
```
{: pre}

### Example to update a policy using the API
{: #agentb1-updatepolicy-api}

```json
PATCH /v2/settings/policies/<your policy_id> HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
X-ENABLE-POLICIES: true
Authorization: Bearer <auth_token>
{
    "name": "policy-1",
    "description": "updated Policy for job execution of secured workspaces on agent1",
    "resource_group": "Default",
    "tags": [
      "policy:secured-job"
    ],
    "location": "us-south",
    "kind": "agent_assignment_policy",
    "target": {
      "selector_kind": "ids",
      "selector_ids": [
        "agent5.13a6"
      ]
    },
    "parameter": {
      "agent_assignment_policy_parameter": {
        "selector_kind": "scoped",
        "selector_scope": [
          {
            "kind": "action",
            "tags": [
              "env:dev",
              "k8s"
            ],
            "resource_groups": [
              "dummy_resource_group"
            ],
            "locations": [
              "us-south"
            ]
          }
        ]
      }
    }
  }
```
{: pre}

### Example to find policies using the API
{: #agentb1-searchpolicy-api}

```json
POST /v2/settings/policies/search HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>
{
   "parameter": {
            "kind": "workspace",
            "tags": [
              "env:dev",
              "k8s"
            ],
            "resource_groups": [
              "test"
            ],
            "locations": [
              "us-south"
            ]
          },
    "target": "action" 
  }
```
{: pre}


You can now use the agent to run {{site.data.keyword.bpshort}} Terraform or Ansible jobs. The agent executes any jobs for workspaces or actions that match the defined selection policy parameters:

- resource group
- location
- tags

Note now, tags must be set at workspace or action create time. Any changes to tags performed via the {{site.data.keyword.bpshort}} UI will not be detected or considered during policy evaluation. 
{: attention}

After execution, the workspace or action job logs contain a header indicating the agent that the job was executed on.  

```text
2023/04/08 15:22:07 [1m-----  New Workspace Action  -----[21m[0m
2023/04/08 15:22:07 Request: activitId=e3fcfdfdb13b07a1c60176e4b95c41ba, account=, owner=steve_strutt@uk.ibm.com, requestID=0a8a3428-b461-4dd0-8104-e859d68d35f6, OrchestratorID=orchestrator-5c8585dc74-6z9s5, agentID=agent-test-da.deA.e055, agentName=agent-test-da, jobRunnerID=jobrunner-5d99b5cfb7-p5xcz
2023/04/08 15:22:07 Related Workspace: name=myworkspace, agentID=agent-test-da.deA.e055 sourcerelease=(not specified), sourceurl=https://github.com/stevestrutt/multitier-vpc-bastion-host, branch=(not specified), folder=.
2023/04/08 15:22:07  --- Ready to execute the command on Agent agent-test-da.deA.e055 ---
```
{: screen}

## Next steps
{: #agent-policy-nextsteps}

You can check out the [agent FAQ](/docs/schematics?topic=schematics-faqs-agent&interface=ui) for any common questions that are related to an agent.

When the agent is no longer required, it can be removed following the steps in [delete an agent](/docs/schematics?topic=schematics-delete-agent-overview&interface=ui).
