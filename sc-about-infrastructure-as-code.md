---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-29"

keywords: iac, infrastructure, infrastructure as code, terraform, ansible

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# What is Infrastructure as Code?
{: #infrastructure-as-code}

In simple terms, Infrastructure as Code (IaC) is using code to manage and provision infrastructure in a descriptive model instead of manual processes.
{: shortdesc}

With IaC, configuration files define your infrastructure, which also makes it easier to edit, share, and reuse configurations. By codifying your infrastructure, you provision the same environment every time avoiding undocumented, ad-hoc configuration changes.

{{site.data.keyword.bpshort}} utilizes open-source Ansible and Terraform to provide a powerful set of IaC tools as a service to program your cloud infrastructure. With {{site.data.keyword.bpshort}} you can use this rich set of IaC automation capabilities to build stacks of cloud resources, manage their lifecycle, manage changes in their configurations, deploy your app workloads, and perform day-2 operations.

## Benefits of Infrastructure as Code
{: #iac-benefits}

Adopting an IaC approach to infrastructure deployment solves many common problems with provisioning infrastructure and delivers several benefits. {{site.data.keyword.bpshort}} enables you to realize these benefits without the need to install, run, and manage your own IaC tooling.
{: shortdesc}

- **Reliability and Consistency**: New environments or infrastructure are provisioned reliably. Manual processes result in mistakes. With IaC the same configurations are deployed over and over, repeatedly without differences. IaC improves consistency across environments and deployments.

- **Speed**: IaC enables you to quickly set up your complete infrastructure through automation. You apply it to every environment, from development to production, staging, QA, and more. This can lead to lower costs as the time to deploy, manage, and maintain environments decreases.

- **Tracking and accountability**: Changes to existing infrastructure are made in code, and the changes are tracked. Like any source code file, you have full traceability of the changes made to a configuration.

- **Detect and correct environment drift**: If a part of the infrastructure is modified manually outside of the code, it can be brought back in line with the desired state on the next run. [Drift detection](/docs/schematics?topic=schematics-drift-note) is a feature of {{site.data.keyword.bpshort}} Workspaces. 

## Best Practices 
{: #iac-best-practices}

When adopting IaC for provisioning and configuration management, there are number of recommended practices. These practices are fully supported when using {{site.data.keyword.bpshort}}.
{: shortdesc}

### Codifying everything in IaC
{: #iac-bp-codify}

All the infrastructure specifications should be explicitly coded in a configuration file, for instance as Terraform configurations or Ansible playbooks. The configuration files are the single source of truth of your infrastructure specification and describe what infrastructure components will be used and their configuration?
{: shortdesc}

### Minimize documentation
{: #iac-bp-docs}

IaC is the documentation. With IaC in place, the configuration files represent the documentation and are always up-to-date, which reduces effort. The remaining documentation is about the process.
Maintain code in a Version Control System.
{: shortdesc}

IaC configuration files should be kept in a version control systems (VCS), like GitHub or GitLab. This provides an audit trail for code changes, but it also gives the opportunity to collaborate or peer review and test changes before they go live.

With this practice, you can easily track, manage, and revert any potential changes to your systems, with enhanced traceability and visibility.

### Testing
{: #iac-bp-testings} 

One of the practices that IaC borrows from software development is testing. Rigorous testing of infrastructure configuration plays a role in reducing post deployment issues. When combined with version control systems, testing can be automatically triggered every time there is a modification in the code.
{: shortdesc}

With Continuous Integration (CI) in place, the templated infrastructure configuration can be implemented in multiple environments such as the `development`, `UAT`, `QA`, or `production` environment with minimal changes applied effectively.

### Modular Infrastructure
{: #iac-bp-modularity}

Breaking down infrastructure into [modules](https://github.com/terraform-ibm-modules/documentation){: external} allows for reuse, improved reliability and an easier adoption path. Similar to the use of modules and packages in programming languages. Following are the benefits to this practice.
{: shortdesc}

- Frequently used configurations can be codified as modules and reused multiple times across environments.
- Reliability increases as modules can be tested and become hardened over time with use.
- Composition from reusable modules lowers the skills barrier to IaC adoption.
- Changes are easier to make and test at a module level.
- The risk of change reduces as configuration changes are localized.

## Next steps
{: #iac-nextsteps}

Now you understand more about IaC, why not review the use of IaC in {{site.data.keyword.bpshort}}: 
- Learn more about the [Open-source tools in {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-schematics-open-projects)
- Explore these [use cases](/docs/schematics?topic=schematics-how-it-works).
