---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-29"

keywords: schematics agent, agent policy, policies

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bplong_notm}} Agent [beta-1](/docs/schematics?topic=schematics-schematics-relnotes&interface=cli#schematics-mar2223) release delivers a simplified Agents installation process.
{: attention}

{{site.data.keyword.bpshort}} Agent are a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta1-limitations) that are available for evaluation and testing purposes. It is not intended for production usage.
{: beta}



# Managing agent assignment policy
{: #policy-manage}

Agents for {{site.data.keyword.bplong}} extends its ability to work directly with your cloud infrastructure on your private network or in any network isolation zones. You can deploy multiple agents in your {{site.date.keyword.cloud_notm}} account, each catering to the different network isolation zones. For example, based on the following factory your cloud infrastructure can be spread across or partitioned.
- multiple cloud regions (region-1, region-2, region-3)
- multiple VPC zones for the application layer, data layer, management layer
- multiple cloud-vendors or on-premises vendors, or
- multiple department-wise information technology zones, in your organization such as `HR`, `Finance`, `Manufacturing`, and so on.
{: shortdesc}

The agents are deployed in each partition or network isolation zone in order to, run the
Terraform or Ansible automation, for the local or private Cloud resources. The agent
assignment policy is used by {{site.data.keyword.bpshort}} to dynamically route the workspace job or an action job to the agent.

You can create, update, and delete the `agent assignment policy` by using the {{site.data.keyword.bpshort}} CLI for [agent policy commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-policy-create).

The `agent-assignment-policy` for an agent is defined by using the following attributes of the
workspace or an action. The `selector` attribute can be a combination of the following flags.
- `tags` – workspaces or actions with the matching tags are selected.
- `location` – workspaces or actions with the matching location are selected.
- `resource-group` - workspaces or actions with the matching resource-group are selected.

If the selector for `agent-1` selects tags=[`dev`], resource-group=[`rg-2`]
{{site.data.keyword.bpshort}} automatically routes the workspace jobs such as Git download, Terraform
plan, apply, destroy jobs of all the workspaces that matches the `tags`, and `resource-group` criteria to the `agent-1`.
{: example}




## Creating an agent policy using CLI
{: #agentb1-createpolicy-cli}
{: cli}

Create your agent policy by using the CLI. For the complete list of an agent policy command with the options, see [policy commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-policy-create) command.
{: shortdesc}

Before your begin:

- Install or update the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version that is greater than the `1.12.8`.
- Select the {{site.data.keyword.cloud_notm}} region that you wish to use to manage your {{site.data.keyword.bpshort}}. Set the region by running [`ibmcloud target -r <region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command.
- Check that you have the [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create agent policy.

Example

```sh
ibmcloud schematics policy create --name agent-policy-testing-cli-mar-27 --kind agent_assignment_policy --location eu-de --resource-group Default --tags workspace-policy:prod
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

## Displaying the list of policy using CLI
{: #agentb1-listpolicy-cli}
{: cli}

You can dispaly the list of policy in your account by using the [policy list](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-policy-list) command.

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
policy-067dfb28-928b-4e90-ad2b-9d26343a1ceb   policy-067dfb28-928b-4e90-ad2b-9d26343a1ceb.deP.796d   Created agent-assignment-policy for the ...             
policy-08aa34da-d89d-4f9c-92d5-bf9b9c1624b0   policy-08aa34da-d89d-4f9c-92d5-bf9b9c1624b0.gbP.c061   Created agent-assignment-policy for the ...             
policy-0d47dbea-264b-43c1-8c44-d519d9a04df1   policy-0d47dbea-264b-43c1-8c44-d519d9a04df1.deP.efb1   Created agent-assignment-policy for the ...             
policy-110e7b82-26cc-4a0a-9594-4fdd41a7487d   policy-110e7b82-26cc-4a0a-9594-4fdd41a7487d.deP.0f96   Created agent-assignment-policy for the ...             
policy-203863db-46ef-422f-aba7-62520ec36dfc   policy-203863db-46ef-422f-aba7-62520ec36dfc.deP.050a   Created agent-assignment-policy for the ...             
policy-39b076a0-34f2-4f76-9562-cecfd1822353   policy-39b076a0-34f2-4f76-9562-cecfd1822353.deP.df8e   Created agent-assignment-policy for the ...             
policy-4a480058-1851-4a06-8700-e31eef7fbeac   policy-4a480058-1851-4a06-8700-e31eef7fbeac.gbP.f473   Created agent-assignment-policy for the ...             
policy-5613f1fb-03bc-47db-94e7-12aed2243828   policy-5613f1fb-03bc-47db-94e7-12aed2243828.gbP.cec6   Created agent-assignment-policy for the ...             
policy-5e8c1128-9dbe-4a57-9e94-1862bd2db5b8   policy-5e8c1128-9dbe-4a57-9e94-1862bd2db5b8.gbP.2550   Created agent-assignment-policy for the ...             
policy-61437bd4-0b7b-439d-b8a3-5ff091de5a01   policy-61437bd4-0b7b-439d-b8a3-5ff091de5a01.gbP.b393   Created agent-assignment-policy for the ...             
policy-6da22456-6cce-4c44-a397-a8a677377d96   policy-6da22456-6cce-4c44-a397-a8a677377d96.gbP.338c   Created agent-assignment-policy for the ...             
policy-7b699887-5c97-4795-8dba-6158f6674944   policy-7b699887-5c97-4795-8dba-6158f6674944.deP.45c7   Created agent-assignment-policy for the ...             
policy-7cb53b8a-1f65-402d-adeb-4d4863978d05   policy-7cb53b8a-1f65-402d-adeb-4d4863978d05.deP.9b71   Created agent-assignment-policy for the ...             
policy-a1c11bcb-c550-4d84-bd34-e8323b2856eb   policy-a1c11bcb-c550-4d84-bd34-e8323b2856eb.deP.74c2   Created agent-assignment-policy for the ...             
policy-bf94e2e4-89f2-4fe8-b4c7-c809507bbcb2   policy-bf94e2e4-89f2-4fe8-b4c7-c809507bbcb2.gbP.dd2e   Created agent-assignment-policy for the ...             
policy-e53a1fa9-6393-48ec-8566-84288770072b   policy-e53a1fa9-6393-48ec-8566-84288770072b.gbP.020d   Created agent-assignment-policy for the ...             
policy-ed3c4ebb-6df5-4898-9070-fcdb1af55778   policy-ed3c4ebb-6df5-4898-9070-fcdb1af55778.deP.1809   Created agent-assignment-policy for the ...             
policy-f60ca5c5-d7d3-4b1c-91de-7d8e04b6c4b3   policy-f60ca5c5-d7d3-4b1c-91de-7d8e04b6c4b3.deP.808e   Created agent-assignment-policy for the ...             
policy-fd7349d5-de33-4fbc-87de-3be75018271a   policy-fd7349d5-de33-4fbc-87de-3be75018271a.deP.08dc   Created agent-assignment-policy for the ...             
                                                                                                     
Showing 1-29 of 29 items
```
{: screen}

## Displaying an policy using CLI
{: #agentb1-getpolicy-cli}
{: cli}

You can view the configuration of a single agent policy by using the [policy get](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-policy-get) command.

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


## Updating an agent policy using CLI
{: #agentb1-updatepolicy-cli}
{: cli}

You can update an agent policy to set a tags, description based on the `AGENT_ID` input argument.

```sh
ibmcloud schematics policy update --id agent-policy-testing-cli-mar-27.deP.c737 --kind agent_assignment_policy --resource-group Default --tags workspace-policy:prod --description testing-policy-cli --tags newtag
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
                  - newtag
```
{: screen}

You can retrieve an updated policy to observe the updation.

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
                  - newtag	   
                    
```
{: screen}

## Deleting an policy using CLI
{: #agentb1-deletepolicy-cli}
{: cli}

You can [delete a policy](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-policy-delete) by using `AGENT_ID` input argument.

```sh
ibmcloud schematics policy delete --id agent-policy-testing-cli-mar-27.deP.c737 
```
{: pre}

```sh
Do you really want to delete the policy? [y/N]> y
Initiating policy delete...
```
{: screen}

## Agent policy command using API
{: #agentb1-policydm-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, about agent policy API, see [agent policy APIs](/apidocs/schematics/schematics) job status.
{: shortdesc}

### Example to create policy using API
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

### Example to get policy using API
{: #agentb1-getpolicy-api}

```json
GET /v2/settings/policies/<your policy_id> HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
X-ENABLE-POLICIES: true
Authorization: Bearer <auth_token>
```
{: pre}

### Example to update policy using API
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

### Example to search policy using API
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
