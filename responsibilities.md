---

copyright:
  years: 2017, 2021
lastupdated: "2021-05-20"

keywords: schematics reponsibilities, schematics high availability, schematics backup, schematics disaster recovery, schematics security, schematics ibm vs user
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
{:help: data-hd-content-type='help'}
{:support: data-reuse='support'}

# User responsibilities by using {{site.data.keyword.bplong_notm}}
{: #responsibilities}
{: help}
{: support}

Learn about the responsibilities that you have when you use {{site.data.keyword.bplong_notm}}. 
{: shortdesc}

<table>
<thead>
<th>Category</th>
<th>Responsibilities</th>
</thead>
<tbody>
<tr>
<td>Resource provisioning and management</td>
<td><strong>IBM responsibilities: </strong>
<ul>
<li>Provide a suite of tools to automate resource provisioning and resource management in {{site.data.keyword.cloud_notm}}, such as the {{site.data.keyword.bplong_notm}} console, CLI, and API. </li>
<li>Provide and maintain sample Terraform templates.</li>
  <li>Create, update, and manage the Terraform statefile (`terraform.tfstate`) to determine the required actions to achieve the required state that is described in your Terraform template. </li>
</ul></br><strong>User responsibility</strong>
<ul><li>Create your own Terraform templates or use one of the provided sample templates to configure the resources that you want. </li>
<li>Design the source repository structure, and set up and manage the source repository for your Terraform template.</li>
<li>Understand the {{site.data.keyword.cloud}} resource offerings that are used in your templates, including availability, pricing, limitations and the tools to set up security, high availability, and logging and monitoring.</li>
<li>Design and provision your resources in a way that achieves high availability and resiliency, such as [multizone clusters](/docs/containers?topic=containers-ha_clusters#multizone) or [multi-region VPCs](/docs/vpc?topic=solution-tutorials-vpc-multi-region#vpc-multi-region). </li>
<li>Use the provided tools to [provision](/docs/schematics?topic=schematics-manage-lifecycle#deploy-resources), [modify](/docs/schematics?topic=schematics-manage-lifecycle#update-resources), or [delete](/docs/schematics?topic=schematics-manage-lifecycle#destroy-resources) your {{site.data.keyword.cloud_notm}} resources.</li>
  <li>Set up logging and monitoring for your resources to track resource activity and monitor the resource health, such as with [{{site.data.keyword.la_full_notm}}](/docs/log-analysis?topic=log-analysis-getting-started) and [{{site.data.keyword.mon_full_notm}}](/docs/monitoring?topic=monitoring-getting-started). </li></ul></td>
</tr>
<tr>
<td>Security</td>
<td><strong>IBM responsibilities: </strong>
<ul>
  <li>Provide the tools to lock a workspace and disable changes for a workspace. </li>
<li>Automatically apply security and version updates to sample templates and the templates in the {{site.data.keyword.cloud_notm}} catalog.</li>
<li>Update operational {{site.data.keyword.bplong_notm}} components, such as the {{site.data.keyword.cloud_notm}} Provider plug-in for Terraform, the Terraform command-line version, and other supported providers and provisioning engines.</li>
<li>Provide access to {{site.data.keyword.bpshort}} log information for resource-related actions.</li>
<li>Track workspace activities and automatically send events to {{site.data.keyword.at_full_notm}}. </li>
<li>Encrypt workspace data in transit and at rest. </li>
</ul></br><strong>User responsibility: </strong>
<ul>
<li>Use Identity and Access Management (IAM) to control access to a {{site.data.keyword.bpshort}} workspace and related {{site.data.keyword.cloud_notm}} resources.</li>
<li>Secure the source repository for your Terraform template, including access control, security settings, collaboration, and version control. </li>
<li>Secure the {{site.data.keyword.cloud_notm}} resources that you create by using the security features that are provided by the resource offering. </li>
<li>Use the provided tools of your {{site.data.keyword.cloud_notm}} resources to apply security patches, access controls, and encryption to your resources. </li>
</ul></td></tr>
<tr>
  <td>High availability, backup, and disaster recovery</td>
  <td><strong>IBM responsibilities: </strong>
<ul>
<li>Back up workspace data across {{site.data.keyword.cloud_notm}} regions.  </li>
  <li>Recover workspace data from the replicated region within 24 hours. </li>
  <li>Recover operational {{site.data.keyword.bplong_notm}} components.</li>
</ul></br><strong>User responsibility: </strong>
<ul>
<li>Design and provision your resources in a way that achieves high availability and resiliency, such as [multizone clusters](/docs/containers?topic=containers-ha_clusters#multizone) or [multi-region VPCs](/docs/vpc?topic=solution-tutorials-vpc-multi-region#vpc-multi-region). </li>
  <li>Set up a backup and recovery strategy for your resources and your workload data. </li></ul>
</tbody>
</table>

