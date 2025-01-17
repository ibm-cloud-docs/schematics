---

copyright: 
  years: 2017, 2021
lastupdated: "2025-01-17"

keywords: tools and utilities, utilities, tools, runtime tools, schematics tools, schematics utilities

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# {{site.data.keyword.bpshort}} worker runtime
{: #sch-utilities}

The {{site.data.keyword.bpshort}} worker runs your automation workload. The workload is deployed as a Kubernetes job pod that run only one {{site.data.keyword.bpshort}} automation command and self-destructs on completion. All your {{site.data.keyword.bpshort}} command such as `schematics plan`, `schematics apply`, `schematics run`, and `schematics destroy` runs by using the {{site.data.keyword.bpshort}} worker pods. In addition to the core Terraform CLI and Ansible CLI, the {{site.data.keyword.bpshort}} worker job images include several utilities and executables.

Software can not be installed in the {{site.data.keyword.bpshort}} runtime. Any attempt to install a software in the `schematics-runtime-job` pod, is considered a violation and can cause vulnerability.

Following are the {{site.data.keyword.bpshort}} type of workers.

| Worker | Description |
| --- | --- |
| runtime-terraform-job | Used to run the Terraform CLI. </br> This worker is used to run the {{site.data.keyword.bpshort}} workspace jobs. |
| runtime-ansible-job | Used to run the Ansible playbook CLI. </br> This worker is used to run the {{site.data.keyword.bpshort}} Action jobs. |
{: caption="{{site.data.keyword.bpshort}} workers types" caption-side="top"}

The {{site.data.keyword.bpshort}} worker images are built by using the [Universal Base Image (UBI-8)](https://catalog.redhat.com/software/containers/ubi8/ubi/5c359854d70cc534b3a3784e){: external} as the base image. The {{site.data.keyword.bpshort}} worker images used by the multi-tenant {{site.data.keyword.bpshort}} service instance and the corresponding {{site.data.keyword.bpshort}} agents are not the same.

In the multi-tenant {{site.data.keyword.bpshort}} service instance, your Terraform or Ansible automation cannot install any software or tools in the {{site.data.keyword.bpshort}} worker pods. It is blocked by design.
{: note}

For more information about deprecation note, see [{{site.data.keyword.bpshort}} announcement](https://cloud.ibm.com/status/announcement?component=schematics){: external}.
{: note}

| From date | Deprecation notice |
| --- | --- |
| 21 September 2022 | Support for `Python v3.6` in runtime-ansible-job is deprecated. Instead the runtime-ansible job image uses `Python v3.8`. |
{: caption="{{site.data.keyword.bpshort}} deprecation note" caption-side="top"}

The following table enlists the preinstalled software and tools in the {{site.data.keyword.bpshort}} worker images.

## Runtime-terraform-job image
{: #sch-runtime-tf-job}

| Software | Versions | In agent | Description |
| --- | --- | ---| --- |
|`Terraform CLI` |	`1.0.x`, `1.1.x`,</br>`1.2.x`, `1.3.x`, `1.4.x` | `1.5.x` | Terraform CLI |
| `Ansible` |  `v2.9.27`	| `v2.9.27`	| For use by the [ansible-provisioner](https://github.com/radekg/terraform-provisioner-ansible){: external} for Terraform. </br>It is recommended to use the {{site.data.keyword.bpshort}} actions to run your Ansible automation.|
| `{{site.data.keyword.cloud_notm}} CLI` |	Latest	 | Latest	| Latest version of the {{site.data.keyword.cloud_notm}} CLI plug-ins are pre-installed. For your Terraform automation, it is recommended to use the Terraform provider plugi-ns for {{site.data.keyword.cloud_notm}}. |
| `JQ` |	`v1.6` |	Yes	| As the [JSON processor](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_jq) in your Terraform automation. |
| `Kubectl client` | | Yes |For use in your Terraform automation. It is recommended to use the Terraform provider plug-ins for Kubernetes. |
| `OpenShift client` | | Yes | {{site.data.keyword.redhat_openshift_notm}} CLI for your Terraform automation. It is recommended to use the Terraform provider plug-ins for {{site.data.keyword.cloud_notm}} and Kubernetes. |
| `Python` |	`v3.6` |	No	| For use in your Terraform automation. |
| `Python libraries` |	`netaddr`	| No	| Red Hat OpenShift CLI for your Terraform automation. It is recommended to use the Terraform provider plug-ins for {{site.data.keyword.cloud_notm}} and Kubernetes. |
{: caption="{{site.data.keyword.bpshort}} runtime terraform job image" caption-side="top"}

## Runtime-ansible-job image
{: #sch-runtime-ansible-job}

The {{site.data.keyword.bpshort}} actions use the `Ansible v2.9.27` in the `Ansible-runtime-job` image. The current minor version of the Ansible CLI is used in the {{site.data.keyword.bpshort}}.

| Software | Versions | In agent | Description |
| --- | --- | ---| --- |
| `Ansible` |  `v2.9.27`	| `v2.9.27`	| Ansible CLI |
| `{{site.data.keyword.cloud_notm}} CLI` |  Latest	| Latest	| Latest version of the {{site.data.keyword.cloud_notm}} CLI plug-ins are pre-installed, to use in your Ansible automation.|
| `JQ` |	`v1.6` |	`v1.6`	| As the [JSON processor](/docs/solution-tutorials?topic=solution-tutorials-tutorials#getting-started-macos_jq) in your Ansible automation. |
| `Kubectl client` | Latest | Yes |	For use in your Ansible automation.|
| `OpenShift client` | Latest | Yes | {{site.data.keyword.redhat_openshift_notm}} CLI for your Ansible automation.|
| `Python` |	`v3.8` |	`v3.8`	| For use in your Ansible automation. |
| `Python libraries` |	`netaddr`, </br>`kubernetes`, </br>`openshift`, </br>`pywinrm`, </br>`boto3`, </br>`boto`, </br>`botocore`, </br>`PyVmomi`	| No	| For use in your Ansible automation. |
{: caption="{{site.data.keyword.bpshort}} runtime ansible job image" caption-side="top"}
