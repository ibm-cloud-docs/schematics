---

copyright:
  years: 2017, 2022
lastupdated: "2022-08-04"

keywords: schematics agents trusted profile id, agent trusted id, trusted profile,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agents is a [Beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to, the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations) in the Beta release.
{: beta}

# Create `profile_id` for Agents
{: #agent-trusted-profile}

Enable and configure your Agent service to establish trust with computed resources for Kubernetes cluster by using [trusted profile](/docs/account?topic=account-create-trusted-profile#create-profile-compute) as listed in the steps.

1. Describe your profile.
   - Go to **Manage** > **Access (IAM)** in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com) and select **Trusted profiles**.
   - Click **Create +**.
   - Name the profile **myagent-profile**.
   - Click **Continue**.
 
2. Establish trust to the trusted profile.
   - From the Trust relationship tab, select **Compute resources**.
   - Select Compute service type as **Kubernetes**.
   - Select **Specific resources**.
   - Click **Add a resource +**.
   - For **Allow access to**, select your cluster.
   - For **Namespace** enter `schematics-job-runtime`.
   - For  **Service account**, enter **default**.
   - Click **Continue**.

3. Assign access to the trusted profile.

   - From the **Access policies**, click **Assign access +**.
   - Select **{{site.data.keyword.bpshort}}** service to assign access.
   - Click **Next**.
   - Check **All Resources** to scope the access.
   - Click **Next**.
   - Check **Reader**, **Manager**, **Viewer**, and **Operator** level of access for the Roles and actions.
   - Check **Create**.

4. View trusted profile ID.

   - From your Trusted profiles page, click **Details** tab.
   - Record the **Profile ID**, for example, `Profile-1bd5eala-000-4a6666-00011` to override the input variable `profile_id` of Agents service.

## Next Step
{: #agent-profile-id-nextstep}

Refer to [Deploying the Agent services settings](/docs/schematics?topic=schematics-agents-setup#agents-setup-svc) to override the `profile_id` input variable.
