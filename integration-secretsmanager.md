---

copyright:
  years: 2017, 2025
lastupdated: "2025-10-31"

keywords: secrets manager, s2s authentication, schematics integration

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Integrating {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}}
{: #sm-integration}

{{site.data.keyword.bpfull}} supports integration with [{{site.data.keyword.secrets-manager_full}}](/docs/secrets-manager?topic=secrets-manager-key-value&interface=ui) that allows you to securely manage sensitive information without displaying actual secret values in {{site.data.keyword.bpshort}} configurations. Instead of hardcoding secrets, you can provide the secret reference directly from {{site.data.keyword.secrets-manager_short}} to enhance security and simplify your secret rotation.
{: shortdesc}

Integrating {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}} eliminates the need to expose secret values in automation stacks such as Terraform, Ansible, and Extensions. With direct secret rotation and improved security, {{site.data.keyword.secrets-manager_short}} enhances the maintainability of your {{site.data.keyword.bpshort}} workflows.

To integrate {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}} and access the secrets. you need to create a service to service policy between {{site.data.keyword.secrets-manager_short}} and {{site.data.keyword.bpshort}} and assign `Viewer` and `Secret Reader` roles.

## Before you begin
{: #sm-integration-prerequisites}

- You must have your key-value secrets. To create the secrets, see [create {{site.data.keyword.secrets-manager_short}} instance](/docs/secrets-manager?topic=secrets-manager-create-instance&interface=ui).
- You need to configure [service to service authorization](/docs/account?topic=account-serviceauth&interface=ui#create-auth) to integrate {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}} service.
- Follow these steps to grant service to service authorization access to {{site.data.keyword.bpshort}} service.

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click **Manage** > **Access (IAM)** > **Authorizations** > **Create**.
3. Select a **Source account** as **This account**.
4. Select **Service** as **{{site.data.keyword.bpshort}}**.
5. Select **Resources** as **All resources** or **Specific resources**.
6. Select Target **Service** as {{site.data.keyword.secrets-manager_short}}.
7. Select the **Role** as **Reader** and **Secret Reader**.
8. Select **Authorize dependent services** to enable authorization to be delegated by source and dependent services.
9. Click **Authorize**.

## Integrating {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}} Terraform by using console
{: #sm-integrate-wks}
{: ui}

Follow the steps to enable {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}} to securely connect with {{site.data.keyword.bpshort}} Terraform.

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![hamburger icon](images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > **Terraform** > [**Create workspace**](https://cloud.ibm.com/schematics/workspaces/create){: external}.
    - In the **Specify Template** section:
        - **GitHub, GitLab, or `Bitbucket` repository URL** - `<provide your Terraform template Git repository URL>`.
        - **Personal access token** - `<leave it blank>`. You can click the `Open reference picker` to select your {{site.data.keyword.secrets-manager_short}} key reference. For more information, see [creating a {{site.data.keyword.secrets-manager_short}} instance](/docs/secrets-manager?topic=secrets-manager-create-instance).
3. In the Select a reference page, Select **Account**, **Service instance**, and **Secret**.
4. Click **OK**.
5. Click **Create** to create a workspace.

Observe the secret reference for an input variable, which is stored as a reference.

### Integrating {{site.data.keyword.secrets-manager_short}} in variable
{: #sm-integrate-variable}
{: ui}

Follow the steps to enable {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}} to securely update the {{site.data.keyword.bpshort}} Terraform variable.

1. Log in to [{{site.data.keyword.cloud_notm}} console](https://cloud.ibm.com/){: external}.
2. Click the **Menu** icon ![hamburger icon](images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > [**Terraform**](https://cloud.ibm.com/automation/schematics/terraform){: external}.
3. Click your workspace to edit.
4. Click **Settings**. In **Variables** click **Edit** icon to edit the `api_key` parameters.
5. In **Edit Variable**, click the `Open reference picker` to view the Select a reference page, add **, **Service instance**, and **Secret**.
6. Click **Save** to view the secret reference parameter as `ref://secrets-manager.eu-gb.Default.Secrets-Manager-POC/Default/xxx-test-apikey`.

Observe the secret reference for an input variable that is stored as a reference.

## Steps to integrate {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}} by using CLI
{: #sm-cli}
{: cli}

Follow the steps to enable {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}} to securely update the {{site.data.keyword.bpshort}} Terraform.

1. [Download and install the command-line](/docs/cli?topic=cli-install-ibmcloud-cli) and run the shared commands to target your region, create a service to service policy, create a {{site.data.keyword.secrets-manager_short}} instance, reference the secrets in your Terraform code, and apply.

    ```sh
    ibmcloud login --sso
    ibmcloud target -r <region>
    ibmcloud iam service-policy-create --source-service-name schematics --target-service-instance-name <your-secrets-manager-name> --roles "Viewer,SecretReader"
    ibmcloud secrets-manager secret-create --secret-type arbitrary --name <secret-name> --payload <secret-value>
    ```
    {: pre}

2. Reference the secret in your Terraform code.

   ```terraform
    variable "my_secret" {
    default = "ic://secrets-manager/secret-id"
   }
   ```
   {: pre}

3. Apply the {{site.data.keyword.bpshort}} workspace

   ```sh
   ibmcloud schematics workspace apply --id <WORKSPACE_ID>
   ```
   {: pre}


## Integrating {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}} Terraform by using API
{: #sm-api}
{: api}

Follow the steps to enable {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}} to securely connect with {{site.data.keyword.bpshort}} Terraform.

1. Follow the [steps](/docs/schematics?topic=schematics-setup-api#cs_api) to retrieve your IAM access token and authenticate with {{site.data.keyword.bplong_notm}} by using the API.

2. Create a {{site.data.keyword.secrets-manager_short}} Instance (if not already created) by using your target endpoint and IBM Cloud Resource Controller API.

   ```sh
    curl -X POST /v1/resource_instances -H "Authorization: <iam_access_token>" -d '{"name": "my-secrets-manager",
    "target": "test-eu-de",
    "resource_group": "default",
    "resource_plan_id": "<plan-id-for-secrets-manager>"
    }'
    ```
    {: pre}

3. Create a Secret in {{site.data.keyword.secrets-manager_short}} by using {{site.data.keyword.secrets-manager_short}} API.

   ```sh
    curl -X POST /api/v1/secrets/arbitrary  -H "Authorization: <iam_access_token>" -d '{"name": "my-token-secret","description": "Token for private Git repo","secret_group_id": "<secret-group-id>","resources": [],"payload": "your-token-value"}'
   ```
   {: pre}

4. Create a service to service IAM Policy

   ```json
    {
      "type": "service",
      "subjects": [{
        "attributes": [{
          "name": "serviceName",
          "value": "schematics"
        }]
      }],
      "roles": [
        { "role_id": "crn:v1:bluemix:public:iam::::role:Viewer" },
        { "role_id": "crn:v1:bluemix:public:iam::::role:SecretReader" }
      ],
      "resources": [{
        "attributes": [{
          "name": "serviceInstance",
          "value": "<secrets-manager-instance-id>"
        }]
      }]
    }
    ```
    {: codeblock}

5. Reference the secret in your Terraform variable file in by using {{site.data.keyword.bpshort}} workspace.

    ```terraform
     variable "git_token" {
     default = "ic://secrets-manager/secret-id"
    }
    ```
    {: codeblock}

6. Update the workspace with the Terraform template to reference the secret.

    ```sh
    curl -X PATCH /v1/workspaces/{workspace_id}
    ```
    {: pre}

7. Apply the workspace with the Terraform template.

    ```sh
    curl -X POST /v1/workspaces/{workspace_id}/actions/apply
    ```
    {: pre}

## Integrating {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}} Terraform by using Terraform
{: #sm-terraform}
{: terraform}

Follow the steps to enable {{site.data.keyword.secrets-manager_short}} in {{site.data.keyword.bpshort}} to securely connect with {{site.data.keyword.bpshort}} Terraform.

1. Define the [{{site.data.keyword.secrets-manager_short}} Instance](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/resource_instance){: external}.
2. Create a Secret in {{site.data.keyword.secrets-manager_short}}. You can use the CLI or manually create secrets. In Terraform, secrets are typically referenced, not created directly.
3. Create [IAM Service-to-Service Policy](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/iam_service_policy){: external}.
4. Reference the Secret in Terraform Variables. Replace `secret-id` with the actual ID of the secret stored in {{site.data.keyword.secrets-manager_short}}.

    ```terraform
     variable "git_token" {
     default = "ic://secrets-manager/secret-id"
    }
    ```
    {: codeblock}

5. Use the {{site.data.keyword.bpshort}} Terraform provider or CLI to [create your workspace](/docs/schematics?topic=schematics-sch-create-wks&interface=terraform#create-wks-terraform) with the preceding configuration.
