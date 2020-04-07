---

copyright:
  years: 2017, 2020
lastupdated: "2020-04-07"

keywords: about schematics, schematics overview, infrastructure as code, iac, differences schematics and terraform, schematics vs terraform, how does schematics work, schematics benefits, why use schematics, terraform template, schematics workspace

subcollection: schematics

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# Managing cross-workspace state access with Terraform
{: #remote-state}

You can access information about the resources that you manage in a workspace from other workspaces in your account by using the `ibm_schematics_output` data source. 
{: shortdesc}

As you manage your {{site.data.keyword.cloud_notm}} resources with {{site.data.keyword.bplong_notm}}, you might require to access resource information across workspaces. To retrieve this information in native Terraform, you use the [`remote_state` data source](https://www.terraform.io/docs/providers/terraform/d/remote_state.html){: external}. The `remote_state` data source is not supported in {{site.data.keyword.bpshort}}. Instead, you use the `ibm_schematics_output` data source to access this information. 

**How is the `ibm_schematics_output` data source different from the `remote_state` data source?** </br>
When you use the `remote_state` data source, you must configure a Terraform remote backend to connect to your Terraform workspaces. With the `ibm_schematics_output` data source, you automatically have access to the built-in {{site.data.keyword.bplong_notm}} backend and can access workspace information directly. 

**What do I need to do to access resource information in other workspaces?** </br>
Similar to the `remote_state` data source, you can only access information that you configured as output values in your Terraform template. For example, let's say you have a workspace that you used to provision a VPC. To access the VPC ID, you must define the ID as an output variable in your Terraform configuration file. Then, you use [Terraform interpolation syntax](https://www.terraform.io/docs/configuration-0-11/interpolation.html){: external} to access the VPC ID in other workspaces. 

**To use the `ibm_schematics_output` data source**:

1. Follow the example in the [getting started tutorial](/docs/schematics?topic=schematics-getting-started) to create a {{site.data.keyword.bpshort}} workspace and provision a virtual server in a VPC. As you follow the instructions, review the output variables that are defined at the end of the `vpc.tf` Terraform configuration file. 

   If you already used a different Terraform configuration file in one of your workspaces, you can use this workspace for this excercise. Make sure to add output values as outlined in this example to your configuration file so that you can access your workspace information later. 
   {: tip}
   
   ```
   ...
   output sshcommand {
     value = "ssh root@ibm_is_floating_ip.fip1.address"
   }
   
   output vpc_id {
    value = ibm_is_vpc.vpc.id
   }
   ```
   {: screen}
   
   In this example, two output values are defined, the `sshcommand` to access the virtual server instance in your VPC and the `vpc_id`. For more information about how to define output values, see [Output values](https://www.terraform.io/docs/configuration/outputs.html){: external}.
   
2. Retrieve the ID of the VPC workspace that you created. 
   - **From the console**: 
     1. From the [workspace dashboard ![External link icon](../icons/launch-glyph.svg "External link icon")](https://cloud.ibm.com/schematics/workspaces), select the VPC workspace. 
     2. Select the **Settings** tab.
     3. Find the workspace ID in the **Summary** section. 
   
   - **From the CLI**: 
     1. List available workspaces in your account. 
        ```
        ibmcloud terraform workspace list
        ```
        {: pre}
        
     2. Find the workspace ID in the **ID** column of your CLI output. 
   
3. Create another Terraform configuration file that is named `remotestate.tf` to access the output parameters of the `vpc.tf` file by using the `ibm_schematics_output` data source. Make sure to store this configuration file in a GitHub or GitLab repository.
   ```
   data "ibm_schematics_workspace" "vpc" {
     workspace_id = "<schematics_workspace_ID>"
   }
   
   data "ibm_schematics_output" "vpc" {
     workspace_id = "<schematics_workspace_ID>"
     template_id  = data.ibm_schematics_workspace.vpc.template_id.0
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
   
   <table>
   <caption>Understanding the configuration file components</caption>
   <thead>
   <th colspan=2><img src="images/idea.png" alt="Idea icon"/> Understanding the configuration file components</th>
   </thead>
   <tbody>
     <tr>
       <td><code>data.ibm_schematics_workspace.workspace_id</code></td>
       <td>Enter the ID of the VPC workspace where you defined the output values that you want to access. You need this data source to retrieve the template ID of the workspace in the <code>ibm_schematics_output</code> data source. </td>
     </tr>
     <tr>
       <td><code>data.ibm_schematics_output.workspace_id</code></td>
       <td>Enter the ID of the VPC workspace where you defined the output values that you want to access.</td>
     </tr>
     <tr>
       <td><code>data.ibm_schematics_output.template_id</code></td>
       <td>Enter <code>data.ibm_schematics_workspace.vpc.template_id.0</code> to reference the Terraform template of your workspace.</td>
     </tr>
     <tr>
       <td><code>output.output_vars.value</code></td>
         <td>Use Terraform interpolation syntax to access all output values that you defined in the <code>vpc.tf</code> file by using the <code>output_values</code> output parameter of the <code>ibm_schematics_output</code> data source. The <code>output_values</code> output parameter returns all output values as a list.  </td>
     </tr>
     <tr>
       <td><code>output.sshcomand.value</code> and <code>output.vpc_id.value</code></td>
       <td>You can access a specific value in the <code>output_values</code> list by adding the ID of the output value to your <code>ibm_schematics_output</code> data source. For example, to access the <code>vpc_id</code>, simply use <code>data.ibm_schematics_output.vpc.output_values.vpc_id</code>. </td>
     </tr>
  </tbody>
  </table>
  
4. [Create a workspace that points to the `remotestate.tf` file in your GitHub or GitLab repository](/docs/schematics?topic=schematics-workspace-setup#create-workspace). 

5. [Run your Terraform code in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources). When you review your logs, you can see the output values from your VPC workspace in the **Output** section. 

   Example output:
   ```
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
