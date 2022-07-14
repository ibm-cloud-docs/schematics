---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-14"

keywords: schematics agents, agents, set up an agents

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agents is a [Beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to, the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations) in the Beta release.
{: beta}

# Installing {{site.data.keyword.bpshort}} Agent
{: #agents-setup}

The [{{site.data.keyword.bpshort}} Agents](/docs/schematics?topic=schematics-agents-intro) installation and configuration involves the following steps: 
1. Provisioning the Agent infrastructure (estimated time 45 - 90 minutes)
2. Deploying the Agent services (estimated time 15 - 30 minutes)
3. Connecting the Agent to {{site.data.keyword.bpshort}} (estimated time 15 - 20 minutes)
4. Using the Agent to run your IaC automation (estimated time 15 - 20 minutes) 

The diagram depicts the complete {{site.data.keyword.bpshort}} Agents set-up flow that you can provision, deploy, connect, and use.

![{{site.data.keyword.bpshort}} Agents set up](images/agents-infra-setup.svg "{{site.data.keyword.bpshort}} Agents set up"){: caption=" " caption-side="center"}

## Prerequisites
{: #agents-setup-prereq}

Before you begin, complete the following prerequisites.

- Your {{site.data.keyword.cloud_notm}} account must be a paid account.
- You must have the permissions to provision VPC, {{site.data.keyword.containerlong_notm}} cluster, and logging service in a predefined resource group.

## Provision the Agent infrastructure through UI
{: #agents-setup-infra-ui}
{: ui}

You can use {{site.data.keyword.bpshort}} to provision the Agent infrastructure in your {{site.data.keyword.cloud_notm}} account. The Agent infrastructure is composed of the following resources.

    - [VPC infrastructure](https://cloud.ibm.com/docs/vpc?topic=vpc-iam-getting-started) as `public_gateways`, `subnets`.
    - [IKS](https://cloud.ibm.com/docs/containers?topic=containers-access_reference) as `vpc_kubernetes_cluster`.
    - [LogDNA](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-iam)
    - [Activity tracker](https://cloud.ibm.com/docs/activity-tracker?topic=activity-tracker-iam)

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/)
2. Navigate to **Schematics** > **Workspaces** > [**Create workspace**](https://cloud.ibm.com/schematics/workspaces/create){: external} with the following inputs to create an Agent infrastructure Workspace.
    - In **Specify Template** section:
        - **GitHub, GitLab or Bitbucket repository URL** - `https://github.com/Cloud-Schematics/schematics-agents/tree/main/templates/infrastructure`.
        - **Personal access token** - `<leave it blank>`.
        - Terraform Version - `terraform_v1.0`. **Note** you need to select Terraform verion 1.0 or greater than version.
        - Click `Next`.
    - In **Workspace details** section:
        - **Workspace name** as `schematics-agent-infra_workspace`.
        - **Tags** as `agents-infra`. 
        - **Resource group** as `default` or other resource group for this Workspace. For more information, see [Creating a resource group](/docs/account?topic=account-rgs&interface=ui). Ensure you have right access permission for the resource group.
        - **Location** as `North America` or other [region](/docs/schematics?topic=schematics-multi-region-deployment) for this Workspace. 
           If the location used for Agent infrastructure and Agent service does not match, then the logs are not sent to LogDNA.
           {: note}

        - Click `Next`.
        - Check the information entered are correct to create a Workspace.
    - Click `Create`.

3. On successful creation of the `schematics-agent-infrastructure` Workspace, review and edit the following `Agent infrastructure` input variables in the workspace **Settings** page.

    The Agent infrastructure workspace and the Agent infrastructure need not have the same input values for the **Resource Group**, **Location**, and **Tags**.
    {: note} 

    | Input variable  | Data type | Required/Optional | Description |
    |--|--|--| -- |
    | `agent_prefix` | String | Required | Provide the prefix that is used for your VPC, {{site.data.keyword.containerlong_notm}} Cluster, and Observability.
    | `location`| String | Required | The location of an Agent infrastructure. **Note** for Beta, the Agent must be deployed in a fresh provisioned VPC, {{site.data.keyword.containerlong_notm}} Cluster, and Log Analysis instance. For example, **us-south**. If the location used for Agent infrastructure and Agent service does not match, then the logs are not sent to LogDNA.|
    | `resource_group_name` | String | Required | Name for the resource group used for an Agent infrastructure and Agent service. For example, **test_agent**. For more information, see [Creating a resource group](/docs/account?topic=account-rgs&interface=ui). Ensure you have right access permission for the resource group. |
    | `ibmcloud_api_key` | String | Optional | The {{site.data.keyword.cloud_notm}} API key used to provision the {{site.data.keyword.bpshort}} Agent infrastructure resources. If not provided, resources provisions in currently logged in user credentials.|
    | `tags` | List(String) | Optional | A list of tags for an Agent infrastructure. For example, `myproject:agent`, `test:agentinfra`. You can find the provisioned resources of an Agent faster by using Tag name. |
    {: caption="{{site.data.keyword.bpshort}} Agents infrastructure inputs" caption-side="bottom"}

4. Click **Apply plan** on the `schematics-agent-infrastructure` workspace to provision the Agent infrastructure. Wait 45 - 90 minutes to provision the resource. 
5. View the **Jobs** logs and **Resources** page to monitor the resources are provisioned successfully and observe the workspace status as `ACTIVE`.
    Record the `cluster_id` and `logdna_name` from the `Outputs:` section of the Jobs log. This information are used while deploying the Agent service. If you do not observe `cluster_id` details in the Jobs log, ensure you {{site.data.keyword.cloud_notm}} has right permission to create a `VPC Infrastructure`, and `Kubernetes cluster` service access. Then, click **Apply plan** to refresh your workspace.
    {: important}

### Output
{: #agents-setup-infra-output}

Follow the steps to view the Agent infrastructure workspace setup.

1. Navigate to the [Resources list](https://cloud.ibm.com/resources/){: external} page.
2. Verify the following resources are provisioned from the resource list page.
    - **VPC > Search** `<agent_prefix>-vpc` the status as **Available**.
    - **Services and Software** > `<agent_prefix>-logdna` the status as **Active**.
    - **Clusters** > `<agent_prefix>-iks` the status as **Normal**.
   
    Optionally, you can search the provisioned resources with the tag name from the [Resources list](https://cloud.ibm.com/resources/){: external} page.
    {: note}

### Deploying the Agent services 
{: #agents-setup-svc}

You can use {{site.data.keyword.bpshort}} to deploy the Agent services on the Agent infrastructure. 
For Beta, the Agent service must be deployed in a newly provisioned Agent infrastructure.
{: note}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/).
2. Navigate to **Schematics** > **Workspaces** > [**Create workspace**](https://cloud.ibm.com/schematics/workspaces/create){: external}.
    - In **Specify Template** section:
        - **GitHub, GitLab or Bitbucket repository URL** - `https://github.com/Cloud-Schematics/schematics-agents/tree/main/templates/service`.
        - **Personal access token** - `<leave it blank>`.
        - Terraform Version - `terraform_v1.0`. **Note** you need to select Terraform verion 1.0 or greater than version.
        - Click `Next`.
    - In **Workspace details** section:
        - **Workspace name** as `schematics-agent-service`.
        - **Tags** as `agents-service`.
        - **Resource group** as `default` or other resource group for this Workspace. 
        - **Location** as `North America` or other [region](/docs/schematics?topic=schematics-multi-region-deployment) for this Workspace.
            If the location used for Agent infrastructure and Agent service does not match, then the logs are not sent to LogDNA.
           {: note}

        - Click `Next`.
        - Check the information entered are correct to create a Workspace.
    - Click `Create`.

3. On successful creation of `schematics-agent-service` workspace, review, and edit the following Agent service input variables in the workspace **Settings** page.
    The Agent service workspace and the Agent service should have the same input values for Resource Group, Location, and Tags. Use the `cluster_id`, and `logdna_name` recorded while provisioning the Agent infrastructure.
    {: note}

    | Input variable | Data type | Required/Optional | Description  | 
    |----|----| --- | -- |
    | `agent_name` | String | Required | The name of an Agent. |
    | `location` | String | Required | The [location](/docs/schematics?topic=schematics-multi-region-deployment) of an Agent services. It must be the same as the Agent infrastructure/cluster location. If the location used for Agent infrastructure and Agent service does not match, then the logs are not sent to LogDNA.|
    | `resource_group_name` | String | Required | The name of the resource group used for an Agent infrastructure and Agent service. For more information, see [Creating a resource group](/docs/account?topic=account-rgs&interface=ui). Ensure you have right access permission for the resource group.|
    | `profile_id` | String | Required | Create an IAM trusted profile ID for the Agents with the roles and actions. For more information, refer to, [creating profile_id for Agents](/docs/schematics?topic=schematics-agent-trusted-profile).|
    | `cluster_id` | String | Required | The {{site.data.keyword.containerlong_notm}} cluster ID used to run an Agent service. **Note** to provide the `cluster_id` details that you received in Agent infrastructure output logs.|
    | `logdna_name` | String | Required | The LogDNA service instance name used to send an Agent logs. **Note** to provide the `logdna_name` details that you received in Agent infrastructure output logs.|
    | `ibmcloud_api_key` | String | Optional | The {{site.data.keyword.cloud_notm}} API key used to deploy the {{site.data.keyword.bpshort}} Agent resources. If not provided, resources provisions in currently logged in user credentials.|
    {: caption="{{site.data.keyword.bpshort}} service inputs" caption-side="bottom"}

4. Click **Apply plan** on the `schematics-agent-service` workspace to deploy the Agent service. Wait 15 - 30 minutes to complete the service execution.
5. View the **Jobs** logs and **Resources** page to observe the workspace status as `ACTIVE`.

#### Output
{: #agents-svc-output}

Follow the steps to view the deployment of Agent service workspace.

1. Navigate to the target [{{site.data.keyword.cloud_notm}} clusters](https://cloud.ibm.com/kubernetes/clusters/){: external} page. Enter your `<target_iks_cluster_ID>` as part of the URL.
2. Click **Kubernetes Clusters**  page.
3. Click your `<cluster>` hyperlink > click **Kubernetes dashboard** > **Pods**.
4. Switch to your **schematics-job-runtime** namespace from the drop down box next to search icon to view `jobrunner` pod (1 instance) > Status: `Running`.
5. Switch to your **schematics-ibm-observe** namespace from the drop down box next to search icon to view `logdna-agent` pods (3 instances - one on each worker node) > Status: `Running`.
6. Switch to your **schematics-runtime** namespace from the drop down box next to search icon to view `jobx0.x0` pods (3 instances - one on each worker node) > Status: `Running`.

    You can search the Agents services and its status with the tag name from the [{{site.data.keyword.cloud_notm}} clusters](https://cloud.ibm.com/kubernetes/clusters/){: external} page.
    {: note}

## Provision the Agent infrastructure through CLI
{: #agents-setup-infra-cli}
{: cli}

Before your begin
- Setup your [CLI](/docs/schematics?topic=schematics-setup-cli).
- Install [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin).
- You have the right permission to create [VPC infrastructure](https://cloud.ibm.com/docs/vpc?topic=vpc-iam-getting-started), [IKS](https://cloud.ibm.com/docs/containers?topic=containers-access_reference) cluster, [LogDDNA](https://cloud.ibm.com/docs/log-analysis?topic=log-analysis-iam), and [Activity tracker](https://cloud.ibm.com/docs/activity-tracker?topic=activity-tracker-iam) services.

Here are the list of commands used to provision the Agent infrastructure.

- Run workspace new command to create the Agent infrastructure workspace.
   ```sh
   ibmcloud schematics workspace new --file https://github.com/Cloud-Schematics/schematics-agents/tree/main/tarfiles/create_agent_infra_workspace.json
   ```
   {: pre}

- Record your workspace ID, and template ID to [upload the tar files](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) provided for installing the Agent infrastructure. Follow the `Readme` file to extract the `.tar` file from the [Cloud-Schematics Git repository](https://github.com/Cloud-Schematics/schematics-agents/tree/main/tarfiles){: external}.
- Run workspace upload command to upload the tar file.
   ```sh
   ibmcloud schematics workspace upload  --id <provide your workspace ID> --file /Users/sundeepmulampaka/goblueprint/src/github.ibm.com/schematicsblueprint/schematics-agents/tarfiles/agent-infrastructure-templates.tar --template <provide your template_id>`
   ```
   {: pre}

- Click **Apply plan** on the `schematics-agent-infrastructure` workspace to provision the Agent infrastructure. Wait 45 - 90 minutes to provision the resource. 
- Run apply command to apply the plan to provision the Agent infrastructure.
   ```sh
   ibmcloud schematics apply --id <Provide your workspace ID>
   ```
   {: pre}

- Enter `y` for **Do you really want to perform this action? [y/N]>**. Record the generated **Activity ID**.
- Run job logs command to view your job logs.
   ```sh
   ibmcloud schematics job logs --id JOB_ID
   ```
   {: pre} 
   
   For more information, refer to, [`ibmcloud schematics job logs`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs-job).
   {: note}

- Optional: View the **Jobs** logs and **Resources** page and observe the workspace status as `ACTIVE` from [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/workspaces){: external}.
- If case of Job failure, rectify the issue in the **Settings** page of the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/workspaces){: external} and run `ibmcloud schematics refresh --id  <Provide your workspace ID>` command.
- View the **Jobs** logs and **Resources** page to observe the workspace status as `ACTIVE`.

### Output of CLI Agent infrastructure
{: #agents-infra-cli-output}

1. Navigate to the [Resources list](https://cloud.ibm.com/resources/){: external} page.
2. Verify the following resources are provisioned from the resource list page.
    - **VPC > Search** `<agent_prefix>-vpc` the status as **Available**.
    - **Services and Software** > `<agent_prefix>-logdna` the status as **Active**.
    - **Clusters** > `<agent_prefix>-iks` the status as **Normal**.
   
    Optionally, you can search the provisioned resources with the tag name from the [Resources list](https://cloud.ibm.com/resources/){: external} page.
    {: note}

### Deploying the Agent services through CLI
{: #agents-setup-svc-cli}

Before you begin

- Keep your the `cluster_id`, and `logdna_name` that are recorded from the Agent infrastructure Job logs.
- Create or use an existing trusted `profile_id`. For more information to create trusted `profile-id`, refer to [Create profile_id for Agents](/docs/schematics?topic=schematics-agent-trusted-profile).

Here are the list of commands used to create the Agent service.

- Edit the Agent service using the following input variables in the [create Agent service workspace](https://github.com/Cloud-Schematics/schematics-agents/tree/main/tarfiles/create_agent_service_workspace.json){: external} JSON file. For more information, about the input variables, refer to [Input variable for Agent service](/docs/schematics?topic=schematics-agents-setup&interface=ui#agents-setup-svc).
    | Input variable | value |
    | --- | --- |
    | `cluster_id` | provide the recorded cluster_id from Agent infrastructure job log. |
    | `resource_group_name` | provide the target resource group name, for example, default.|
    | `logdna_name` | provide the recorded cluster_id from Agent infrastructure job log.|
    | `schematics_endpoint_location` | provide your endpoint location, for example, `us`.|
    | `profile_id` | Provide your trusted profile ID.|
    | `location` | Enter the region, for example, `us-south`.|
    | `agent_name` | Enter your agent name, for example, myproject.|
    {: caption="Agent service input variable" caption-side="bottom"}

    The Agent service workspace should have the same input values for Resource Group, Location, and Tags. Use the `cluster_id`, and `logdna_name` that are recorded while provisioning the Agent infrastructure.
    {: note}

- Run workspace new command to create the Agent service workspace.
   ```sh
   ibmcloud schematics workspace new --file https://github.com/Cloud-Schematics/schematics-agents/tree/main/tarfiles/create_agent_service_workspace.json
   ```
   {: pre}

- Record your workspace ID, and template ID to [upload the tar files](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) provided for installing the Agent infrastructure. Follow the `Readme` file to extract the `.tar` file from the [Cloud-Schematics Git repository](https://github.com/Cloud-Schematics/schematics-agents/tree/main/tarfiles){: external}.
- Run upload command to upload the tar file.
   ```sh
   ibmcloud schematics workspace upload  --id <provide your workspace ID> --file /Users/sundeepmulampaka/goblueprint/src/github.ibm.com/schematicsblueprint/schematics-agents/tarfiles/agent-service-templates.tar --template <provide your template_id>
   ```
   {: pre} 

- On successful creation of `schematics-agent-svc-workspace` workspace, review, and edit the following Agent service input variables in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/workspaces){: external} > **Settings** page.
    The Agent service workspace should have the same input values for Resource Group, Location, and Tags. Use the `cluster_id`, and `logdna_name` that are recorded while provisioning the Agent infrastructure.
    {: note}

- Run apply command, and wait 15 - 30 minutes to complete the service execution.
   ```sh
   ibmcloud schematics apply --id <Provide your workspace ID>
   ```
   {: pre}

- Enter `y` for **Do you really want to perform this action? [y/N]>**. Record the generated **Activity ID**. **Note** wait for 15-30 minutes.
- Run job command. For more information, refer to, [`ibmcloud schematics job logs`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs-job).
   ```sh
   ibmcloud schematics job logs --id JOB_ID
   ```
   {: pre}
   
- Optional: View the **Jobs** logs and **Resources** page and observe the workspace status as `ACTIVE` from [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/workspaces){: external}.
- If case of Job failure, rectify the issue in the **Settings** page of the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/workspaces){: external} and run refresh command.
   ```sh
   ibmcloud schematics refresh --id  <Provide your workspace ID>
   ```
   {: pre}

- View the **Jobs** logs and **Resources** page to observe the workspace status as `ACTIVE`.

#### Output of CLI Agent service
{: #agents-svc-cli-output}

1. Navigate to the target [{{site.data.keyword.cloud_notm}} clusters](https://cloud.ibm.com/kubernetes/clusters/){: external} page. Enter your `<target_iks_cluster_ID>` as part of the URL.
2. Click **Kubernetes Clusters**  page.
3. Click your `<cluster>` hyperlink > click **Kubernetes dashboard** > **Pods**.
4. Switch to your **schematics-job-runtime** namespace from the drop down box next to search icon to view `jobrunner` pod (1 instance) > Status: `Running` or green icon showing the running status.
5. Switch to your **schematics-ibm-observe** namespace from the drop down box next to search icon to view `logdna-agent` pods (3 instances - one on each worker node) > Status: `Running` or green icon showing the running status.
6. Switch to your **schematics-runtime** namespace from the drop down box next to search icon to view `jobx0.x0` pods (3 instances - one on each worker node) > Status: `Running` or green icon showing the running status.

    You can search the Agents services and its status with the tag name from the [{{site.data.keyword.cloud_notm}} clusters](https://cloud.ibm.com/kubernetes/clusters/){: external} page.
    {: note}



## Next steps
{: #nextsteps-agentsetup}

You have completed the {{site.data.keyword.bpshort}} Agent set up.
- Now, you need to [Connect your Agent](/docs/schematics?topic=schematics-register-agent&interface=ui) with your {{site.data.keyword.bpshort}} service instance.
- For any challenges in Agents installation or configuration, refer to, [FAQ about Agent](/docs/schematics?topic=schematics-faqs-agent&interface=cli) and [Troubleshooting guide](/docs/schematics?topic=schematics-agent-crn-not-found&interface=cli).
