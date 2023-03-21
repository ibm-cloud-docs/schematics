---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-21"

keywords: schematics agent health, agent health, health

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agent are a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that are available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for agent](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.
{: beta}

# Managing agent health
{: #agentb1-health}

The {{site.data.keyword.bplong}} Agent creates a terraform based automation to check the health of an installed agent. Following are the evidences that the {{site.data.keyword.bpshort}} Agent health fetches to a user.
- Agent health can fetch details only once your _agent deploy_ is successful. 
- Agent health displays the job status of your deployed agent that are up and running. 
- Agent registration with {{site.data.keyword.bpshort}} is successful or unsuccessful.

## List agent health using CLI
{: #health-agentb1-cli}
{: cli}

To view the agent health by using the CLI, use the `ibmcloud schematics agent health` command. This command requires `agent_id` arguments. It is region specific and will only list agents in the selected CLI region. 
{: shortdesc}

For all the agent commands, syntax, and option flag details, see [agent beta-1 commands](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-health).
{: important}

```sh
ibmcloud schematics agent health --id <Provide your agent_ID> [--output OUTPUT]
```
{: pre}

```text
Validating agent health...
Health ID: .ACTIVITY.85353750


runtime error: invalid memory address or nil pointer dereference
geethasathyamurthy@Geethas-MacBook-Pro agentb1bnppstagetesting % vi createpolicy.json
geethasathyamurthy@Geethas-MacBook-Pro agentb1bnppstagetesting % ibmcloud schematics workspace list
Retrieving workspaces...
OK
Name                             ID                                                        Description   Version   Status   Frozen   
gsmmar2cliv2-agent-test-prs      eu-de.workspace.gsmmar2cliv2-agent-test-prs.30e0d302                              FAILED   False   
gsmmar2cliv2-agent-test-health   eu-de.workspace.gsmmar2cliv2-agent-test-health.7ff086ed                           FAILED   False   
gsmmar2cliv2-agent-test-deploy   eu-de.workspace.gsmmar2cliv2-agent-test-deploy.89fa2a8e                           FAILED   False   
                                 
Showing 1-3 of 3 items
```
{: screen}

## List agent health using API
{: #health-agentb1-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information, about agent health API, see [get an agent health check](/apidocs/schematics/schematics_internal_v1#get-health-check-agent-job) job status. Agent health API displays the job status of your deployed agent.
{: shortdesc}

Example

```json
GET /v2/agent_health/agent-id-xx-000soB.347a/ HTTP/1.1
Host: schematics.cloud.ibm.com
Content-Type: application/json
Authorization: Bearer <auth_token>
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

