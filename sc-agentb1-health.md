---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-16"

keywords: schematics agent health, agent health, health

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agent are a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that are available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for agent](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.
{: beta}

# Managing agent health
{: #agent-health}

{{site.data.keyword.bpshort}} Agent helps to collect metrics, events, analytics in your cluster through `schematics-agents-observe` namespaces. Each pod in your cluster writes its application health data into a file. Agent health can periodically retrieve the data through file from COS bucket by _agent health get_ call. For more information about the agent health command arguments, see [ibmcloud schematics agent health](/schematics?topic=schematics-schematics-cli-reference#schematics-agent-health).
{: shortdesc}

## List agent health using the CLI
{: #health-agentb1-cli}
{: cli}


## List agent health using the API
{: #health-agentb1-api}
{: api}

Syntax

```curl
curl -X POST http://us-east.schematics.cloud.ibm.com/v2/agent_health \
    -H 'Authorization: <enter your authorization token>' \
    -d '{
"name":"agent5",
"agent_location": "us-south"
}'

```
{: codeblock}

Example

```curl
```json
GET /v2/agents/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>

{
"name":"agent5",
"agent_location": "us-south"
}
```
{: codeblock}

Output

```text
Health scan
=======================
    
+---------------+--------+--------+----------+
| Namespaces    | Result | Found  | Expected |
+---------------+--------+--------+----------+
| sandbox       | Pass   | Active | Active   |
| runtime       | Pass   | Active | Active   |
| job-runner    | Pass   | Active | Active   |
| log-collector | Pass   | Active | Active   |
+---------------+--------+--------+----------+
+---------------+-------+------------------+----------+
| Pods          | Ready | Found            | Expected |
+---------------+-------+------------------+----------+
| sandbox       | 0/3   | ImagePullBackOff | Running  |
| runtime       | 0/6   | ImagePullBackOff | Running  |
| job-runner    | 0/1   | ImagePullBackOff | Running  |
| log-collector | 3/3   | Running          | Running  |
+---------------+-------+------------------+----------+

=======================
Health Check Completed 
```
{: screen}
