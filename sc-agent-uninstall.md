---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-22"

keywords: schematics agents, agents, set up an agent

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agents is a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.
{: beta}

# Uninstalling {{site.data.keyword.bpshort}} Agents
{: #uninstall-agent}

The {{site.data.keyword.bpshort}} Agent that are deployed in your {{site.data.keyword.cloud}} account can be uninstalled from your {{site.data.keyword.bpshort}} service instance by following the given cleanup steps.
{: shortdesc}

Decide if you want to delete the workspace and destroy the associated cloud resources, or both. This job cannot be undone. If you remove the workspace and keep the cloud resources, you need to manage the resources with the resource list or [command-line](/docs/schematics?topic=schematics-schematics-cli-reference&interface=cli#schematics-workspace-delete). For more information, refer to [Deleting a workspace](/docs/schematics?topic=schematics-workspace-setup#del-workspace).
{: note}

## Clean up Agent infrastructure
{: #cleanup-agent-infra}

1. From the [{{site.data.keyword.bpshort}} Workspace](https://cloud.ibm.com/schematics/workspaces/) page, open your workspace. Enter your `<workspace_id_schematics-agent-infrastructure>` as part of the URL.
2. Select the **Actions** drop down list> **Destroy resources** > **Type the name of workspace** > click **Destroy**.
3. From the workspace `schematics-agent-infrastructure` page > **Jobs** tab
    - Wait for Terraform destroy job to complete. It takes about 10 to 15 minutes.
    - From the workspace `schematics-agent-infrastructure` page, click **Jobs** to view a message as `Destroy successful`. 
4. Select **Actions** > **Delete workspace** > **Type the name of workspace** > click **Delete**.

## Clean up Agent service
{: #cleanup-agent-service}

1. From the [{{site.data.keyword.bpshort}} Workspace](https://cloud.ibm.com/schematics/workspaces/) page, open your workspace. Enter your `<workspace_id_schematics-agent-service-workspace>` as part of the URL.
2. Select **Actions** > **Destroy resources** > **Type the name of workspace** > click **Destroy**.
3. From the workspace `schematics-agent-service-workspace` page > **Jobs** tab
    - Wait for Terraform destroy job to complete. It takes about 10 to 15 minutes.
    - From the workspace `schematics-agent-service-workspace` page, click **Jobs** to view a message as `Destroy successful`.
4. Select **Actions** > **Delete workspace** > **Type the name of workspace** > click **Delete**.
