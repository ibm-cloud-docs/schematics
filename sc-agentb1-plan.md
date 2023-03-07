---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-07"

keywords: schematics agent planning, planning agent, agent planning, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Planning agent
{: #plan-agent-overview}

Prepare to create and deploy an agent by using {{site.data.keyword.bpshort}}. You need to ensure the following preparation to own an advantage of the {{site.data.keyword.bpshort}} agent.

- The agent can be used on any existing private or public infrastructure such as [{{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters), [{{site.data.keyword.vpc_full}}](/docs/openshift?topic=openshift-cluster-create-vpc-gen2&interface=ui) clusters, or {{site.data.keyword.redhat_openshift_full}} cluster services with minimum three worker nodes, a flavor of `b4x16` or higher setup.
- You need to create [trusted profile](/docs/schematics?topic=schematics-agent-trusted-profile) setup, or trusted profile ID, and trusted profile name to access the {{site.data.keyword.bpshort}} Agent feature.
- Ensure the {{site.data.keyword.cloud_notm}} Users needs {{site.data.keyword.bpshort}} services access and the User must have `Manager` service access and `Operator` platform access to work with an agent. For more information about setting up an access, see [assign access to the trusted profile](/docs/schematics?topic=schematics-agent-trusted-profile).
- You need to have right [IAM](/docs/containers?topic=containers-access_reference) access, when you are accessing the resources such as {{site.data.keyword.containerlong_notm}}, {{site.data.keyword.vpc_full}}, {{site.data.keyword.redhat_openshift_notm}} clusters, {{site.data.keyword.cos_full_notm}}, and so on from another {{site.data.keyword.cloud_notm}} account.
- You need full permission to access the {{site.data.keyword.bpshort}} Workspace from other {{site.data.keyword.cloud_notm}} account.
- Ensure that the {{site.data.keyword.cloud_notm}} catalog are onboarded in the region where agent are planned to creat.
- Ensure you have installed {{site.data.keyword.bpshort}} CLI v1.12.8 and higher plugin to create an agent. For more information about plugin installation, see [installing {{site.data.keyword.bpshort}} CLI plugin installation](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin).

## Next steps
{: #agent-plan-nextsteps}

The next step is to deploy an agent.
