---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-18"

keywords: schematics agents trusted profile id, agent trusted id, trusted profile,

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agents are a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.
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
   - In **Allow access to**, select your cluster.
   - In **Namespace** enter `schematics-job-runtime`.
   - For **Service account**, enter **default**.
   - Click **Continue**.

   You created a trusted profile for a compute resource (Cluster running agent micro-services in `schematics-job-runner` namespace in your default service account.
   {: note}

3. Assign access to the trusted profile.

   - Click the **Access policy**.
   - Select **{{site.data.keyword.bpshort}}** service.
   - Click **Next**.
   - Select **All resources**.
   - Click **Next**.
   - Select `Operator` role.
   - Click **Add**.
     The trusted profile is provided Operator access in {{site.data.keyword.bpshort}} services to allow Agents to fetch jobs to process.
     {: note}

   - Select `All Identity and Access enabled services`.
      - Click **Next**.
      - Select **Specific Resources** option. Enter the name of the resource group where your Agents are registered.
      - Click **Next** and select `Viewer` role.
      - Click **Next** and select `Reader` role.
      - Click **Add** and select `Assign` in the right navigation bar.
        The trusted profile provides `Reader`, and `Viewer` access for the resource group that allow Agents to read an Agent registration detail.
        {: note}

   - Click **Create**.

4. View trusted profile ID.

   - From your Trusted profiles page, click **Details** tab.
   - Record the **Profile ID**, for example, `Profile-1bd5eala-000-4a6666-00011` to override the input variable `profile_id` of Agents service.

## Next Step
{: #agent-profile-id-nextstep}

Refer to [Deploying the Agent services settings](/docs/schematics?topic=schematics-agents-setup#agents-setup-svc) to override the `profile_id` input variable.
