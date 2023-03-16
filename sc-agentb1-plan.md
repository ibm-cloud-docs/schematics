---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-16"

keywords: schematics agent planning, planning agent, agent planning, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Planning agent
{: #plan-agent-overview}

{{site.data.keyword.bpshort}} Agents is a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

To access your self managed on premises or cloud storage, you need an {{site.data.keyword.bpshort}} Agent that are associated with your {{site.data.keyword.cloud}}. Consider the following steps to create the correct agent each time.
{: shortdesc}

Consider the following steps to create the correct {{site.data.keyword.bpshort}} agent each time.

Prepare your {{site.data.keyword.cloud_notm}} account for {{site.data.keyword.bplong}} Agent. After the account administrator makes these preparations, you might not need to change them each time that you create an agent. However, each time that you create an agent, you still want to verify that the current account-level access is what you need it to be.

1. _Infrastructure with capacity_: {{site.data.keyword.bpshort}} Agent can be used on any existing private or public infrastructure such as [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters), [{{site.data.keyword.vpc_full}}](/docs/openshift?topic=openshift-cluster-create-vpc-gen2&interface=ui) clusters, or [{{site.data.keyword.redhat_openshift_full}}](/openshift?topic=openshift-clusters) cluster services with minimum three worker nodes, a flavor of `b4x16` or higher setup.
2. _Resources_: Assign access to the Resource Group, and resources such as {{site.data.keyword.cos_full_notm}}, {{site.data.keyword.cloud_notm}} {{site.data.keyword.objectstorageshort}} bucket of the specific region. For more information about assign access, see [giving access to resources in resource groups](/docs/account?topic=account-rgs_manage_access).
3. _Permissions_: An user must have the resource level, {{site.data.keyword.bpshort}} level, [service role, platform role](/schematics?topic=schematics-access#iam-platform-svc-roles), and account level permissions. For the detailed list of permission, refer to [agent permissions](/docs/schematics?topic=schematics-access#agent-permissions).
4. _Agent creation through CLI_ To create an agent beta-1 through CLI, you must install [{{site.data.keyword.bpshort}} CLI v1.12.8](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin) and higher plugin. For more information about plugin installation, see [installing {{site.data.keyword.bpshort}} CLI plugin](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin).

## Next steps
{: #agent-plan-nextsteps}

The next step is to [deploy an agent](/docs/schematics?topic=schematics-deploy-agent-overview).
