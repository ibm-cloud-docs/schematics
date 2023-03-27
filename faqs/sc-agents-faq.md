---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-27"

keywords: schematics faqs, infrastructure as code, iac, schematics agents faq, agents faq,

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agents are a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that are available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agent](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.
{: beta}

# Agent
{: #faqs-agent}

Answers to common questions about the Agent for {{site.data.keyword.bplong_notm}}.
{: shortdesc}

## What are the new updates in the agent beta-1 release?
{: #faqs-agent-update}
{: faq}
{: support}

The following are the features in Agent beta-1 release.
- Improvements to the agent deployment experience through CLI.
- Support to run Ansible playbooks on the agent.
- Dynamic assignment of workspace or action jobs to the agent.

## What is the cost of installing the {{site.data.keyword.bpshort}} Agent?
{: #faqs-agent-cost}
{: faq}
{: support}

The following are the cost break-up for the {{site.data.keyword.bpshort}} Agent.
Pre-requisite: Agent infrastructure
- Cost of VPC infrastructure elements such as, subnet, public gateways.
- Cost of IBM Kubernetes Service (cluster) on VPC, with three-node worker pool.
- Cost of IBM Cloud Object Storage

Agent beta-1 service
- There is no cost that are involved in running the agent service. 
- Post beta, the agent services may be a priced service.

## Can I install more than one {{site.data.keyword.bpshort}} Agent on a cluster?
{: #faqs-agent-install}
{: faq}
{: support}

You can install only one agent on a Kubernetes cluster on {{site.data.keyword.containerlong_notm}} or {{site.data.keyword.redhat_openshift_full}} on {{site.data.keyword.cloud_notm}}. You can agent on to different clusters.

You cannot install more than one agent in a single Kubernetes cluster. You get a failure with namespace conflict error.

## What type of Schematics jobs can I run in my Agent?
{: #faqs-agent-jobs}
{: faq}
{: support}

You can run {{site.data.keyword.bpshort}} Workspace jobs or Terraform workload on an Agent. You can also run {{site.data.keyword.bpshort}} Action jobs or Ansible playbooks on an Agent. 

## How can I see the {{site.data.keyword.bpshort}} job results and logs, for the workloads running on an agent?
{: #faqs-agent-workload}
{: faq}
{: support}

The workspace job or action job logs are available in {{site.data.keyword.bpshort}} UI console. You can also access these job logs using the {{site.data.keyword.bpshort}} Workspace API, or CLI.

## How many {{site.data.keyword.bpshort}} jobs can run in parallel in the Agent?
{: #faqs-agent-parallel}
{: faq}
{: support}

Currently, an agent runs three {{site.data.keyword.bpshort}} jobs in parallel. The rest of the jobs are queued in your cluster.
In future, you can customize an agent to increase the number of job Pods, in order to increase the number of parallel jobs. You must also monitor the resources in the Kubernetes cluster. 

## What is the minimum cluster configuration required in Agent release?
{: #faqs-agent-min-cluster}
{: faq}
{: support}

The agent needs {{site.data.keyword.containerlong_notm}} or {{site.data.keyword.redhat_openshift_full}}  Service with minimum three worker nodes, with a flavor of `b4x16` or higher.

## How many workspaces can be assigned to an agent?
{: #faqs-agent-min-wks}
{: faq}
{: support}

Currently, you can assign any number of workspaces to an agent. The workspace jobs are queued to run on the agent or based on the agent selection policy.
The agent periodically polls the {{site.data.keyword.bpshort}} for jobs to run polling interval of one minute. By default, the agent runs only three jobs in parallel. The remaining jobs are kept in queue.

## How many jobs can run in parallel on an agent?
{: #faqs-agent-min-job}
{: faq}
{: support}

- Schematics Agent can perform three Git download jobs in parallel.
- Schematics Agent can run three Workspace jobs (Terraform commands), in parallel.
- Schematics Agent can run three Action jobs (Ansible playbook), in parallel.

## What is the default polling interval for an agent?
{: #faqs-agent-poll-interval}
{: faq}
{: support}

{{site.data.keyword.bpshort}} maintains a queue of jobs that must be delivered to an agent. The agent will poll for {{site.data.keyword.bpshort}} Jobs, every one minute by default.

## What is the difference between agent-location and location input variable flag in Agent service?
{: #faqs-agent-diff-location}
{: faq}
{: support}

The `--agent-location` is a variable that specifies the region of the cluster where an agent service is deployed. For example, `us-south`. 
The `--location` is a variable that specifies the region supported by {{site.data.keyword.bpshort}} service such as `us-south`, `us-east`, `eu-de`, `eu-gb`. The agent polls {{site.data.keyword.bpshort}} service instance from this location, for workspace or action jobs for processing.

## Can an agent run {{site.data.keyword.bpshort}} Job from different resource group?
{: #faqs-agent-diff-rg}
{: faq}
{: support}

Yes, an ggent can run {{site.data.keyword.bpshort}} Jobs related to workspace or actions, from all or any resource group, in an account.

## Can an agent run {{site.data.keyword.bpshort}} Job from different region?
{: #faqs-agent-diff-region}
{: faq}
{: support}

The Agent periodically polls the regional endpoint of the {{site.data.keyword.bpshort}} service instance, to fetch and run jobs. It can connect to only one regional endpoint (home). For example, if an agent is deployed on a cluster in Sydney and has been configured to use the {{site.data.keyword.bpshort}} `eu-de` regional endpoint as it’s home location. The agent polls for jobs in `eu-de` region. Hence, the workspace or action for Sydney must be created in the `eu-de` region.

## Can I register one agent with multiple accounts?
{: #faqs-agent-register}
{: faq}
{: support}

You cannot register an agent with multiple account in the beta release.

## Can jobs of an existing workspace configured to run on an agent?
{: #faqs-agent-conf}
{: faq}
{: support}

Yes, if your workspace has the right values with the tags, resource-group, location. {{site.data.keyword.bpshort}} uses an `agent-selection-policy` to automatically assign the jobs to run on the target agent.
For example, If you have an existing workspace: `wks-0120` with `tag=dev`, and you want the workspace to run on `Agent-1`. Create an `agent-selection-policy` with the rule to pick `Agent-1` when the `tag == dev`.  Subsequently, the workspace job such as plan, apply, update, and so on automatically routes to `Agent-1`.

## What are the identity and permissions needed to deploy an agent?
{: #faqs-agent-permission}
{: faq}
{: support}

Agent recommends to deploy an agent, connect with {{site.data.keyword.bpshort}}, and users to manage agents. For more information about identity and permissions, see [agent permission](/docs/schematics?topic=schematics-access#agent-permissions) 

## When my agent is deployed in a private network. How can I configure mirror site for the Terraform plug-ins?
{: #faqs-agent-pvt-network}
{: faq}
{: support}

By default, {{site.data.keyword.bpshort}} Job running the Terraform CLI attempts to download the Terraform plug-ins or Terraform modules from the Internet or public network.  
If an Agent is running in a private network, you can add the following two environment variables in the workspace payload to setup the mirror site.
- The `TF_NETWORK_MIRROR_URL` location where custom Terraform providers are hosted.
- The `TF_NETWORK_MIRROR_PROVIDER_NAME` provider name that is downloaded from the custom location. This is an optional variable. It is defaulted to all providers.

{{site.data.keyword.bpshort}} job use the environment variables to auto generate the following custom configuration file.

```json
provider_installation {
  network_mirror {
    url = "${TF_NETWORK_MIRROR_URL}"
    include = ["${TF_NETWORK_MIRROR_PROVIDER_NAME}"]
  }
}
```
{: pre}

Sets a new environment variable to point to this generated cutsom config file

```sh
export TF_CLI_CONFIG_FILE=/home/appuser/terraform-custom.config
```
{: pre}

## Can I inject the self signed or TLS certificates in {{site.data.keyword.containerlong_notm}} pod or container's trusted CA root certificate store during agent runtime?
{: #faqs-agent-certificate}
{: faq}
{: support}

Yes, follow these steps to inject the certificates into an agent runtime.

In the four `.cer` extension filenames ensure you modify to replace the space with underscore.
{: note}

1. Create config map by using `.cer` file as shown in the `kubectrl` command.

    ```sh
    kubectl -n schematics-runtime create configmap bnpp-root —-from-file 2014-2044_BNPP_Root.cer
    ```
    {: pre}

    ```sh
    kubectl -n schematics-runtime create configmap bnpp-authentication —-from-file 2014-2029 BNPP_Users_Authentication.cer
    ```
    {: pre}

    ```sh
    kubectl -n schematics-runtime create configmap bnpp-infrastructure —-from-file 2014-2029 BNPP_Infrastructure.cer
    ```
    {: pre}

2. Mount config map file as a volume in a directory `/etc/ssl/certs/` as file `agent-runtime-deployment-certs.yaml` in a shared `bnpp_agent_deployment_files` directory.

Shared directory `bnpp_agent_deployment_files` has two yaml files named 
    - `agent-runtime-deployment-certs.yaml` and 
    - `agent-runtime-deployment.yaml`.

The `agent-runtime-deployment-certs.yaml` file updates the certificates and appends the `agent-runtime-deployment.yaml` file which provides you the desired deployment details to inject the certificates without any additional changes.

## List the attributes that {{site.data.keyword.bpshort}} Workspaces or Actions attributes used to dynamically select an agent
{: #agent-dynamic-attribute}

The following attributes of the {{site.data.keyword.bpshort}} Workspace or {{site.data.keyword.bpshort}} Action are used to dynamically select the agent instance.
{: shortdesc}

- Resource group
- Location
- Tags

The [Agent assignment policy](/docs/schematics?topic=schematics-policy-manager) for an agent instance describes how an Agent is selected to run a Workspace job or Action job.

Example

If your organization has three different network isolation zones (such as `Dev`, `HR-Stage`, and `HR-Prod`) and you have installed three agents (one each, for the three network isolation zone). You have defined an `agent-assignment-policy` for the agent running in `Dev`, with the selector as `tags=dev`. All workspaces that have `tags=dev` automatically bounds to the `Dev` agent. In other words, the `Dev` agent is used to download Terraform templates (from the Git repository), to run Terraform jobs. Similarly, the `agent-assignment-policy` can include other attributes of the workspaces, in order to control the location of the job execution.

