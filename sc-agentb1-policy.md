---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-20"

keywords: schematics agent, agent policy, policies

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agent are a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that are available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for agent](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.
{: beta}



# Managing policies
{: #policy-manage}

{{site.date.keyword.bpshort}} account policies allows you to create rules or criteria to define the behavior, schedule, or the constraint for the {{site.data.keyword.bpshort}} core capabilities.
{: shortdesc}

## Components
{: #policy-components}

The following are the major components of the {{site.data.keyword.bpshort}} policy solution.

### Policy execution engine
{: #policy-exe-engine}

Policy execution engine provides an interface for other microservices to evaluate the applicable policy or policies and to make decisions. Policy execution engine is implemented as datajob gRPC service.

### Policy kind
{: #policy-kind}

Policy kind is based on core capability and the operations for these capabilities such as assignment, enablement, purge, schedule. Policy kind helps in organinsing policies and identifying the unique policy parameter schema evaluated through a respective policy manager during policy evaluation.

The following are the planned or the supported kind.

- agent_assignment_policy
- job_purge_policy
- workspace_drift_scheduler_policy
- workspace_control_enablement_policy




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
