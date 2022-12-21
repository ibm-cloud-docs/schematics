---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-21"

keywords: schematics agents, agents, set up an agent

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agents is a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.
{: beta}

# Installing {{site.data.keyword.bpshort}} Agent
{: #agents-setup}

The [{{site.data.keyword.bpshort}} Agents](/docs/schematics?topic=schematics-agents-intro) are deployed in your {{site.data.keyword.cloud}} account and configured to connect to your {{site.data.keyword.bpshort}} service instance. You can follow these steps to use the included deployment automation to provision the required Kubernetes cluster and to install an Agent in that cluster.
{: shortdesc}

The block diagram represents the {{site.data.keyword.bpshort}} Agent set up flow that you can provision, deploy, connect, and use.

![{{site.data.keyword.bpshort}} Agents set up](images/agents-setup-latest.svg "{{site.data.keyword.bpshort}} Agents set up"){: caption="{{site.data.keyword.bpshort}} Agents set up" caption-side="center"}

Following are the estimated time to complete the {{site.data.keyword.bpshort}} Agents set up.
1. Provisioning the Agent infrastructure (estimated time 45 - 90 minutes)
2. Deploying the Agent services (estimated time 15 - 30 minutes)
3. Connecting the Agent to {{site.data.keyword.bpshort}} (estimated time 15 - 20 minutes)
4. Using the Agent to run your IaC automation (estimated time 15 - 20 minutes) 

## Prerequisites
{: #agents-setup-prereq}

Before you begin, complete the following prerequisites.

- You must have an [{{site.data.keyword.cloud_notm}} Pay-As-You-Go or Subscription](https://cloud.ibm.com/registration){: external} account to proceed. For more information about managing your {{site.data.keyword.cloud_notm}}, see [Setting up your {{site.data.keyword.cloud_notm}} account](/docs/account?topic=account-account-getting-started).
- Check whether you have the permissions to [provision VPC](/docs/vpc?topic=vpc-resource-authorizations-required-for-api-and-cli-calls), [{{site.data.keyword.containerlong_notm}} cluster](/docs/containers?topic=containers-access_reference#cluster_create_permissions), and [logging service](/docs/log-analysis?topic=log-analysis-iam_manage_events) in a predefined resource group.
- Check whether you have the [permissions](/docs/schematics?topic=schematics-access#workspace-permissions) to create a workspace.
- Create an IAM trusted ID. For more information, see [Trusted Profile ID](/docs/schematics?topic=schematics-agent-trusted-profile).

## Provision the Agent infrastructure through UI
{: #agents-setup-infra-ui}
{: ui}

You can use {{site.data.keyword.bpshort}} to provision the Agent infrastructure in your {{site.data.keyword.cloud_notm}} account. The Agent infrastructure is composed of the following resources.

- [VPC infrastructure](/docs/vpc?topic=vpc-iam-getting-started) as `public_gateways`, `subnets`.
- [IKS](/docs/containers?topic=containers-access_reference) as `vpc_kubernetes_cluster`.
- [LogDNA](/docs/log-analysis?topic=log-analysis-iam)
- [Activity tracker](/docs/activity-tracker?topic=activity-tracker-iam)

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Navigate to **Schematics** > **Workspaces** > [**Create workspace**](https://cloud.ibm.com/schematics/workspaces/create){: external} with the following inputs to create an Agent infrastructure workspace.
    - In **Specify Template** section:
        - **GitHub, GitLab or `Bitbucket` repository URL** - `https://github.com/Cloud-Schematics/schematics-agents/tree/main/templates/infrastructure`.
        - **Personal access token** - `<leave it blank>`.
        - Terraform Version - `terraform_v1.0`. Note that you need to select Terraform version 1.0 or greater than version.
        - Click `Next`.
    - In **Workspace details** section:
        - **Workspace name** as `schematics-agent-infra_workspace`.
        - **Tags** as `agents-infra`. 
        - **Resource group** as `default` or other resource group for this workspace. For more information, see [Creating a resource group](/docs/account?topic=account-rgs). Ensure you have right access permission for the resource group.
        - **Location** as `North America` or other [region](/docs/schematics?topic=schematics-multi-region-deployment) for this workspace. 
           If the location used for Agent infrastructure and Agent service does not match, then the logs are not sent to LogDNA.
           {: note}

        - Click `Next`.
        - Check the information entered are correct to create a workspace.
    - Click `Create`.

3. On successful creation of the `schematics-agent-infrastructure` Workspace, review and edit the following `Agent infrastructure` input variables in the workspace **Settings** page.

    The Agent infrastructure workspace and the Agent infrastructure need not have the same input values for the **Resource Group**, **Location**, and **Tags**.
    {: note} 

    | Input variable  | Data type | Required/Optional | Description |
    |--|--|--| -- |
    | `agent_prefix` | String | Required | Provide the prefix that is used for your VPC, {{site.data.keyword.containerlong_notm}} Cluster, and Observability.
    | `location`| String | Required | The location of an Agent infrastructure. For beta, the Agent must be deployed in a fresh provisioned VPC, {{site.data.keyword.containerlong_notm}} Cluster, and Log Analysis instance. For example, **`us-south`**. If the location used for Agent infrastructure and Agent service does not match, then the logs are not sent to LogDNA.|
    | `resource_group_name` | String | Required | Name for the resource group used for an Agent infrastructure and Agent service. For example, **`test_agent`**. For more information, see [Creating a resource group](/docs/account?topic=account-rgs). Ensure you have right access permission for the resource group. |
    | `ibmcloud_api_key` | String | Optional | The {{site.data.keyword.cloud_notm}} API key used to provision the {{site.data.keyword.bpshort}} Agent infrastructure resources. If not provided, resources provisions in currently logged in user credentials.|
    | `tags` | List(String) | Optional | A list of tags for an Agent infrastructure. For example, `myproject:agent`, `test:agentinfra`. You can find the provisioned resources of an Agent faster by using Tag name. |
    {: caption="{{site.data.keyword.bpshort}} Agents infrastructure inputs" caption-side="bottom"}

4. Click **Apply plan** on the `schematics-agent-infrastructure` workspace to provision the Agent infrastructure. Wait 45 - 90 minutes to provision the resource. 
5. View the **Jobs** logs and **Resources** page to monitor the resources are provisioned successfully and observe the workspace status as `ACTIVE`.

    Record the `cluster_id` and `logdna_name` from the `Outputs:` section of the Jobs log. This information are used while deploying the Agent service. If you do not observe `cluster_id` details in the Jobs log, ensure you {{site.data.keyword.cloud_notm}} has right permission to create a `VPC Infrastructure`, and `Kubernetes cluster` service access. Then, click **Apply plan** to refresh your workspace.
    {: important}

### Expected outcome
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
For beta, the Agent service must be deployed in a newly provisioned Agent infrastructure.
{: note}

1. Log in to [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external}.
2. Navigate to **Schematics** > **Workspaces** > [**Create workspace**](https://cloud.ibm.com/schematics/workspaces/create){: external}.
    - In **Specify Template** section:
        - **GitHub, GitLab or `Bitbucket` repository URL** - `https://github.com/Cloud-Schematics/schematics-agents/tree/main/templates/service`.
        - **Personal access token** - `<leave it blank>`.
        - Terraform Version - `terraform_v1.0`. Note that you need to select Terraform version 1.0 or greater than version.
        - Click `Next`.
    - In **Workspace details** section:
        - **Workspace name** as `schematics-agent-service`.
        - **Tags** as `agents-service`.
        - **Resource group** as `default` or other resource group for this workspace. 
        - **Location** as `North America` or other [region](/docs/schematics?topic=schematics-multi-region-deployment) for this workspace.
            If the location used for Agent infrastructure and Agent service does not match, then the logs are not sent to LogDNA.
           {: note}

        - Click `Next`.
        - Check the information entered are correct to create a workspace.
    - Click `Create`.

3. On successful creation of `schematics-agent-service` workspace, review, and edit the following Agent service input variables in the workspace **Settings** page.

    The Agent service workspace and the Agent service should have the same input values for Resource Group, Location, and Tags. Use the `cluster_id`, and `logdna_name` recorded while provisioning the Agent infrastructure.
    {: note}

    | Input variable | Data type | Required/Optional | Description  | 
    |----|----| --- | -- |
    | `agent_name` | String | Required | The name of an Agent. |
    | `location` | String | Required | The [location](/docs/schematics?topic=schematics-multi-region-deployment) of an Agent services. It must be the same as the Agent infrastructure/cluster location. If the location used for Agent infrastructure and Agent service does not match, then the logs are not sent to LogDNA.|
    | `resource_group_name` | String | Required | The name of the resource group used for an Agent infrastructure and Agent service. For more information, see [Creating a resource group](/docs/account?topic=account-rgs). Ensure you have right access permission for the resource group.|
    | `profile_id` | String | Required | Create an IAM trusted profile ID for the Agents with the roles and actions. For more information, see [creating `profile_id` for Agents](/docs/schematics?topic=schematics-agent-trusted-profile).|
    | `cluster_id` | String | Required | The {{site.data.keyword.containerlong_notm}} cluster ID used to run an Agent service. You need to record the `cluster_id` details that you have received in Agent infrastructure output job logs.|
    | `logdna_name` | String | Required | The LogDNA service instance name used to send an Agent logs. You need to record the `logdna_name` details that you have received in Agent infrastructure output job logs.|
    | `ibmcloud_api_key` | String | Optional | The {{site.data.keyword.cloud_notm}} API key used to deploy the {{site.data.keyword.bpshort}} Agent resources. If not provided, resources provisions in currently logged in user credentials.|
    {: caption="{{site.data.keyword.bpshort}} service inputs" caption-side="bottom"}

4. Click **Apply plan** on the `schematics-agent-service` workspace to deploy the Agent service. Wait 15 - 30 minutes to complete the service execution.
5. View the **Jobs** logs and **Resources** page to observe the workspace status as `ACTIVE`.

#### Expected outcome
{: #agents-svc-output}

Follow the steps to view the deployment of Agent service workspace.

1. Navigate to the target [{{site.data.keyword.cloud_notm}} clusters](https://cloud.ibm.com/kubernetes/clusters/){: external} page. Enter your `<target_iks_cluster_ID>` as part of the URL.
2. Click **Kubernetes Clusters**  page.
3. Click your cluster hyper link.
4. Click **Kubernetes dashboard** > **Pods**.
5. Switch to your **schematics-job-runtime** namespace from the drop down box next to search icon to view `jobrunner` pod (1 instance) > Status: `Running`.
6. Switch to your **schematics-ibm-observe** namespace from the drop down box next to search icon to view `logdna-agent` pods (3 instances - one on each worker node) > Status: `Running`.
7. Switch to your **schematics-runtime** namespace from the drop down box next to search icon to view `jobx0.x0` pods (3 instances - one on each worker node) > Status: `Running`.

    You can search the Agents services and its status with the tag name from the [{{site.data.keyword.cloud_notm}} clusters](https://cloud.ibm.com/kubernetes/clusters/){: external} page.
    {: note}

## Provision the Agent infrastructure through CLI
{: #agents-setup-infra-cli}
{: cli}

Before your begin
- Setup your [CLI](/docs/schematics?topic=schematics-setup-cli).
- Install [{{site.data.keyword.bpshort}} plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin).
- You have the right permission to create [VPC infrastructure](/docs/vpc?topic=vpc-iam-getting-started), [IKS](/docs/containers?topic=containers-access_reference) cluster, [LogDNA](/docs/log-analysis?topic=log-analysis-iam), and [Activity tracker](/docs/activity-tracker?topic=activity-tracker-iam) services.

Here are the list of commands used to provision the Agent infrastructure.

- Run [workspace new](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) command to create the Agent infrastructure workspace.
   ```sh
   ibmcloud schematics workspace new --file https://github.com/Cloud-Schematics/schematics-agents/tree/main/tarfiles/create_agent_infra_workspace.json
   ```
   {: pre}

- Record your workspace ID, and template ID to [upload the tar files](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) provided for installing the Agent infrastructure. Follow the `Readme` file to extract the `.tar` file from the [Cloud-Schematics Git repository](https://github.com/Cloud-Schematics/schematics-agents/tree/main/tarfiles){: external}.
- Run [workspace upload](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command to upload the tar file.
   ```sh
   ibmcloud schematics workspace upload  --id <provide yourworkspace ID> --file </provide-path-where-tar-fileresides>/schematics-agents/tarfiles/agent-infrastructure-templates.tar --template <provide your template_id>`
   ```
   {: pre}

- Run [apply command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-apply) to provision the Agent infrastructure.
   ```sh
   ibmcloud schematics apply --id <Provide yourworkspace ID>
   ```
   {: pre}

   Wait 45 - 90 minutes to provision the resource.
   {: note}

- Enter `y` for **Do you really want to perform this action? [y/N]>**. Record the generated **Activity ID**.
- Run [job logs](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs-job) command to view your job logs.
   ```sh
   ibmcloud schematics job logs --id JOB_ID
   ```
   {: pre} 

- Optional: View the **Jobs** logs and **Resources** page and observe the workspace status as `ACTIVE` from [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/workspaces){: external}.
- If case of job failure, rectify the issue in the **Settings** page of the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/workspaces){: external} and run `ibmcloud schematics refresh --id  <Provide yourworkspace ID>` command.
- View the **Jobs** logs and **Resources** page to observe the workspace status as `ACTIVE`.

    Record the `cluster_id` and `logdna_name` from the `Outputs:` section of the Jobs log. This information are used while deploying the Agent service. If you do not observe `cluster_id` details in the Jobs log, ensure you {{site.data.keyword.cloud_notm}} has right permission to create a `VPC Infrastructure`, and `Kubernetes cluster` service access. Then, apply command to refresh your workspace](/apidocs/schematics/schematics#refresh-workspace-command).
    {: important}

- Review the output to view the [Agent infrastructure resource](/docs/schematics?topic=schematics-agents-setup#agents-setup-infra-output) provisioned.
   
    Optionally, you can search the provisioned resources with the tag name from the [Resources list](https://cloud.ibm.com/resources/){: external} page.
    {: note}

### Deploying the Agent services through CLI
{: #agents-setup-svc-cli}

Before you begin

- Keep your the `cluster_id`, and `logdna_name` that are recorded from the Agent infrastructure Job logs.
- Create or use an existing trusted `profile_id`. For more information to create trusted `profile-id`, refer to [Create `profile_id` for Agents](/docs/schematics?topic=schematics-agent-trusted-profile).

Here are the list of commands used to create the Agent service.

- Edit the Agent service using the following input variables in the [create Agent service workspace](https://github.com/Cloud-Schematics/schematics-agents/blob/main/tarfiles/create_agent_service_workspace.json){: external} JSON file. For more information about the input variables, refer to [Input variable for Agent service](/docs/schematics?topic=schematics-agents-setup#agents-setup-svc).
    | Input variable | value |
    | --- | --- |
    | `cluster_id` | provide the recorded `cluster_id` from Agent infrastructure job log. |
    | `resource_group_name` | provide the target resource group name, for example, default.|
    | `logdna_name` | provide the recorded `logdna_name` from Agent infrastructure job log.|
    | `schematics_endpoint_location` | provide your endpoint location, for example, `us`.|
    | `profile_id` | Provide your trusted profile ID.|
    | `location` | Enter the region, for example, `us-south`.|
    | `agent_name` | Enter your agent name, for example, `myproject`.|
    {: caption="Agent service input variable" caption-side="bottom"}

    The Agent service workspace should have the same input values for Resource Group, Location, and Tags. Use the `cluster_id`, and `logdna_name` that are recorded while provisioning the Agent infrastructure.
    {: note}

- Run [workspace new](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) command to create the Agent service workspace.
   ```sh
   ibmcloud schematics workspace new --file <Provide the path where the JSON file resides>/create_agent_service_workspace.json
   ```
   {: pre}

- Record your workspace ID, and template ID to [upload the tar files](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) provided for installing the Agent infrastructure. Follow the `Readme` file to extract the `.tar` file from the [Cloud {{site.data.keyword.bpshort}} Git repository](https://github.com/Cloud-Schematics/schematics-agents/tree/main/tarfiles){: external}.
- Run [workspace upload](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) to upload the tar file.
   ```sh
   ibmcloud schematics workspace upload  --id <provide yourworkspace ID> --file </provide-path-where-tar-fileresides>/schematics-agents/tarfiles/agent-service-templates.tar --template <provide your template_id>
   ```
   {: pre} 

- On successful creation of `schematics-agent-svc-workspace` workspace, review, and edit the following Agent service input variables in the [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/workspaces){: external} > **Settings** page.
    The Agent service workspace should have the same input values for Resource Group, Location, and Tags. Use the `cluster_id`, and `logdna_name` that are recorded while provisioning the Agent infrastructure.
    {: note}

- Run [apply command](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-apply) to provision cloud resource.
   ```sh
   ibmcloud schematics apply --id <Provide your workspace ID>
   ```
   {: pre}

- Enter `y` for **Do you really want to perform this action? [y/N]>**. Record the generated **Activity ID**. Wait for 15-30 minutes.
   Wait 15 - 30 minutes to complete the service execution.
   {: note}

- Run [job log](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-logs-job) command to view your job logs.
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

- Review the output to view the [Agent service workspace](/docs/schematics?topic=schematics-agents-setup#agents-svc-output) execution.

    You can search the Agent services and its status with the tag name from the [{{site.data.keyword.cloud_notm}} clusters](https://cloud.ibm.com/kubernetes/clusters/){: external} page.
    {: note}

## Provision the Agent infrastructure through API
{: #agents-setup-infra-api}
{: api}

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.
2. You have the right permission to create [VPC infrastructure](/docs/vpc?topic=vpc-iam-getting-started), [IKS](/docs/containers?topic=containers-access_reference) cluster, [LogDNA](/docs/log-analysis?topic=log-analysis-iam), and [Activity tracker](/docs/activity-tracker?topic=activity-tracker-iam) services.

Here are the list of CURL commands used to provision the Agent infrastructure:
1. Run a workspace create command.
   ```curl
    curl -X POST \
    https://schematics.cloud.ibm.com/v1/workspaces \
    -H 'authorization: <auth> \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    -d '{
        "name": "smulampa-schematics-agent-infra-workspace",
        "type": [
            "terraform_v1.0"
        ],
        "description": "schematics agents infra workspace",
        "template_repo": {
            "url": ""
        },
        "template_data": [{
            "folder": ".",
            "type": "terraform_v1.0",
            "variablestore": [{
                    "name": "agent_prefix",
                    "type": "string",
                    "value": "myproject-agent"
                },
                {
                    "name": "location",
                    "type": "string",
                    "value": "us-south"
                },
                {
                    "name": "resource_group_name",
                    "type": "string",
                    "value": "Default"
                }
            ]
        }]
    }'
    ```
   {: codeblock}

2. Run [workspace get](/apidocs/schematics/schematics#get-workspace) to record the workspace ID.
   ```curl
    curl -X GET \
    https://schematics.cloud.ibm.com/v1/workspaces/ \
    -H 'authorization: <auth> \
    -H 'cache-control: no-cache' \
    ```
   {: pre}

3. Run [workspace upload](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) to upload a `agent-infrastructure-templates.tar` file that contains the blueprint template, inputs details.
   ```curl
    curl -X PUT \
    https://schematics.cloud.ibm.com/v1/workspaces/{w_id}/template_data/{template_id}/template_repo_upload \
    -H 'authorization: <auth> \
    -H 'cache-control: no-cache' \
    -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
    -F file=@agent-infrastructure-templates.tar
    ```
   {: pre}

4. Run [workspace get](/apidocs/schematics/schematics#get-workspace) command to pull the workspace details that are uploaded by the `agent-infrastructure-templates.tar` file.
   ```curl
    curl -X GET \
    https://schematics.cloud.ibm.com/v1/workspaces/{w_id} \
    -H 'authorization: <auth> ' \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    ```
   {: pre}

   Wait 45 - 90 minutes to provision the resource. 
   {: note}

5. Run [apply command](/apidocs/schematics/schematics#apply-workspace-command) to provision an Agent infrastructure.
   ```curl
    curl -X PUT \
    https://schematics.cloud.ibm.com/v1/workspaces/{w_id}/apply \
    -H 'authorization: <auth>' \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    -d '{
        "name": "smulampa-schematics-agent-infra-workspace",
        "type": [
            "terraform_v1.0"
        ],
        "description": "schematics agents infra workspace",
        "template_repo": {
            "url": ""
        },
        "template_data": [{
            "folder": ".",
            "type": "terraform_v1.0",
            "variablestore": [{
                    "name": "agent_prefix",
                    "type": "string",
                    "value": "myproject-agent"
                },
                {
                    "name": "location",
                    "type": "string",
                    "value": "us-south"
                },
                {
                    "name": "resource_group_name",
                    "type": "string",
                    "value": "Default"
                }
            ]
        }]
    }'
    ```
   {: pre}

6. Run [job logs](/apidocs/schematics/schematics#get-job) to pull your workspace logs.
   ```curl
    curl -X GET \
    https://schematics.cloud.ibm.com/v1/workspaces/{w_id}/runtime_data/{template_id}/log_store \
    -H 'authorization: <auth>' \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    ```
   {: pre}

    Record the `cluster_id` and `logdna_name` from the `Outputs:` section of the Jobs log. This information are used while deploying the Agent service. If you do not observe `cluster_id` details in the Jobs log, ensure you {{site.data.keyword.cloud_notm}} has right permission to create a `VPC Infrastructure`, and `Kubernetes cluster` service access. Then, apply command to refresh your workspace](/apidocs/schematics/schematics#refresh-workspace-command).
    {: important}

7. Review the output to view the [Agent infrastructure resource](/docs/schematics?topic=schematics-agents-setup#agents-setup-infra-output) provisioned.

### Deploying the Agent services
{: #agents-setup-svc-api}

Here are the list of CURL commands used to provision the Agent Service:
1. Run [workspace create](/apidocs/schematics/schematics#create-workspace) using the payload.
   ```curl
    curl -X POST \
    https://schematics.cloud.ibm.com/v1/workspaces \
    -H 'authorization: <auth>' \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    -d '{
        "name": "smulampa-schematics-agent-service-workspace",
        "type": [
            "terraform_v1.0"
        ],
        "description": "schematics agents service workspace",
        "template_repo": {
            "url": ""
        },
        "template_data": [{
            "folder": ".",
            "type": "terraform_v1.0",
            "variablestore": [{
                    "name": "agent_prefix",
                    "type": "string",
                    "value": "myproject-agent"
                },
                {
                    "name": "location",
                    "type": "string",
                    "value": "us-south"
                },
                {
                    "name": "resource_group_name",
                    "type": "string",
                    "value": "Default"
                }
            ]
        }]
    }'
    ```
   {: pre}

2. Run [workspace get](/apidocs/schematics/schematics#get-workspace) to list the workspaces.
   ```curl
    curl -X GET \
    https://schematics.cloud.ibm.com/v1/workspaces/{w_id} \
    -H 'authorization: <auth>' \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    ```
   {: pre}

3. Run [workspace upload](/apidocs/schematics/schematics#template-repo-upload) to upload `agent-service-templates.tar` file.
   ```curl
    curl -X PUT \
    https://schematics.cloud.ibm.com/v1/workspaces/{w_id}/template_data/{template_id}/template_repo_upload \
    -H 'authorization: <auth>' \
    -H 'cache-control: no-cache' \
    -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
    -F file=@agent-service-templates.tar
    ```
   {: pre}

4. Run [workspace get](/apidocs/schematics/schematics#get-workspace) operation to pull the workspace details from the `.tar` file.
   ```curl
    curl -X GET \
    https://schematics.cloud.ibm.com/v1/workspaces/{w_id} \
    -H 'authorization: <auth> ' \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    ```
   {: pre}

5. Run [apply command](/apidocs/schematics/schematics#apply-workspace-command) to provision an Agent service.
    ```curl
        curl -X PUT \
        https://schematics.cloud.ibm.com/v1/workspaces/{w_id}/apply \
        -H 'authorization: <auth>' \
        -H 'cache-control: no-cache' \
        -H 'content-type: application/json' \
        -d '{
            "name": "smulampa-schematics-agent-service-workspace",
            "type": [
                "terraform_v1.0"
            ],
            "description": "schematics agents service workspace",
            "template_repo": {
                "url": ""
            },
            "template_data": [{
                "folder": ".",
                "type": "terraform_v1.0",
                "variablestore": [{
                        "name": "agent_prefix",
                        "type": "string",
                        "value": "myproject-agent"
                    },
                    {
                        "name": "location",
                        "type": "string",
                        "value": "us-south"
                    },
                    {
                        "name": "resource_group_name",
                        "type": "string",
                        "value": "Default"
                    }
                ]
            }]
        }'
    ```
    {: pre}

     Wait 15 - 30 minutes to complete the service execution
     {: note}

6. Run [job logs](/apidocs/schematics/schematics#get-job) to pull your workspace logs.

    ```curl
    curl -X GET \
    https://schematics.cloud.ibm.com/v1/workspaces/workspaces/{w_id}/runtime_data/{template_id}/log_store \
    -H 'authorization: <auth>' \
    -H 'cache-control: no-cache' \
    -H 'content-type: application/json' \
    ```
    {: pre}

7. Review the output to view the [Agent service workspace](/docs/schematics?topic=schematics-agents-setup#agents-svc-output) execution.

## Next steps
{: #nextsteps-agentsetup}

You have completed the {{site.data.keyword.bpshort}} Agent set up.
- Now, you need to [Connect your Agent](/docs/schematics?topic=schematics-register-agent) with your {{site.data.keyword.bpshort}} service instance.
- For any challenges in Agents installation or configuration, see [FAQ about Agent](/docs/schematics?topic=schematics-faqs-agent&interface=cli) and [Troubleshooting guide](/docs/schematics?topic=schematics-agent-crn-not-found&interface=cli).
