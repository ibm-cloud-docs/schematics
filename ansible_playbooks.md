---

copyright:
  years: 2017, 2020
lastupdated: "2020-06-12"

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
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# Running Ansible code on Terraform-provided infrastructure
{: #ansible}

To run Ansible playbooks on compute hosts that you created with {{site.data.keyword.bplong_notm}}, you can use the {{site.data.keyword.bpshort}} Ansible provisioner. 
{: shortdesc}

**What is Ansible?**</br>
[Ansible](){: external} is a configuration management and provisioning tool, and is designed to automate multi-tier app deployments and provisioning in the cloud. Written in Python, Ansible uses YAML syntax to describe automation tasks, such as the installation of software on a virtual machine. 

**How does Ansible work?**</br>
Ansible does not use agents or a custom security infrastructure that must be present on a target machine to work properly. Instead, Ansible connects to compute hosts over the private network by using SSH keys. The SSH key can be preconfigured on the virtual server instance when you order the infrastructure in {{site.data.keyword.cloud_notm}} so that you can use Ansible right away after your virtual server instance is provisioned. 

Ansible models software packages, configuration, and services as resources on a managed host to ensure that the resource is in a desired state. To bring a resource to the desired state, Ansible pushes modules to the managed host to run the required tasks. After the tasks are executed, the result is returned to the Ansible server and the module is removed from the managed host. You can use Ansible modules to execute a specific operation or group scripts and configurations in an Ansible playbook that you can execute. Ansible modules are idempotent such that executing the same playbook or operation multiple times returns the same result as resources are changed only if required.

**How can I use the {{site.data.keyword.bpshort}} Ansible provisioner to run Ansible code against a virtual server instance?** </br>
You can use the `ibm_is_instance` and `null_resource` resources to specify the Ansible provisioner and connect to a virtual server instance to run Ansible code against the instance. 


## Running Ansible code against a {{site.data.keyword.vsi_is_short}} instance
{: #null-resource}

You can run Ansible code against a {{site.data.keyword.vsi_is_short}} instance that you created with the Terraform engine in {{site.data.keyword.bpshort}} by using the Ansible provisioner. 
{: shortdesc}

The example in this topic describes how to run Ansible code against a {{site.data.keyword.vsi_is_short}} instance. If you plan to run Ansible code against a classic virtual server instance, see [this blog]() for more information about how to configure the provisioner. 
{: tip}

1. [Generate an SSH key](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-ssh-keys). The SSH key is required to provision the VPC virtual server instance in {{site.data.keyword.bpshort}}, and is used by the Ansible provisioner to securely connect to your instance and install additional software onto the virtual server instance later. After you created your SSH key, make sure to [upload this SSH key](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-managing-ssh-keys#managing-ssh-keys-with-ibm-cloud-console) to your IBM Cloud account in the VPC zone and resource group where you want to create your virtual server instance.

2. Prepare the Ansible configuration files that you want to run and download the required [Ansible roles](https://galaxy.ansible.com/){: external}. For information about how to create Ansible configuration files, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html){: external}.  

3. Upload your configuration files and roles to the same GitHub or GitLab repository where you stored your Terraform configuration files. Make sure that your folder structure looks similar to the following: 
   ```
   ├── myapp
   │  └── outputs.tf
   │  └── main.tf
   │  └── vars.tf
   ├── ansible-data    
      ├── playbooks    
      │  └── install-tree.yml   
      │  └── play.yaml     
      └── roles        
         └── tree            
         └── tasks                
         └── main.yml
   ```
   {: screen}

4. In your Terraform configuration file, configure the Ansible provisioner to run Ansible code against your {{site.data.keyword.vsi_is_short}} instance. You can attach the Ansible provisioner to your virtual server definition by adding the provisioner details to your `ibm_is_instance` directly. You can also use the `null_resource` to reference one or multiple virtual server instances in other workspaces or other configuration files. In both cases, the Ansible provisioner runs the Ansible code that you define against the virtual server instance after it is created. 

   The following code snippets are based on the Terraform code of the {{site.data.keyword.bpshort}} [getting started tutorial](/docs/schematics?topic=schematics-getting-started) and show only the resources that you need to modify to add the Ansible provisioner to the {{site.data.keyword.vsi_is_short}} instance. 
   {: note}
   
   **Example code snippet to attach the Ansible provisioner to the `ibm_is_instance`**: 
   ```
   resource "ibm_is_instance" "vs1_ansible" {
   
     resource ibm_is_instance "vsi1" {
      name    = "vsi1"
      resource_group = data.ibm_resource_group.group.id
      vpc     = ibm_is_vpc.vpc.id
      zone    = "us-south-1"
      keys    = [data.ibm_is_ssh_key.ssh_key_id.id]
      image   = data.ibm_is_image.ubuntu.id
      profile = "cc1-2x4"

      primary_network_interface {
       subnet          = ibm_is_subnet.subnet1.id
       security_groups = [ibm_is_security_group.sg1.id]
       }
     }

     connection {
       private_key = var.ssh_private_key
     }
   
     provisioner "ansible" {
       plays {
          playbook {
            file_path = path.module/ansible-data/playbooks/play.yml
            roles_path = [path.module/ansible-data/roles]
          }
       }
      }
      ```
      {: pre}
   
   **Example code snippet for using the `ibm_null_resource`**: 
   ```
   resource "ibm_is_instance" "vsi_ansible" {
   ...
   }

   resource "null_resource" "ansible" {

      connection = {
        private_key = var.ssh_private_key
      }
   
      provisioner "ansible" {
        plays {
           playbook = {
             file_path = path.module/ansible-data/playbooks/play.yml
             roles_path = [path.module/ansible-data/roles]
           }
        
           groups = ["servers"]
           hosts = [ibm_is_instance.vsi_ansible.ipv4_address]
        }
      }
     }
     ```
     {: pre}
     
     <table>
     <thead>
     <th></th>
     <th></th>
     </thead>
     <tbody>
      <tr>
      <td><code>plays</code></td>
      <td>A <code>play</code> block configures an Ansible playbook that you want to run against a virtual server instance.</td>
      </tr>
      <tr>
       <td><code>plays.playbook.file_path</code></td>
        <td>The file path to the Ansible playbook.</td>
      </tr>
      <tr>
       <td><code>plays.playbook.role_path</code></td>
        <td>The file path to the Ansible roles that you downloaded.</td>
      </tr>
      <tr>
       <td><code>plays.hosts</code></td>
       <td>A list of virtual server instances where you want to run Ansible playbooks. To execute the playbook, you must provide the IP address of the virtual server instance. </td>
      </tr>
      </tbody>
      </table>
   
5.  Follow the steps to [create a {{site.data.keyword.bpshort}} workspace](/docs/schematics?topic=schematics-workspace-setup#create-workspace) and to [run your Terraform and Ansible code in {{site.data.keyword.cloud_notm}}](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources). 

   

