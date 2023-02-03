---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-03"

keywords: schematics actions, actions, schematics

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} Actions
{: #sc-actions}

{{site.data.keyword.bpshort}} Actions delivers Ansible-as-a-Service capabilities for you to automate configuration and management of your {{site.data.keyword.cloud_notm}} environment, and deploy complex multitiered apps to your cloud infrastructure. 
{: shortdesc}

To get started with [Configuration Management]({: /docs/schematics?topic=schematics-schematics-open-projects#sc-iac-cm}) in {{site.data.keyword.bpshort}}, see [Getting started tutorial](/docs/schematics?topic=schematics-getting-started-ansible). 
{: tip}

## Architecture
{: #sc-actions-overview}


[Ansible](https://www.ansible.com/){: external} is a [configuration management and provisioning tool](/docs/schematics?topic=schematics-schematics-open-projects). The blog [Infrastructure as Code: Chef, Ansible, Puppet, or Terraform?](https://www.ibm.com/cloud/blog/chef-ansible-puppet-terraform) provides an overview of several popular open-source IaC tools and summarizes their capabilities and relative strengths. 

 It is designed to automate the configuration, operation, and management of cloud environments, and to deploy multitiered app workloads in the cloud. Ansible uses YAML syntax to describe the tasks that must be run against a single host or a group of hosts, and stores these tasks in an Ansible playbook. 

Ansible does not use agents or a custom security infrastructure that must be present on a target machine to work properly. Instead, Ansible securely connects to compute hosts over the public network by using SSH keys. To bring a resource to the required state, Ansible pushes modules to the managed host that run the tasks in your Ansible playbook. After the tasks are executed, the result is returned to the Ansible server and the module is removed from the managed host. Ansible modules are idempotent such that executing the same playbook or operation multiple times returns the same result as resources are changed only if required. For more information about Ansible, check out this [video](https://www.youtube.com/watch?v=fHO1X93e4WA){: external}. 

![Configuration Management with Actions and Ansible](/images/new/sc-actions.svg){: caption="Configuration Management with Actions and Ansible" caption-side="bottom"}

Using your supplied playbooks, {{site.data.keyword.bpshort}} runs the Ansible engine to execute your playbook. Ansible, tasks, roles and playbooks can perform provisioning tasks via the {{site.data.keyword.cloud_notm}} APIs via HTTPS, or configuration of compute instances (virtual servers) using SSH. Server configuration is performed via SSH over the public network. To maintain security for your environment, use of a bastion host to provide a secure gateway to your compute infrastructure is strongly encouraged.     

It is your responsibility as a user to ensure suitable network policies are in place for their cloud environment to allow {{site.data.keyword.bpshort}} to connect via SSH to your environment. 
{: note}

Example as-is {{site.data.keyword.cloud}} VPC configurations with bastion hosts are available in the [Cloud-Schematics repo](https://github.com/orgs/Cloud-Schematics/repositories?q=bastion&type=all&language=&sort=){: external}. Follow the tutorial [Discover best-practice VPC configuration for application deployment](https://developer.ibm.com/articles/secure-vpc-access-with-a-bastion-host-and-terraform/) for guidance on creating a suitable network configuration. 

## Using Actions
{: #sc-actions-use}

To use Ansible capabilities in {{site.data.keyword.bpshort}}, you create an action that points to the Ansible playbook that you want to run. 

1. **Add tasks to your playbook**: Use Ansible YAML syntax to describe the configuration tasks that you want to run on your cloud infrastructure, such as installing software or starting, stopping, and rebooting a virtual server. You add these tasks to an Ansible playbook and store the playbook in a GitHub, GitLab, or `Bitbucket` repository to ensure source control and enable collaboration, review, and auditing in your organization. If you are not familiar with Ansible, you can use one of the [{{site.data.keyword.IBM_notm}} provided playbooks](https://github.com/Cloud-Schematics){: external}, or browse the [Ansible Galaxy library](https://galaxy.ansible.com/){: external}.
2. **Create a {{site.data.keyword.bpshort}} action**: When you create an action, you point your action to the repository that stores your Ansible playbook. Then, you select the cloud resources where you want to run the tasks that are defined in your Ansible playbook. To protect your cloud resources, you can further set up a bastion host in front of your target hosts that proxies all Ansible SSH connections to the target hosts. 
3. **Run your action**: When you are ready to configure your cloud resources, you can run your action. {{site.data.keyword.bpshort}} uses the built-in Ansible engine to connect to your target hosts via SSH, and execute the tasks that are defined in your Ansible playbook. You can monitor the progress by reviewing the logs. 


## Next steps
{: #sch-actions-nextsteps}

So far you have learned about {{site.data.keyword.bpshort}} Actions. The following are some next steps to explore.
{: shortdesc}

- See [Creating Actions](/docs/schematics?topic=schematics-create-tf-config) to dig into how to create workspace using the Terraform templates. 


