---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-17"

keywords: schematics agent, agent policy, policies

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agent are a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that are available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for agent](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.
{: beta}

# Managing network policies
{: #agent-networkpolicy}

{{site.data.keyword.bpshort}} Agents contain the network policies to secure the runtime pods communication. It communicates with other pods and the external endpoints. The incoming and outgoing network traffic is allowed or restricted based on the protocol, port, source, and destination IP addresses.
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

## Workspaces or Actions attributes used to dynamically select Agent
{: #policy-dynamic-attribute}

The following attributes of the {{site.data.keyword.bpshort}} Workspace or {{site.data.keyword.bpshort}} Action are used to dynamically select the agent instance.

- Resource group
- Location
- Tags

The `Agent assignment policy` for an agent instance describes how an Agent is selected to run a Workspace job or Action job. For more information about {{site.data.keyword.bpshort}} policy, see [policy commands](/docs/schematics?topic=schematics-schematics-cli-reference#policy-cmd).

Example

If your organization has three different network isolation zones (such as `Dev`, `HR-Stage`, and `HR-Prod`) and you have installed three agents (one each, for the three network isolation zone). You have defined an `agent-assignment-policy` for the agent running in `Dev`, with the selector as `tags=dev`. All workspaces that have `tags=dev` automatically bounds to the `Dev` agent. In other words, the `Dev` agent is used to download Terraform templates (from the Git repository), to run Terraform jobs. Similarly, the `agent-assignment-policy` can include other attributes of the workspaces, in order to control the location of the job execution.
