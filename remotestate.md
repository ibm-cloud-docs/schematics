---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-09"

keywords: about schematics, schematics overview, infrastructure as code, iac, differences schematics and terraform, schematics vs terraform, how does schematics work, schematics benefits, why use schematics, terraform template, schematics workspace

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Remote state and cross-workspace access
{: #remote-state}

{{site.data.keyword.cloud}} provides built in [remote-state](https://developer.hashicorp.com/terraform/language/state/remote){: external} management for Terraform. Terraform state files are automatically preserved between runs and are accessible by {{site.data.keyword.cloud}} commands and operations. {{site.data.keyword.cloud}} remote-state management enables team work and workspace shared operations, with built in state locking preventing concurrent operations against the same state file. 

The built in workspace remote-state management supports a number of {{site.data.keyword.bpshort}} use cases: 
- The sharing of resource information between workspaces. This allows your infrastructure to be broken down into smaller components, with read-only resource information passed between environments using {{site.data.keyword.bpshort}} remote-state data sources. Discrete environments linked by data sources allows responsibility for different elements of infrastructure to be delegated to different teams with information shared between workspaces as read-only resources. 
- Integration of Terraform and Ansible operations with [Actions](/docs/schematics?topic=schematics-action-working). Workspace resource information can be directly passed as an Ansible [dynamic inventory](/docs/schematics?topic=schematics-inventories-setup#dynamic-inv) without the need for manual host inventory creation or use of inventory scripts.    
- Management of large-scale IaC environments. [Blueprints](/docs/schematics?topic=schematics-blueprint-intro) enables large environments to be composed from modular Terraform environments by the [direct passing of values](/docs/schematics?topic=schematics-blueprint-templates#blueprint-module-outputs) and state information between deployed modules. 
{: shortdesc}


## Accessing workspace state and outputs
{: #data-sources}

You can access information about the resources that you manage in a workspace from other workspaces in your account by using the [`ibm_schematics_output`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/schematics_output){: external} and [`ibm_schematics_state`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/schematics_state){: external} data sources.

{{site.data.keyword.cloud}} uses the `local` built-in Terraform state support and does not use Terraform [backend](https://developer.hashicorp.com/terraform/language/settings/backends/configuration){: external} support. No additional configuration is required within your Terraform configs to enable {{site.data.keyword.cloud}} remote state management. {{site.data.keyword.bpshort}} does not use the Terraform `remote_state` data source, instead you use the `ibm_schematics_output` data source to access the information. 

**How is the `ibm_schematics_state` data source different from the `remote_state` data source?** </br>
When you use the Terraform `remote_state` data source, you must configure a Terraform remote backend to connect to your Terraform workspaces. With the `ibm_schematics_state` data source, you automatically have access to the built-in {{site.data.keyword.bpshort}} backend and can access workspace information directly.

**What do I need to do to access resource information in other workspaces?** </br>
Similar to the `remote_state` data source, you can only access information that you configured as output values in your Terraform template. For example, let's say you have a workspace that you used to provision a VPC. To access the VPC ID, you must define the ID as an output variable in your Terraform configuration file.

**To use the `ibm_schematics_output` data source**:

1. Follow the example in the [getting started tutorial](/docs/schematics?topic=schematics-getting-started) to create a {{site.data.keyword.bpshort}} Workspaces and provision a virtual server in a VPC. As you follow the instructions, review the output variables that are defined at the end of the `vpc.tf` Terraform configuration file. 

    If you already used a different Terraform configuration file in one of your workspaces, you can use this workspace for the exercise. Make sure to add output values as outlined in this example to your configuration file so that you can access your workspace information later. 
    {: tip}

    ```terraform
    output sshcommand {
        value = "ssh root@ibm_is_floating_ip.fip1.address"
    }

    output vpc_id {
    value = ibm_is_vpc.vpc.id
    }
    ```
    {: screen}

    In this example, two output values are defined, the `sshcommand` to access the virtual server instance in your VPC and the `vpc_id`.

2. Retrieve the ID of the VPC workspace that you created. 

    **Through console**:
    - From the [workspace dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/schematics/workspaces), select the VPC workspace.
    - Select the **Settings** tab.
    - Find the workspace ID in the **Summary** section. 

    **Through CLI**:
    - List available workspaces in your account.
        ```sh
        ibmcloud schematics workspace list
        ```
        {: pre}

    - Find the workspace ID in the **ID** column of your command-line output. 

    The {{site.data.keyword.bpshort}} `ibmcloud terraform` command usage displays warning and deprecation message as **Alias 'terraform' will be deprecated. Use 'schematics' or 'sch' in your commands.**
    {: note}

3. Create another Terraform configuration file that is named `remotestate.tf` to access the output parameters of the `vpc.tf` file by using the `ibm_schematics_output` data source. To allow version control of this file, make sure to store this configuration file in a GitHub or GitLab repository. 
    ```terraform
    data "ibm_schematics_workspace" "vpc" {
        workspace_id = "<schematics_workspace_ID>"
    }

    data "ibm_schematics_output" "vpc" {
        workspace_id = "<schematics_workspace_ID>"
        template_id = data.ibm_schematics_workspace.vpc.runtime_data.0.id
    }

    output "output_vars" {
        value = data.ibm_schematics_output.vpc.output_values
    }

    output "ssh_command" {
        value = data.ibm_schematics_output.vpc.output_values.sshcommand
    }

    output "vpc_id" {
        value = data.ibm_schematics_output.vpc.output_values.vpc_id
    }
    ```
    {: codeblock}

    | ![Idea icon](images/idea.png) | Understanding the configuration file components |
    | -- | -- |
    | `data.ibm_schematics_workspace.workspace_id` | Enter the ID of the VPC workspace where you defined the output values that you want to access. You need this data source to retrieve the template ID of the workspace in the `ibm_schematics_output` data source. |
    | `data.ibm_schematics_output.workspace_id` | Enter the ID of the VPC workspace where you defined the output values that you want to access. |
    | `data.ibm_schematics_output.template_id` | Enter `data.ibm_schematics_workspace.vpc.runtime_data.0.id` to reference the Terraform template of your workspace. |
    | `output.output_vars.value` | Use Terraform interpolation syntax to access all output values that you defined in the `vpc.tf` file by using the `output_values` output parameter of the `ibm_schematics_output` data source. The `output_values` output parameter returns all output values as a list. |
    | `output.sshcomand.value`  and `output.vpc_id.value` | You can access a specific value in the `output_values` list by adding the ID of the output value to your `ibm_schematics_output` data source. For example, to access the `vpc_id`, simply use `data.ibm_schematics_output.vpc.output_values.vpc_id`. |
    {: caption="Configuration file components" caption-side="bottom"}

4. [Create a workspace](/docs/schematics?topic=schematics-workspace-setup#create-workspace) that points to the `remotestate.tf` file in your GitHub or GitLab repository. If you want to upload a tape archive file (`.tar`) from your local machine instead, use the [`ibmcloud schematics workspace upload`](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-upload) command.

5. [Run your Terraform code in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources). When you review your logs, you can see the output values from your VPC workspace in the **Output** section. 

    **Example output**
    ```text
    ...
    2020/02/21 19:49:30 Terraform show | Outputs:
    2020/02/21 19:49:30 Terraform show | 
    2020/02/21 19:49:30 Terraform show | Output_vars = {
    2020/02/21 19:49:30 Terraform show |   sshcommand = ssh root@169.61.225.111
    2020/02/21 19:49:30 Terraform show |   vpc_id = a1a11aa1-a111-a11a-a111-aa1aa11a1a1a
    2020/02/21 19:49:30 Terraform show | }
    2020/02/21 19:49:30 Terraform show | sshcommand = ssh root@169.61.225.111
    2020/02/21 19:49:30 Terraform show | vpc_id = a1a11aa1-a111-a11a-a111-aa1aa11a1a1a
    ```
    {: screen}


