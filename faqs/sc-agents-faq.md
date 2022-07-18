---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-18"

keywords: schematics faqs, infrastructure as code, iac, schematics agents faq, agents faq,

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agents is a [Beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to, the list of [limitations for Agent](/docs/schematics?topic=schematics-agent-beta-limitations) in the Beta release.
{: beta}

# Agents
{: #faqs-agent}

Answers to common questions about the {{site.data.keyword.bplong_notm}} Agents are classified into following sections.
{: shortdesc}

## Can I install more than one Agent service on the Agent infrastructure?
{: #faqs-agent-install}
{: faq}
{: support}

You can install more than one Agent Service onto different clusters, but not the same cluster.  Installing a second Agent into the same cluster it not supported and fails with the namespace collision error. There is no need for a second Agent service on the Agent infrastructure, such as, Kubernetes cluster, LogDNA.

## What is the cost of installing the {{site.data.keyword.bpshort}} Agent?
{: #faqs-agent-cost}
{: faq}
{: support}

The cost break-up for the {{site.data.keyword.bpshort}} Agents is as follows:

Agents infrastructure
- Cost of VPC infrastructure elements such as, subnet, public gateways.
- Cost of IBM Kubernetes Service (cluster) on VPC, with 3 node worker pool.
- Cost of IBM Log Analysis services.

Agents service
- There is no cost involved with running the Agent Service for Beta.
   Agent Services will be a priced service, post Beta.
   {: note}

## What {{site.data.keyword.bpshort}} jobs can I run in my Agent?
{: #faqs-agent-jobs}
{: faq}
{: support}

For Beta you can bind a `{{site.data.keyword.bpshort}} Workspace`, to the Agent. Therefore, you can run Terraform workload on the Agent.

Currently, you cannot bind the `{{site.data.keyword.bpshort}} Action`, to the Agents. Therefore, you cannot run Ansible workload on the Agent.
{: note}

## How can I see the Workspace job results and logs, for the workloads that ran on the Agent?
{: #faqs-wks-job-logs}
{: faq}
{: support}

The Workspace job logs are available in {{site.data.keyword.bpshort}} UI console.  You can also access the Workspace job logs by using the {{site.data.keyword.bpshort}} Workspace API, or CLI.

## How many {{site.data.keyword.bpshort}} Jobs can run in parallel in the Agent?
{: #faqs-job-parallel}
{: faq}
{: support}

Currently, the {{site.data.keyword.bpshort}} Agent will run three {{site.data.keyword.bpshort}} jobs in parallel. The rest of the jobs are queued in your cluster.
You can customize the Agent service deployment to increase the number of job PODs in order to increase the number of parallel jobs.
You must also monitor the resources in the Agent Infrastructure. The number of the workers in the worker pool must be increased to run more jobs, in parallel.

## While provisioning the Agent infrastructure, I see the following error message. What is the root cause? and What should I do next? 
{: #faqs-auth-error}
{: faq}
{: support}

```text
Error: Authentication failed, Unable to refresh auth token: Request failed with status code: 400, BXNIM0439E: Transaction-Id:[OHptc2Y-6a6f4800b8a346228482d76cd040942c] Session 'C-bdc1fbe8-4445-4fd1-ac23-4e54ae3831f9' is invalidated due to inactivity.. Try again later
 2022/06/21 11:24:18 Terraform apply |
 2022/06/21 11:24:18 Terraform apply |   with module.vpc_cluster[0].ibm_container_vpc_cluster.cluster[0],
 2022/06/21 11:24:18 Terraform apply |   on cluster/main.tf line 5, in resource "ibm_container_vpc_cluster" "cluster":
 2022/06/21 11:24:18 Terraform apply |    5: resource "ibm_container_vpc_cluster" "cluster" {
```
{: screen}

**Root cause** is while provisioning the Agent infrastructure, the API key is an optional input. If you did not provide the API key, {{site.data.keyword.bpshort}} uses your IAM token as the user credentials to provision the Agent infrastructure. The Agent Infrastructure may take a long time to complete the provisioning. Sometimes, the IAM token would expire before the provisioning completes. An expired IAM token is one possible cause of this Authentication failure.

You need to just retry to provision an Agent infrastructure. Or, you can provide a valid API key to provision the Agent infrastructure. 

## What is the difference between `agent-location` and `location` input variable flag in Agents service?
{: #faqs-agent-location}
{: faq}
{: support}

`--agent-location` is a required variable that specifies the region of the cluster where the Agent service is deployed. For example, `us-south`. Where as `--location` is also a required varaible that specified the geographic locations that are supported by {{site.data.keyword.bpshort}} service such as, `us-south`, `us-east`, `eu-de`, `eu-gb`. Jobs are picked up from this location for processing.

## Can I have a different resource group for {{site.data.keyword.bpshort}} Workspaces and a Agents?
{: #faqs-agent-rg}
{: faq}
{: support}

Yes, you can have a different resource group for {{site.data.keyword.bpshort}} Workspaces and a Agents.

## Can I register an Agent in a different resource group to what I provided in the Agent service and infrastructure Workspaces? 
{: #faqs-agent-register}
{: faq}
{: support}

Yes, you can register Agent in a different resource group to what is provided in the Agent service and infrastructure Workspaces.

## Can I register an Agent in a different region to what I provided in the Agent service and infrastructure Workspaces?
{: #faqs-agent-region}
{: faq}
{: support}

Yes, you can register an Agent in a different region to what is provided in the Agent service and infrastructure Workspaces.

## Can I have different region for {{site.data.keyword.bpshort}} Workspace and Agents? 
{: #faqs-wks-agent-region}
{: faq}
{: support}

If an Agent is running is Sydney, but Agent having us-sourth, or eu-de as an endpoint. Workspaces should be created in the same region, where the {{site.data.keyword.bpshort}} endpoint is configures. Because {{site.data.keyword.bpshort}} picks the jobs to executed based on the {{site.data.keyword.bpshort}} endpoint configuration.

## Can I know the steps to get the Jobrunner (JR) logs to provide the request ID?
{: #faqs-agent-jr-logs}
{: faq}
{: support}f

The following steps allows to get the JR logs and provide the request Id:
- Get the `logdna_name` from outputs: section of your jobs log in the Agent infrastructure workspace.
- After your job is run and `failed`, or `succeeded`. If you want to {{site.data.keyword.bpshort}} team to debug the backend logs of the Agent, you can filter the logDNA logs with the request ID from the jobs log.
- The logDNA sends to your mail. You can save the logs with that filter and send the file to {{site.data.keyword.bpshort}} team.

## What is time period set to deploy the cloud resources?
{: #faqs-agent-limit}
{: faq}
{: support}

The default time period set to deploy the cloud resources is `30 minutes` for an Agent. For more information, to set the time limit, refer to [time out](/docs/schematics?topic=schematics-job-queue-process#job-queue-timeout)
