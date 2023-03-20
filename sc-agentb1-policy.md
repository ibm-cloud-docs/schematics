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



# Managing account policies
{: #policy-manager}

{{site.date.keyword.bpshort}} account policies allows you to create rules or criteria to define the behavior, schedule,or the constraint for {{site.data.keyword.bpshort}} core capabilities.
{: shortdesc}

## Components
{: #policy-components}

The following are the major components of the {{site.data.keyword.bpshort}} policy solution.

### Policy execution engine
{: #policy-exe-engine}

Policy execution engine provides an interface for other microservices to evaluate the applicable policy or policies and to make decisions. Policy execution engine is implemented as datajob gRPC service.

### Policy kind
{: #policy-kind}

Policy kind is based on core capability and the operations for these capabilities such as assignment, enablement, purge, scheduling. Policy kind helps in organinsing policies and identifying the unique policy parameter schema which will be evaluated by respective policy manager during policy evaluation.

The following are the planned or the supported kind.

- agent_assignment_policy
- job_purge_policy
- workspace_drift_scheduler_policy
- workspace_control_enablement_policy

### Policy manager
{: #policy-mgr}

Policy manager is responsible to read and evaluate the policy data. There is a separate policy manager per policy kind to evaluate the policy parameter as policy paramater schema varies for each policy kind. Refer the syntax of the policy data model. Each state of the policy date value can be `active`, `inactive`, or `draft`. 

Policy Data model

```json
{
    "name": "",
    "description":"",
    "id": "",
    "crn": "",
    "account": "",
    "created_at": "",
    "created_by": "",
    "updated_at": "",
    "resource_group" : "",
    "tags" : ["", ""],
    "location": "",
    "state": "",
    "policy_kind": "",
    "policy_target" : {}
    "policy_parameter" : {}
}
```
{: pre}

`policy_kind` is one of policy kinds.
`policy_target` is set of criteria for selecting schematics data. `policy_target` expression contains one of the following

```json
{
    "tags" : [],
    "resource_groups": [],
    "locations" : []
}
OR
{
    "ids": []
}
```
{: pre}

`policy_parameter` evaluation determines decision which is shared through PDP interface for consumption by PEP modules in {{site.data.keyword.bpshort}}. The `policy_parameter` schema varies based on the `policy_kind`.

`agent_assignment_policy` contains one of the following

```json
{
    "tags" : [""], 
    "types" : [""],
    "resource_groups": [""],
    "locations" : []
}
OR
{
    "ids": []
} 
```
{: pre}

`job_purge_policy`  : TBD
`workspace_drift_scheduler_policy` : TBD
`workspace_control_enablement_policy` : TBD

## Workspaces or Actions attributes used to dynamically select Agent
{: #policy-dynamic-attribute}

The following attributes of the {{site.data.keyword.bpshort}} Workspace or {{site.data.keyword.bpshort}} Action are used to dynamically select the agent instance.

- Resource group
- Location
- Tags

The `Agent assignment policy` for an agent instance describes how an Agent is selected to run a Workspace job or Action job. For more information about {{site.data.keyword.bpshort}} policy, see [policy commands](/docs/schematics?topic=schematics-schematics-cli-reference#policy-cmd).

Example

If your organization has three different network isolation zones (such as `Dev`, `HR-Stage`, and `HR-Prod`) and you have installed three agents (one each, for the three network isolation zone). You have defined an `agent-assignment-policy` for the agent running in `Dev`, with the selector as `tags=dev`. All workspaces that have `tags=dev` automatically bounds to the `Dev` agent. In other words, the `Dev` agent is used to download Terraform templates (from the Git repository), to run Terraform jobs. Similarly, the `agent-assignment-policy` can include other attributes of the workspaces, in order to control the location of the job execution.