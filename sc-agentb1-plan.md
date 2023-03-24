---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-23"

keywords: schematics agent planning, planning agent, agent planning, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Preparing for agent deployment
{: #plan-agent-overview}

{{site.data.keyword.bpshort}} Agents is a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

Complete the tasks below to prepare your {{site.data.keyword.cloud_notm}} environment to deploy a new agent.
{: shortdesc}

After an account administrator with the required authority prepares for deploying an agent, you might not need to change them each time that you create additional agents. Each time you create a new agent, review the previous settings and confirm the level of account access is still required. 

Review and complete the following tasks to prepare to deploy an agent. 

1. _Infrastructure Availability_: A {{site.data.keyword.bpshort}} agent can be deployed on any existing private or public infrastructure such as [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters), [{{site.data.keyword.vpc_full}}](/docs/openshift?topic=openshift-cluster-create-vpc-gen2&interface=ui) clusters, or [{{site.data.keyword.redhat_openshift_full}}](/openshift?topic=openshift-clusters) cluster services with a minimum three worker nodes, a flavor of `b4x16` or higher configuration. Verify that a suitable cluster exists or provision a new cluster with the correct config. 
2. _Resource Access_: Assign IAM access permissions to the Resource Group the agent will be associated with, and any additional resources such as {{site.data.keyword.cos_full_notm}} and {{site.data.keyword.objectstorageshort}} bucket for the specified region. For more information about assigning access, see [giving access to resources in resource groups](/docs/account?topic=account-rgs_manage_access).
3. _Access Permissions_: An user must have the resource, {{site.data.keyword.bpshort}}, [service role, platform role](/schematics?topic=schematics-access#iam-platform-svc-roles), and account permissions. For the detailed list of permissions, refer to [agent permissions](/docs/schematics?topic=schematics-access#agent-permissions).
4. _{{site.data.keyword.cloud_notm}} CLI_ When deploying an agent via the CLI, you must install the [{{site.data.keyword.bpshort}} CLI v1.12.8](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) or higher plugin. For more information about plugin installation, see [installing {{site.data.keyword.bpshort}} CLI plugin](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin).

## Next steps
{: #agent-plan-nextsteps}

The next step is to [deploy an agent](/docs/schematics?topic=schematics-deploy-agent-overview).
