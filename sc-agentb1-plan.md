---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-08"

keywords: schematics agent planning, planning agent, agent planning, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Planning agent
{: #plan-agent-overview}

{{site.data.keyword.bpshort}} Agents is a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations#sc-agent-beta-limitation) in the beta release.
{: beta}

To access your self managed on premises or cloud storage, you need an {{site.data.keyword.bpshort}} Agent that are associated with your {{site.data.keyword.cloud}}.

To create, deploy and activate your {{site.data.keyword.bpshort}} Agent, you need to ensure the following preparation steps.

- The agent can be used on any existing private or public infrastructure such as [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters), [{{site.data.keyword.vpc_full}}](/docs/openshift?topic=openshift-cluster-create-vpc-gen2&interface=ui) clusters, or {{site.data.keyword.redhat_openshift_full}} cluster services with minimum three worker nodes, a flavor of `b4x16` or higher setup.
- Ensure the {{site.data.keyword.cloud_notm}} Users needs {{site.data.keyword.bpshort}} services access and the User must have `Manager` service access and `Operator` platform access to work with an agent. For more information about setting up an access, see [assign access to the trusted profile](/docs/schematics?topic=schematics-agent-trusted-profile).
- You need to have right [permission](/docs/schematics?topic=schematics-access#agent-permissions) to access the resources such as {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.vpc_full}}, {{site.data.keyword.redhat_openshift_notm}} clusters, {{site.data.keyword.cos_full_notm}}, and so on from another {{site.data.keyword.cloud_notm}} account.
- You need full permission to access the {{site.data.keyword.bpshort}} Workspace from other {{site.data.keyword.cloud_notm}} account.
- Ensure you have installed {{site.data.keyword.bpshort}} CLI v1.12.8 and higher plugin to create an agent beta-1. For more information about plugin installation, see [installing {{site.data.keyword.bpshort}} CLI plugin installation](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin).



## Next steps
{: #agent-plan-nextsteps}

The next step is to [deploy an agent](/docs/schematics?topic=schematics-deploy-agent-overview).
