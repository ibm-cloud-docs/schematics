---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-28"

keywords: schematics terminologies, infrastructure as code, iac, terminologies, terminology 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} terminologies
{: #learn-schematics-term} 

Building on open-source Ansible, Terraform and related technologies, {{site.data.keyword.bplong}} provides a powerful set of Infrastructure as Code (IaC) tools as a service to program your cloud infrastructure provisioning. {{site.data.keyword.bpshort}} can run your end-to-end automation to build one or more stacks of cloud resources, manage their lifecycle, manage changes in the configurations, deploy the app workloads, and perform day-2 operations.
{: shortdesc}

Learn the basic concepts of the technology behind {{site.data.keyword.bplong_notm}}.

| Objects | Description |
| --- | --- |
| `Actions` | Use [{{site.data.keyword.bpshort}} Actions](/docs/schematics?topic=schematics-action-setup) to run your Ansible playbook in {{site.data.keyword.bpshort}}. It acts as a container, for artifacts, used while running your [Ansible playbook](/docs/schematics?topic=schematics-getting-started-ansible) in Schematics. </br> The action points to your Ansible playbook in GitHub or GitLab repository, the inventory to run the playbook, input variables and environment variables are used to run the playbook. When you run the action, it creates a {{site.data.keyword.bpshort}} Job. The action also stores the historical data about the jobs, the job results and the job logs. |
| `Agents`| Use {{site.data.keyword.bpshort}} Agents to run your automation workloads in your Kubernetes cluster, instead of running them on shared environment managed by {{site.data.keyword.bpshort}}. The automation workload running in the [{{site.data.keyword.bpshort}} Agents](/docs/schematics?topic=schematics-agents-intro) enables your Terraform templates or Ansible playbooks to access the data plane of your cloud resources or app resources.</br> It provides the flexibility and control to administer the dedicated infrastructure used to run the IaC automation workload. The data used by the {{site.data.keyword.bpshort}} Agents is stored and managed at home, by {{site.data.keyword.bpshort}} service. |
| `blueprints`| Use [{{site.data.keyword.bpshort}} Blueprints](/docs/schematics?topic=schematics-blueprint-intro) to build, manage and operate a complex solution architecture from reusable IaC modules. The blueprint template assembles multiple modules that are dependent on each other. It acts as a container for multiple workspaces and actions that corresponds to the modules in your solution architecture. </br> It points to your blueprints template in GitHub or GitLab, cloud configuration settings from GitHub or GitLab, and input variables used to act on the blueprint. When you perform any infrastructure lifecycle management action on you cloud environment by using the blueprint, it creates a job. The blueprints job points to the related child `workspace Job`, or `action Jobs` that are run in the same context. In addition, it holds the aggregated job results and the job logs. |
| `Catalog` | A collection of automation templates that can be ordered from {{site.data.keyword.cloud_notm}}. You can onboard your Terraform automation to the {{site.data.keyword.cloud_notm}} catalog, and share the catalog of templates in a controlled manner with your team by using IAM permissions and policies. </br>{{site.data.keyword.cloud_notm}} catalog already supports a collection of {{site.data.keyword.IBM_notm}} owned and Third party developed automation in the catalog. The automation is used to provision infrastructure and softwares by using Helm charts, Kubernetes Operators, OVA images, `Cloudpak` automation. {{site.data.keyword.bpshort}} Workspaces are used to run these automation.|
| `Inventory` | A collection of cloud resources that are used as target for running the Ansible playbooks, modules, or roles. </br> Your resource inventory can be defined by using a static inventory file, or dynamically retrieve to your target {{site.data.keyword.cloud_notm}} resources from {{site.data.keyword.bpshort}} Workspaces in your {{site.data.keyword.cloud_notm}} account.|
| `Jobs` | A record of all the {{site.data.keyword.bpshort}} operations. You can see these job records appearing in the context of `action`, `Blueprint`, and `workspace`. </br>The job record describes the status of the Job, inputs used to run the job, outputs produced by the job and the console logs.|
| `Template` or `Modules` | Automation code written for provisioning and configuring a cloud infrastructure by using Terraform, Ansible, Helm, Operators, and so on., in the IaC language. </br> You can use {{site.data.keyword.bpshort}} to run the automation templates by using workspaces or action. Automation modules are reusable elements that are used to assemble an automation templates. |
| `Workspaces` | Use [{{site.data.keyword.bpshort}} Workspaces](/docs/schematics?topic=schematics-workspace-setup) to run your Terraform automations in {{site.data.keyword.bpshort}}. It acts as a container, for artifacts, used while running your [Terraform templates](/docs/schematics?topic=schematics-create-tf-config) in {{site.data.keyword.bpshort}}. </br>The [workspace](/docs/schematics?topic=schematics-workspace-setup#create-workspace_ui) points to your Terraform templates that are in your GitHub or GitLab repository, input variables, and environment variables that are used to run your terraform commands. Then used to store the intermediate files generated by Terraform and related tools, such as `state-file`, `plan-json-file`, `resource-json-file`, `cost-estimation`, and so on. When you run Terraform commands such as `plan`, `apply`, `destroy`, `refresh`, and so on., by using your workspace, it creates a {{site.data.keyword.bpshort}} Job. The workspace also stores the historical data about the jobs, the job results, and the job logs. |
{: caption="{{site.data.keyword.bplong_notm}} terminologies" caption-side="bottom"}



## Next steps
{: #terminologies-nextsteps}

- Do you want to know how these objects works in {{site.data.keyword.bpshort}}? Then, you need to explore these [use cases](/docs/schematics?topic=schematics-how-it-works).
- Click [here](/docs/schematics?topic=schematics-learn-schematics-term) to revisit the {{site.data.keyword.bpshort}} terminologies.
