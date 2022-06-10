---

copyright:
  years: 2017, 2022
lastupdated: "2022-06-10"

keywords: schematics terminology, infrastructure as code, iac, terminology, 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} terminology
{: #learn-schematics-term} 

{{site.data.keyword.bplong}} provides powerful set of Infrastructure as Code (IaC) tools as a service to program your cloud infrastructure provisioning. {{site.data.keyword.bpshort}} can run your end-to-end automation to build one or more stack of cloud resources, manage the lifecycle, manage changes in the configurations, deploy of the app workloads, and perform `day-2` operations.
{: shortdesc}

Learn the basic concepts of the technology behind {{site.data.keyword.bplong_notm}}.

| Objects | Description |
| --- | --- |
| `Action` | Use [{{site.data.keyword.bpshort}} Actions](/docs/schematics?topic=schematics-action-setup) to run your Ansible playbook in {{site.data.keyword.bpshort}}. It acts as a container, for artifacts, used while running your [Ansible playbook](/docs/schematics?topic=schematics-getting-started-ansible) in Schematics. The action points to your Ansible playbook in GitHub or GitLab repository, the inventory to run the playbook, input variables and environment variables are used to run the playbook. When you run the Action, it creates a Schematics Job. The Action also stores the historical data pertaining to the jobs, the job results and the job logs. |
| `Agent`| Use {{site.data.keyword.bpshort}} Agents to run your automation workloads in your Kubernetes cluster, instead of running them on shared environment managed by {{site.data.keyword.bpshort}}. The automation workload running in the [{{site.data.keyword.bpshort}} agents](/docs/schematics?topic=schematics-agents-intro) will enable your Terraform templates or Ansible playbooks to access the data plane of your cloud resources or app resources. It provides the flexibility and control to administer the dedicated infrastructure used to run the IaC automation workload. **Note** the data used by the {{site.data.keyword.bpshort}} Agents is stored and managed at home, by {{site.data.keyword.bpshort}} service. |
| `Blueprint`| Use [{{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-blueprint-intro) to build, manage and operate a complex solution architecture from reusable IaC modules. The blueprint definition assembles multiple modules that are dependent on each other. It acts as a container for multiple workspaces and actions that corresponds to the modules in your solution architecture. It points to your blueprint definition in GitHub or GitLab, cloud configuration settings from GitHub or GitLab, and input variables used to act on the blueprint. When you perform any infrastructure lifecycle management action on you cloud environment, using the blueprint, it creates a job. The blueprint job points to the related child `Workspace Job`, or `Action Jobs` that are run in the same context. In addition, it holds the aggregated job results and the job logs. |
| `Catalog` | A collection of automation templates that can be ordered from {{site.data.keyword.cloud_notm}}. You can onboard your Terraform automation to the {{site.data.keyword.cloud_notm}} catalog, and share the catalog of templates in a controlled manner with your team using IAM permissions and policies. {{site.data.keyword.cloud_notm}} catalog already supports a collection of {{site.data.keyword.IBM_notm}} owned and third-party developed automation in the catalog. The automation is used to provision infrastructure and softwares by using Helm charts, Kubernetes Operators, OVA images, Cloud pak automation. {{site.data.keyword.bpshort}} workspaces are used to run these automation.|
| `Inventory` | A collection of cloud resources that are used as target for running the Ansible playbooks, modules, or roles. Your resource inventory can be defined by using a static inventory file, or dynamically retrieve to your target {{site.data.keyword.cloud_notm}} resources from {{site.data.keyword.bpshort}} workspaces in your {{site.data.keyword.cloud_notm}} account.|
| `Job` | A record of all the {{site.data.keyword.bpshort}} operations. You can see these job records appearing in the context of `Action`, `Blueprint`, and `Workspace`. The job record describes the status of the Job, inputs used to run the job, outputs produced by the job and the console logs.|
| `Template` or `Modules` | Automation code written for provisioning and configuring a cloud infrastructure by using Terraform, Ansible, Helm, Operators, etc., in the IaC language. You can use {{site.data.keyword.bpshort}} to run the automation templates by using workspaces or action. Automation modules are reusable elements that are used to assemble an automation templates. |
| `Workspace` | Use [{{site.data.keyword.bpshort}} workspaces](docs/schematics?topic=schematics-workspace-setup&interface=ui) to run your Terraform automations in {{site.data.keyword.bpshort}}. It acts as a container, for artifacts, used while running your [Terraform templates](/docs/schematics?topic=schematics-create-tf-config) in {{site.data.keyword.bpshort}}. The [workspace](/docs/schematics?topic=schematics-workspace-setup&interface=ui#create-workspace_ui) points to your Terraform templates that are in your GitHub or GitLab repository, input variables, and environment variables that are used to run your terraform commands. Then used to store the intermediate files generated by Terraform and related tools, such as `state-file`, `plan-json-file`, `resource-json-file`, `cost-estimation`, etc. When you run Terraform commands such as `plan`, `apply`, `destroy`, `refresh`, etc., by using your workspace, it creates a {{site.data.keyword.bpshort}} Job. The workspace also stores the historical data pertaining to the jobs, the job results, and the job logs. |
{: caption="{{site.data.keyword.bplong_notm}} terminologies" caption-side="bottom"}
