---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-25"

keywords: schematics agent, agent policy, policies

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agent are a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that are available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for agent](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.
{: beta}

<hidden>

## Managing network policies
{: #agent-networkpolicy}

{{site.data.keyword.bpshort}} Agent contain the network policies to secure the runtime pods communication. It communicates with other pods and the external endpoints. The incoming and outgoing network traffic is allowed or restricted based on the protocol, port, source, and destination IP addresses.
{: shortdesc}

## Restricting properties
{: #networkpolicy-restrict}

The network policies restrict the traffic to the pods that contains the following properties.
- Legitimate pod-to-pod connections are allowed when explicitly allowed.
- When needed pod `egress` traffic is allowed.
{: shortdesc}

## Default network policies
{: #networkpolicy-default}

Following are the default properties that are applied with the {{site.data.keyword.bpshort}} Agent deployment.
{: shortdesc}

| Policy | Description |
| --- | --- |
| `Deny-all-sandbox` | `Namespace:schematics-sandbox`, denies all the `ingress` and `egress` traffic. |
| `Deny-all-runtime` | `Namespace:schematics-runtime`, denies all the `ingress` and `egress` traffic. |
| `Whitelist-sandbox` | `Namespace:schematics-sandbox`, allowed list, and needed ports for `ingress = 3000`, and for `egress TCP = 80, 443, 5986, 22, 53` or `egress UDP = 53, 443`.|
| `Whitelist-ingress-job-ports` | `Namespace:schematics-runtime`, allowed and needed ports for `ingress = 3002`.|
| `Whitelist-runtime-gen-ports` | `Namespace:schematics-runtime`, allowed and needed ports for `ingress = 3002`, and for `egress TCP = 80, 443, 5986, 22, 53, 8080, 10250, 9092, 9093` or `egress UDP = 53, 443, 10250, 9092, 9093`.|
{: caption="Default network policies" caption-side="bottom"}


# Managing agent assignment policy
{: #policy-manage}

Agents for {{site.date.keyword.bplong}} extend its ability to work directly with your cloud
infrastructure on your private network or in any network isolation zones. You can deploy
multiple agents in your {{site.date.keyword.cloud_notm}} account, each catering to the different network isolation zones. For example, based on the following factory your cloud infrastructure can be spread across or partitioned.
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




## Agent policy commands using CLI
{: #agentb1-policycmd-cli}
{: cli}

Create your agent policy with the CLI. For a complete listing of agent policy command with the options, see [policy commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-policy-create) command.
{: shortdesc}

Before your begin:

- Install or update the [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) version that is greater than the `1.12.7`.
- Select the {{site.data.keyword.cloud_notm}} region that you wish to use to manage your {{site.data.keyword.bpshort}}. Set the region by running [`ibmcloud target -r <region>`](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_target) command.
- Check that you have the [IAM permissions](/docs/schematics?topic=schematics-access#blueprint-permissions) to create agent policy.

### Example to create policy using CLI
{: #agentb1-createpolicy-cli}

```sh
ibmcloud schematics policy create --name POLICY_NAME --kind POLICY_KIND --location LOCATION --resource-group RESOURCE_GROUP
```
{: pre}

### Example to get policy using CLI
{: #agentb1-getpolicy-cli}

```sh
ibmcloud schematics policy get --id POLICY_ID 
```
{: pre}

### Example to update policy using CLI
{: #agentb1-updatepolicy-cli}

```sh
ibmcloud schematics policy update --id POLICY_ID --kind POLICY_KIND --location LOCATION  --resource-group RESOURCE_GROUP
```
{: pre}

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
