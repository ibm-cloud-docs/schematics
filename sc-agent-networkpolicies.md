---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-28"

keywords: schematics agents, agents, set up an agents

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agents are a [Beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations) in the Beta release.
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



