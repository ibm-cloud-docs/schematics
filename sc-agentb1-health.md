---

copyright:
  years: 2017, 2024
lastupdated: "2024-08-29"

keywords: schematics agent health, agent health, health

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Monitoring agent health
{: #agentb1-health}

{{site.data.keyword.bplong}} Agent performs a health check on the post deployment validation and in-use status of an agent.
{: shortdesc}

## Monitoring agent health using the CLI
{: #health-agentb1-cli}
{: cli}

To review the health of an agent by using the CLI, use the [agent health](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-agent-health) command. This command requires the `AGENT_ID` as an input argument.
{: shortdesc}

The output of an agent health command displays the list of relevant Kubernetes and agent health property names, the expected value, actual value, and the result as PASS or FAIL.

Example

```sh
ibmcloud schematics agent health --id agent-ga-prod-cli-jan-10.soA.cd1c  
```
{: pre}

Output

```text
Initiating agent health...
Job ID .ACTIVITY.f6f77588
```
{: screen}

Example

```sh
ibmcloud schematics agent get --id agent-ga-prod-cli-jan-10.soA.cd1c  
```
{: pre}

Output

```text
Retrieving agent...
OK
                    
ID               agent-ga-prod-cli-jan-10.soA.cd1c   
Name             agent-ga-prod-cli-jan-10   
Status           ACTIVE   
Version             
Location         us-south  
Agent Location   us-south   
Resource Group   Default   
                 
Recent Job   Job ID                             Status                   Last modified   
DEPLOY       f5c6987ce53032547b6d5d5f870dfe5f   Job Success               2024-01-10T10:00:00.000Z   
HEALTH       .ACTIVITY.f6f77588                 Triggered health check   2024-01-10T12:31:15.326Z 
```
{: screen}

## Health properties
{: #agent-health-property}

The following table describes the list of agent and Kubernetes health properties.

| Property name | Description |
| --- | --- |
| runtime | Health of the workspace and action job pods in an agent. |
| sandbox | Health of the Sandbox job pods in an agent, that are used to download Git repositories. |
| job-runner | Health of the job orchestrator pods in an agent. |
| log-collector | Health of the log collector pods in an agent. |
{: caption="{{site.data.keyword.bpshort}} Agent health properties" caption-side="top"}

## Monitoring agent health using API
{: #health-agentb1-api}
{: api}

Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API. For more information about agent health API, see [get an agent health check](/apidocs/schematics/schematics#get-health-check-agent-job) job status. The agent health API displays the health status of your deployed agent.
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

## Next steps
{: #agent-health-nextstep}

- When agent health deteriorates, you can review the current deployment and update the agent and the Kubernetes configuration as described in [Deploying agents](/docs/schematics?topic=schematics-deploy-agent-overview&interface=cli).

- You can check out the [agent FAQ](/docs/schematics?topic=schematics-faqs-agent) for many common questions.
