---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-25"

keywords: schematics, automation, terraform

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

# Getting started with IBM Cloud Schematics (beta)
{: #getting-started}

Enable Infrastructure as Code (IaC) with {{site.data.keyword.cloud_notm}} Schematics, and start templatizing, provisioning, and managing {{site.data.keyword.cloud_notm}} resources in your {{site.data.keyword.cloud_notm}} environment by using Terraform configuration files. 
{: shortdesc}

{{site.data.keyword.cloud_notm}} Schematics is available as a beta to test out Terraform-as-a-Service capabilities in {{site.data.keyword.cloud_notm}} and might be unstable or change frequently. {{site.data.keyword.cloud_notm}} beta services also might not provide the same level of performance or compatibility that generally available services provide, and are not intended for use in a production environment.
{: preview}

## Create your Terraform configuration file
{: #create-config}

Create a Terraform configuration file that specifies the {{site.data.keyword.cloud_notm}} resources that you want to provision with {{site.data.keyword.cloud_notm}} Schematics, and store the file in a public GitHub repository. 
{: shortdesc}

In this getting started tutorial, you create a Terraform configuration file to deploy a virtual server instance in a Virtual Private Cloud (VPC). You can use any other Terraform configuration file as part of this tutorial, but make sure that your file is stored in the `master` branch of a public GitHub repository. 

A virtual server instances in a VPC incurs costs. Be sure to review the available plans for [VPC virtual server instances](https://cloud.ibm.com/vpc/provision/vs){: external} before you proceed.
{: important}

**What is a Virtual Private Cloud (VPC) and what resources do I need?** </br> 
A VPC allows you to create your own space in {{site.data.keyword.cloud_notm}} so that you can run an isolated environment in the public cloud with custom network policies. To provision a virtual server in a VPC, you must set up the following infrastructure resources: 
- 1 VPC where you provision your VPC virtual server instance
- 1 security group and a rule for this security group to allow SSH connection to your virtual server instance
- 1 subnet to enable networking in your VPC
- 1 VPC virtual server instance 
- 1 floating IP address that you use to access your VPC virtual server instance over the public network

**What credentials do I need to provision a virtual server in a VPC?**</br>
The credentials that you need depend on the type of resource that you want to provision. To create a virtual server instance in a VPC, you must have an SSH key to connect to your virtual server instance.  For more information about what credentials you need for a specific {{site.data.keyword.cloud_notm}} resource, see [Retrieving required credentials for your resources](/docs/terraform?topic=terraform-setup_cli#retrieve_credentials).

**Where can I find an overview of other supported resources in {{site.data.keyword.cloud_notm}}?**</br>
{{site.data.keyword.cloud_notm}} Schematics supports all resources that are defined by the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform. To find a full list of supported {{site.data.keyword.cloud_notm}} resources, see the [{{site.data.keyword.cloud_notm}} Provider reference](https://ibm-cloud.github.io/tf-ibm-docs/){: external}.

To create a configuration file for your VPC resources: 

1. Make sure that you have the [required permissions](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources) to create and work with VPC infrastructure. 
2. [Generate an SSH key](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-ssh-keys). The SSH key is required to provision the VPC virtual server instance and you can use the SSH key to access your instance via SSH. After you created your SSH key, make sure to [upload this SSH key to your {{site.data.keyword.cloud_notm}} account](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-managing-ssh-keys#managing-ssh-keys-with-ibm-cloud-console). 
3. Create your Terraform configuration file that includes all the VPC infrastructure resources that you need to successfully run a virtual server instance in a VPC. For more information about how to structure a Terraform configuration file, see [Creating a Terraform configuration](/docs/schematics?topic=schematics-create-tf-config). 
   ```
   variable "ssh_key" {}

   locals {
        BASENAME = "<name>" 
        ZONE     = "us-south-1"
      }

   resource ibm_is_vpc "vpc" {
     name = "${local.BASENAME}-vpc"
   }

   resource ibm_is_security_group "sg1" {
     name = "${local.BASENAME}-sg1"
     vpc  = "${ibm_is_vpc.vpc.id}"
   }

   # allow all incoming network traffic on port 22
   resource "ibm_is_security_group_rule" "ingress_ssh_all" {
     group     = "${ibm_is_security_group.sg1.id}"
     direction = "ingress"
     remote    = "0.0.0.0/0"                       

     tcp = {
       port_min = 22
       port_max = 22
     }
   }

   resource ibm_is_subnet "subnet1" {
     name = "${local.BASENAME}-subnet1"
     vpc  = "${ibm_is_vpc.vpc.id}"
     zone = "${local.ZONE}"
     total_ipv4_address_count = 256
   }

   data ibm_is_image "ubuntu" {
     name = "ubuntu-18.04-amd64"
   }

   data ibm_is_ssh_key "ssh_key_id" {
     name = "${var.ssh_key}"
   }

   resource ibm_is_instance "vsi1" {
     name    = "${local.BASENAME}-vsi1"
     vpc     = "${ibm_is_vpc.vpc.id}"
     zone    = "${local.ZONE}"
     keys    = ["${data.ibm_is_ssh_key.ssh_key_id.id}"]
     image   = "${data.ibm_is_image.ubuntu.id}"
     profile = "cc1-2x4"

     primary_network_interface = {
       subnet          = "${ibm_is_subnet.subnet1.id}"
       security_groups = ["${ibm_is_security_group.sg1.id}"]
     }
   }

   resource ibm_is_floating_ip "fip1" {
     name   = "${local.BASENAME}-fip1"
     target = "${ibm_is_instance.vsi1.primary_network_interface.0.id}"
   }

   output sshcommand {
     value = "ssh root@${ibm_is_floating_ip.fip1.address}"
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
       <td><code>variable.ssh_key</code></td>
       <td>A variable declaration for the name of the SSH key that you uploaded to your {{site.data.keyword.cloud_notm}} account. You enter the value for your variable when you create your workspace in {{site.data.keyword.cloud_notm}} Schematics.</td>
     </tr>
     <tr>
       <td><code>provider.generation</code></td>
       <td>Enter <strong>1</strong> to provision VPC on Classic infrastructure resources. </td>
     </tr>
   <tr>
   <td><code>locals.BASENAME</code></td>
   <td>Enter a name that you want to append to the name of all VPC infrastructure resources that you create with this configuration file. </td>
   </tr>
     <tr>
       <td><code>locals.ZONE</code></td>
       <td>Enter a supported VPC zone where you want to create your resources. To find available zones, run <code>ibmcloud is zones</code>.  </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_vpc.name</code></td>
       <td>Enter a name for your VPC. In this example, you use <code>locals.BASENAME</code> to create part of the name. For example, if your base name is `test`, the name of your VPC is set to `test-vpc`. Keep in mind that the name of your VPC must be unique within your {{site.data.keyword.cloud_notm}} account. </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_security_group.name</code></td>
       <td>Enter a name for the security group that you create for your VPC. In this example, you use <code>locals.BASENAME</code> to create part of the name. For example, if your base name is `test`, the name of your security group is set to `test-sg1`.  </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_security_group.vpc</code></td>
       <td>Enter the ID of the VPC for which you want to create the security group. In this example, you reference the ID of the VPC that you create with the <code>ibm_is_vpc</code> resource in the same configuration file.  </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_security_group_rule.group</code></td>
       <td>Enter the ID of the security group for which you want to create a security group rule. In this example, you reference the ID of the security group that you create with the <code>ibm_is_security_group</code> resource in the same configuration file.  </td>
     </tr>
      <tr>
       <td><code>resource.ibm_is_security_group_rule.direction</code></td>
       <td>Specify if the security group rule is applied to incoming or outgoing network traffic. Choose <strong>ingress</strong> to specify a rule for incoming network traffic, and <strong>egress</strong> to specify a rule for outgoing network traffic. </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_security_group_rule.remote</code></td>
       <td>Enter the IP address range, for which the security group rule is applied. In this example, <code>0.0.0.0/0</code> allows network traffic from all IP addresses. </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_security_group_rule.tcp</code></td>
       <td>Enter the TCP port range that you want to open in your security group rule. If you want to open up a single port, enter the same port number in <code>port_min</code> and <code>port_max</code>. </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_subnet.name</code></td>
       <td>Enter a name for your subnet. In this example, you use <code>locals.BASENAME</code> to create part of the name. For example, if your base name is `test`, the name of your subnet is set to `test-subnet1`.  </td>
     </tr>
      <tr>
       <td><code>resource.ibm_is_subnet.vpc</code></td>
       <td>Enter the ID of the VPC for which you want to create the subnet. In this example, you reference the ID of the VPC that you create with the <code>ibm_is_vpc</code> resource in the same configuration file. </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_subnet.zone</code></td>
       <td>Enter the zone in which you want to create the subnet. In this example, you use <code>locals.ZONE</code> as the name for your zone. </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_subnet.</code></br><code>total_ipv4_address_count</code></td>
       <td>Enter the number of IPv4 IP addresses that you want to have in your subnet. </td>
     </tr>
     <tr>
       <td><code>data.ibm_is_image.name</code></td>
       <td>Enter the name of the operating system that you want to install on your VPC virtual server instance. You use this Terraform resource to retrieve the ID of the operating system when you specify the VPC virtual server instance. For supported image names, run <code>ibmcloud is images</code>. </td>
     </tr>
     <tr>
       <td><code>data.ibm_is_ssh_key.name</code></td>
       <td>Enter the name of the SSH key that you uploaded to your {{site.data.keyword.cloud_notm}} account. You use this Terraform resource to retrieve the ID of your SSH key when you specify the VPC virtual server instance. </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_instance.name</code></td>
       <td>Enter the name of the VPC virtual server instance that you want to create. In this example, you use <code>locals.BASENAME</code> to create part of the name. For example, if your base name is `test`, the name of your VPC virtual server instance is set to `test-vsi1`.  </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_instance.vpc</code></td>
       <td>Enter the ID of the VPC in which you want to create the VPC virtual server instance. In this example, you reference the ID of the VPC that you create with the <code>ibm_is_vpc</code> resource in the same configuration file.  </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_instance.zone</code></td>
       <td>Enter the zone in which you want to create the VPC virtual server instance. In this example, you use <code>locals.ZONE</code> as the name for your zone. </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_instance.keys</code></td>
       <td>Enter the UUID of the SSH key that you uploaded to your {{site.data.keyword.cloud_notm}} account. In this example, you retrieve the UUID from the <code>ibm_is_ssh_key</code> data source of this configuration file. Terraform uses the name of the SSH key that you define in your data source object to look up information about the SSH key in your {{site.data.keyword.cloud_notm}} account.</td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_instance.image</code></td>
       <td>Enter the ID of the image that represents the operating system that you want to install on your VPC virtual server instance. In this example, you retrieve the ID from the <code>ibm_is_image</code> data source of this configuration file. Terraform uses the name of the image that you define in your data source object to look up information about the image in the {{site.data.keyword.cloud_notm}} infrastructure portfolio. </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_instance.profile</code></td>
       <td>Enter the name of the profile that you want to use for your VPC virtual server instance. For supported profiles, run <code>ibmcloud is instance-profiles</code>. </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_instance.</code></br><code>primary_network_interface.subnet</code></td>
       <td>Enter the ID of the subnet that you want to use for your VPC virtual server instance. In this example, you use the <code>ibm_is_subnet</code> resource in this configuration file to retrieve the ID of the subnet.   </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_instance.</code></br><code><primary_network_interface.security_groups</code></td>
       <td>Enter the ID of the security group that you want to apply to your VPC virtual server instance. In this example, you use the <code>ibm_is_security_group</code> resource in this configuration file to retrieve the ID of the security group.   </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_floating_ip.name</code></td>
       <td>Enter a name for your floating IP resource. In this example, you use <code>locals.BASENAME</code> to create part of the name. For example, if your base name is `test`, the name of your floating IP resource is set to `test-fip1`.   </td>
     </tr>
     <tr>
       <td><code>resource.ibm_is_floating_ip.target</code></td>
       <td>Enter the ID of the network interface where you want to allocate the floating IP addresses. In this example, you use the <code>ibm_is_instance</code> resource to retrieve the ID of the primary network interface.   </td>
     </tr>
     <tr>
       <td><code>output.ssh_command.value</code></td>
       <td>Build the SSH command that you need to run to connect to your VPC virtual server instance. In this example, you use the <code>ibm_is_floating_ip</code> resource to retrieve the floating IP address that is assigned to your VPC virtual server instance.  </td>
     </tr>
   </tbody>
   </table>

4. Store this configuration file in the `master` branch of a public GitHub repository. During the beta, only public GitHub repositories are supported in {{site.data.keyword.cloud_notm}} Schematics. 

## Setting up your workspace
{: #setup-workspace}

Create a workspace in {{site.data.keyword.cloud_notm}} Schematics that points to the GitHub repository that hosts your Terraform configuration file. 
{: shortdesc}

Before you begin, make sure that you have a [Terraform configuration file in a public GitHub repository](#create-config) that you can use in for your workspace. 

1. From the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog?category=devops){: external}, select **Schematics**. 
2. Click **Create a workspace**. 
3. Configure your workspace. 
   1. Enter the link to your public GitHub repository. The link must point to the `master` branch in GitHub. You cannot link to other branches during the beta. 
   2. Enter a descriptive name for your workspace. To find your workspace more easily after you create it, make sure to include the microservice component and the environment in your name. For more information about how to structure your workspaces, see [Designing your workspace structure](/docs/schematics?topic=schematics-workspace-setup#structure-workspace).
   3. Optional: Enter tags for your workspace. You can use the tags later to find workspaces that are related to each other. 
   4. Select a resource group and a region for your workspace. All {{site.data.keyword.cloud_notm}} resources that you create in your workspace are provisioned in the selected resource group and region. 
   5. Optional: Enter a description for your workspace.
   6. Enter the values for your variables. When you enter the GitHub repository URL that hosts your Terraform configuration files, {{site.data.keyword.cloud_notm}} Schematics automatically parses through your files to find variable declarations. 
4. Click **Create** to create your workspace. When you create the workspace, all Terraform configuration files are loaded into {{site.data.keyword.cloud_notm}} Schematics, but your resources are not yet deployed to {{site.data.keyword.cloud_notm}}.


## Provision your {{site.data.keyword.cloud_notm}} resources
{: #provision-resources}

Use {{site.data.keyword.cloud_notm}} Schematics to run the infrastructure code in your Terraform configuration files, and to provision your {{site.data.keyword.cloud_notm}} resources. 
{: shortdesc}

Before you begin: 
- Set up your workspace in [{{site.data.keyword.cloud_notm}} Schematics](#setup-workspace). 
- If you use the sample Terraform configuration file in this tutorial, make sure that you have the [required permissions](/docs/vpc-on-classic?topic=vpc-on-classic-managing-user-permissions-for-vpc-resources) to create and work with VPC infrastructure. 

To provision your resources: 

1. From the workspace details page, click **Run new plan** to create a Terraform execution plan. This plan equals the output of the `terraform plan` command. You can review the status of your plan in the **Recent activtity** section of your workspace details page. 
2. Review the log files of your execution plan. This plan includes a summary of {{site.data.keyword.cloud_notm}} resources that must be created, modified, or deleted to achieve the state that you described in your Terraform configuration files. If you have syntax errors in your configuration files, you can review the error message in the log file. 
3. Apply your Terraform configuration by clicking **Apply plan**. This action equals the `terraform apply` command. {{site.data.keyword.cloud_notm}} Schematics starts provisioning, modifying, or deleting your {{site.data.keyword.cloud_notm}} resources based on what actions were identified in the execution plan. Depending on the type and number of resources that you want to provision, this process might take a few minutes, or even up to hours to complete. During this time, you cannot make changes to your workspace. 
4. After your resources are provisioned, review the log file to ensure that no errors occurred during the provisioning, modification, or deletion process. 
5. From the workspace details page, select the **Resources** tab to find a summary of {{site.data.keyword.cloud_notm}} resources that are available in your {{site.data.keyword.cloud_notm}} account. 

## What's next? 
{: #whats-next}

After you provisioned your resources in {{site.data.keyword.cloud_notm}}, choose between the following options: 
- Learn more about [{{site.data.keyword.cloud_notm}} Schematics](/docs/schematics?topic=schematics-about-schematics) and the benefits of using the service. 
- Review tips and tricks for how to [structure your Terraform configuration file](/docs/schematics?topic=schematics-create-tf-config).
- Set up a [GitHub repository and workspace structure](/docs/schematics?topic=schematics-workspace-setup) for your microservices. 
- Explore other {{site.data.keyword.cloud_notm}} resources that you can provision by reviewing the [{{site.data.keyword.cloud_notm}} Provider plug-in reference](https://ibm-cloud.github.io/tf-ibm-docs/){: external}. 
