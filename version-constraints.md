---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-25"

keywords: schematics utilities, commands and utilities, utilities, jobs

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Specifying version constraints 
{: #version-constraints}

Use version constraints to declare the Terraform, Terraform provider, or Ansible version that you want to use for your Terraform template, Ansible playbook or blueprint templates.
{: shortdesc}

{{site.data.keyword.bpshort}} concurrently supports multiple images that each package a specific version of the {{site.data.keyword.cloud_notm}} Provider plug-in, other Terraform providers, such as the REST API provider, and the Ansible executable. Each image is built, tested, and verified by {{site.data.keyword.cloud_notm}}. Included Terraform provider versions are tested and packaged for a specific Terraform CLI version.

It is good practice to declare the version that your Terraform template, Ansible playbook or blueprint template requires by using version constraints. This way, you can ensure that your templates and playbooks continue to work, even if Terraform, Terraform providers, or Ansible publish new versions that might introduce breaking changes.

## Overview of {{site.data.keyword.bpshort}} images and packaged Terraform providers
{: #schematics-image-ov}

Use the `ibmcloud schematics version` command to retrieve a list of {{site.data.keyword.bpshort}} images and the Terraform provider and Ansible executable versions that are packaged in each image. For example in the following CLI output, the {{site.data.keyword.cloud_notm}} Provider plug-in latest version is tested on `Terraform v1.1`.
{: shortdesc}

{{site.data.keyword.bpshort}} supports the 5 most recent versions of  **{{site.data.keyword.terraform-provider_full_notm}}** binaries in its image. For more information, see [latest releases](https://github.com/IBM-Cloud/terraform-provider-ibm/releases){: external}. Following are some constraints that you must follow when using the {{site.data.keyword.cloud_notm}} provider in your Terraform template. 

It is recommended to use Terraform v1.0 or higher.
{: note}

* **If you are using Terraform v0.13 or higher**, you can arbitrarily choose any version of the {{site.data.keyword.cloud_notm}} provider, in your template. Then, {{site.data.keyword.bpshort}} automatically download the {{site.data.keyword.cloud_notm}} provider either locally from the cache or remotely from the [HashiCorp Configuration Language (HCL) Terraform Registry](https://registry.terraform.io/namespaces/IBM-Cloud).

To use any of the predefined {{site.data.keyword.bpshort}} images, you must explicitly declare the version of the {{site.data.keyword.cloud_notm}} Provider plug-in in your Terraform template that includes the provider versions that you want. For more information, see [Specifying version constraints for the Terraform CLI and Terraform providers](/docs/schematics?topic=schematics-version-constraints#version-constraints-terraform).
{: important}

You cannot change the default version for the Ansible executable. You can only [specify the version of referenced Ansible roles and collections](/docs/schematics?topic=schematics-version-constraints#version-constraints-terraform).
{: note}

```sh
ibmcloud schematics version
```
{: pre}

```text

Template Type   Version
Terraform       terraform_v0.12
Additional terraform Providers   Version
Ansible                          v2.9.23
Ansible Provisioner              v2.3.3
Provider for REST API            v1.10.0
IBM Cloud Provider               v1.38.2
Open shift client                v3.11.0

Template Type   Version
Terraform       terraform_v0.13
Additional terraform Providers   Version
IBM Cloud Provider               v1.38.2
Provider for REST API            v1.10.0
Open shift client                v3.11.0
Ansible                          v2.9.23
Ansible Provisioner              v2.3.3

Template Type   Version
Terraform       terraform_v0.14
Additional terraform Providers   Version
Ansible                          v2.9.23
Ansible Provisioner              v2.3.3
IBM Cloud Provider               v1.38.2
Open shift client                v3.11.0
Provider for REST API            v1.10.0

Template Type   Version
Terraform       terraform_v0.15
Additional terraform Providers   Version
Ansible                          v2.9.23
Ansible Provisioner              v2.3.3
IBM Cloud Provider               v1.38.2
Open shift client                v3.11.0
Provider for REST API            v1.10.0

Template Type   Version
Terraform       terraform_v1.0
Additional terraform Providers   Version
Open shift client                v3.11.0
Provider for REST API            v1.10.0
IBM Cloud Provider               v1.38.2
Ansible                          v2.9.23
Ansible Provisioner              v2.3.3

Template Type   Version
Terraform       terraform_v1.1
Additional terraform Providers   Version
Ansible                          v2.9.23
Open shift client                v3.11.0
Provider for REST API            v1.10.0
Ansible Provisioner              v2.3.3
IBM Cloud Provider               v1.38.2

```
{: screen}


## Specifying version constraints for the Terraform CLI and Terraform providers
{: #version-constraints-terraform}

You can choose to specify the Terraform CLI version and the version of any of the providers that you want to use by using Terraform version constraints. For more information about how to specify version constraints, see the [Terraform documentation](https://developer.hashicorp.com/terraform/language/expressions/version-constraints){: external}. 
{: shortdesc}

### Version constraints for the Terraform CLI
{: #tf-version-constraint}

When you create a {{site.data.keyword.bpshort}} Workspaces and choose a Terraform version such as `v0.13`, your Terraform templates are executed by using the default patch version that is set in {{site.data.keyword.bpshort}}. For example, if you choose `terraform_v0.13`, your templates are applied by using Terraform v0.13.4. You can use the `required_providers` block in your `provider` definition to force the Terraform engine in {{site.data.keyword.bpshort}} to pull a later version. 
{: shortdesc}

You can only specify versions that are higher than the default `MAJOR.MINOR.PATH` version that is set in {{site.data.keyword.bpshort}}. In the codeblock `version = "x.x.x"` signifies the {{site.data.keyword.cloud_notm}} provider version. 

```terraform
terraform {
    required_providers {
        version = "1.39.1"
    }
}
```
{: codeblock}

You can specify Terraform `required_versions` that are higher than the default `MAJOR.MINOR.PATH` in the Terraform configuration file. In the codeblock the `required_version = ">=1.0.0, <2.0"` signifies the Terraform version.


```terraform
terraform {
required_version = ">=1.0.0, <2.0"
  required_providers {
    ibm = {
      source = "IBM-Cloud/ibm"
    }
  }
}
```
{: codeblock}

### Version constraints for Terraform providers
{: #provider-version-contraint}

To use any of the predefined [{{site.data.keyword.bpshort}} images](#schematics-image-ov), you must explicitly declare the version of the {{site.data.keyword.cloud_notm}} Provider plug-in in your Terraform template that includes the provider versions that you want. 

If {{site.data.keyword.cloud_notm}} Provider plug-in version is not declared in your Terraform template, the latest version of the provider plug-in is automatically used in {{site.data.keyword.bpshort}}. 
{: note}

**Example to specify a predefined {{site.data.keyword.bpshort}} image**: </br>

The following example shows how to use the {{site.data.keyword.bpshort}} image that was built for the {{site.data.keyword.cloud_notm}} Provider plug-in v1.39.1. This image includes specific versions for other providers, such as the REST API provider. 

```terraform
terraform {
    required_providers {
        ibm = {
        source = "IBM-Cloud/ibm"
        version = "v1.39.1"
    }
    }
```
{: codeblock}

**Example to use specific Terraform provider versions**: </br>

To use a different {{site.data.keyword.cloud_notm}} Provider plug-in version, or to pin your Terraform configuration file to a specific version of another external provider, such as AWS, Helm or Kubernetes, use the following syntax. 

```terraform
terraform {
    required_providers {
        ibm = {
        source = "IBM-Cloud/ibm"
        version = "~> 1.38.1"
    }
    aws = {
        version = ">= 2.7.0"
        source = "hashicorp/aws"
    }
    }
```
{: codeblock}

## Specifying version constraints in Ansible
{: #version-constraints-ansible}

{{site.data.keyword.bpshort}} currently supports the latest Ansible version v2.9.23 only. When you create a {{site.data.keyword.bpshort}} action, you must ensure that your Ansible playbooks can be run with this version. You cannot specify a specific Ansible version for your playbook. 

However, if you use existing Ansible roles or collections in your playbook, you can specify the version of the role or collection that you want to run by using a `requirements.yml` file. For more information about how to reference roles and collections in your playbook, see [Referencing Ansible roles in your playbook](/docs/schematics?topic=schematics-ansible-roles-galaxy) and [Referencing Ansible collections in your playbook](/docs/schematics?topic=schematics-create-playbook#schematics-collections). To learn more about how to specify versions for roles and collections, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#install-multiple-collections-with-a-requirements-file){: external}.

```yaml
roles:
  - name: andrewrothstein.kubectl
    version: 1.1.50
```
{: codeblock}



