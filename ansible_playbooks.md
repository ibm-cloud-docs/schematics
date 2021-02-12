---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-12"

keywords: schematics, automation, terraform provider, terraform, ansible code, ansible

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
{:beta: .beta}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}
{:support: data-reuse='support'}
{:help: data-hd-content-type='help'}

# Running Ansible playbook against a {{site.data.keyword.vsi_is_short}} instance
{: #ansible}

   The open beta release of Ansible support is now available in {{site.data.keyword.bplong_notm}} to IBM users.Contact your IBM Cloud Schematics Technical Offering Manager [Sai Vennam](mailto:svennam@us.ibm.com), if you are interested in getting early access to this beta offering. For more information, see [Beta limitations](/docs/schematics?topic=schematics-schematics-limitations#beta-limitations).
  {: beta}

You can run Ansible playbook against a {{site.data.keyword.vsi_is_short}} instance that you created by using {{site.data.keyword.bpshort}} by following these steps.
{: shortdesc}



 
1. [Generate an SSH key](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-ssh-keys). The SSH key is required to provision the VPC virtual server instance in {{site.data.keyword.bpshort}}, and is used to securely connect to your instance and install more software onto the virtual server instance later. After you created your SSH key, make sure to [upload this SSH key](/docs/vpc-on-classic-vsi?topic=vpc-on-classic-vsi-managing-ssh-keys#managing-ssh-keys-with-ibm-cloud-console) to your {{site.data.keyword.cloud_notm}} account in the VPC zone and resource group where you want to create your virtual server instance.

2. {{site.data.keyword.bpshort}} actions work with the default folder structure for Ansible playbooks and roles.  Roles are placed in the `/roles` folder and playbooks to be executed are placed in the root folder. The required roles is downloaded and stored in the `/roles` folder.

3. Prepare the Ansible playbook files that you want to run and download the required [Ansible roles](https://galaxy.ansible.com/){: external}. Actions allows multiple playbooks to be placed in the root folder, to allow a single repository to support multiple use cases. When the Action is created, the user can select the required playbook from the list of executable playbooks in the repository to determine the runtime behaviour. For information about how to create Ansible configuration files, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html){: external}. Download a sample [Ansible playbook with the VSI related configuration to start or stop VSI](https://github.com/Cloud-Schematics/ansible-is-instance-actions) for reference.

```
   ├── site.yaml
   ├── roles    
      └── roles        
         └── tasks                
         └── main.yml
   ```
   {: screen}

  For more information, about playbook creation refer to [create playbook](/docs/schematics?topic=schematics-create-playbooks).
  {: note}


   
4.  Follow the steps to [create an Ansible playbook](/docs/schematics?topic=schematics-create-playbooks) and to [run your Ansible playbook](/docs/schematics?topic=schematics-create-playbooks#run-ansible-playbook)by using {{site.data.keyword.bplong_notm}}.

