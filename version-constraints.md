
  
  
# Specifying version constraints in {{site.data.keyword.bpshort}}
{: #version-constraints}

Although {{site.data.keyword.bpshort}}, new versions of built-in external providers, such as the Helm provider or Ansible provisioner, might be added that can introduce breaking changes. 

If no versions are specified, the lateset version is pulled by default. To avoid breaking changes from what the provider created, use version constraints Because external providers might

## Specifying version constraints for Terraform and other Terraform providers
{: #version-constraints-terraform}

{{site.data.keyword.bpshort}} concurrently supports multiple images that each package a specific version of the {{site.data.keyword.cloud_notm}} Provider plug-in and other providers, such as Helm or Kubernetes. Each image is built, tested, and verified by {{site.data.keyword.cloud_notm}} based on a specific Terraform CLI version. For an overview of supported {{site.data.keyword.bpshort}} images and packaged providers, see [Overview of {{site.data.keyword.bpshort}} images and packaged providers](#schematics-image-ov). 

You can choose to specify the Terraform CLI version and the version of any of the providers that you want to use by using Terraform version constraints. For more information about how to specify version contraints, see the [Terraform documentation](https://www.terraform.io/docs/language/expressions/version-constraints.html). 

### Overview of {{site.data.keyword.bpshort}} images and packaged providers
{: #schematics-image-ov}

Use the `ibmcloud schematics version` command to retrieve a list of {{site.data.keyword.bpshort}} images and the provider versions that are packaged in each image. For example, in the following CLI output the {{site.data.keyword.cloud_notm}} Provider plug-in version v1.23.1 is packaged with the Helm provider v0.10.4a and Kubernetes provider v1.10.0a, and was tested on Terraform v0.12.

To use any of the pre-defined {{site.data.keyword.bpshort}} images, you must explicitely declare the version of the {{site.data.keyword.cloud_notm}} Provider plug-in in your Terraform template that includes the provider versions that you want. For more information, see [Specifying version constraints for Terraform and other Terraform providers](#version-constraints-terraform). 
{: important}

```
ibmcloud schematics version
```
{: pre}

Example output: 
```
Template Type   version   
Terraform       terraform_v0.11   
Additional terraform Providers   version   
Open shift client                v3.11.0   
IBM Cloud Provider               v0.31.0   
Kubernetes Provider              v1.10.0a   
Provider for REST API            v1.10.0   
Ansible                          v2.9.7   
Ansible Provisioner              v2.3.3   
Helm Provider                    v0.10.4a   
                                    
Template Type   version   
Terraform       terraform_v0.12   
Additional terraform Providers   version   
Ansible                          v2.9.7   
Ansible Provisioner              v2.3.3   
IBM Cloud Provider               v1.23.1   
Kubernetes Provider              v1.10.0a   
Open shift client                v3.11.0   
Helm Provider                    v0.10.4a   
Provider for REST API            v1.10.0   
                                    
Template Type   version   
Terraform       terraform_v0.13   
Additional terraform Providers   version   
Helm Provider                    v0.10.4a   
IBM Cloud Provider               v1.23.1   
Kubernetes Provider              v1.10.0a   
Open shift client                v3.11.0   
Provider for REST API            v1.10.0   
Ansible                          v2.9.7   
Ansible Provisioner              v2.3.3   
                                    
Template Type   version   
Terraform       terraform_v0.14   
Additional terraform Providers   version   
Ansible Provisioner              v2.3.3   
Helm Provider                    v0.10.4a   
Provider for REST API            v1.10.0   
Ansible                          v2.9.7   
IBM Cloud Provider               v1.23.1   
Kubernetes Provider              v1.10.0a   
Open shift client                v3.11.0
```
{: screen}

### Version constraints for the Terraform CLI
{: #tf-version-constraint}

When you create a {{site.data.keyword.bpshort}} workspace and choose a Terraform version such v0.13, your Terraform templates are applied by using the default patch version that is set in {{site.data.keyword.bpshort}}. For example, if you choose version v0.13, your templates are applied by using Terraform v0.13.4. You can use the `required_version` block in your `provider` definition to force the Terraform engine in {{site.data.keyword.bpshort}} to pull a later version. 

You can only specify a versions that are higher than the default `MAJOR.MINOR.PATH` version that is set in {{site.data.keyword.bpshort}}. 
{: note}

```
terraform {
  required_providers {
      version = ">= 0.13.4"
    }
}
```
{: codeblock}

### Version constraints for Terraform providers
{: #provider-version-contraint}

To use any of the pre-defined [{{site.data.keyword.bpshort}} images](#schematics-image-ov), you must explicitely declare the version of the {{site.data.keyword.cloud_notm}} Provider plug-in in your Terraform template that includes the provider versions that you want. If no {{site.data.keyword.cloud_notm}} Provider plug-in version is declared in your Terraform template, the latest version is automatically used in {{site.data.keyword.bpshort}}. 

The following example shows how to use the {{site.data.keyword.bpshort}} image that was built for the {{site.data.keyword.cloud_notm}} Provider plug-in v1.23.1. This image includes specific versions for other providers, such as Helm and Kubernetes. 

```
terraform {
  required_providers {
    ibm = {
      source = "IBM-Cloud/ibm"
      version = "v1.23.1"
    }
  }
```
{: codeblock}

To use a different {{site.data.keyword.cloud_notm}} Provider plug-in version, or to pin your Terraform configuration file to a specific version of another external provider, such as AWS, Helm or Kubernetes, use the following syntax. 

```
terraform {
  required_providers {
    ibm = {
      source = "IBM-Cloud/ibm"
      version = "~> 1.12.0"
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

{{site.data.keyword.bpshort}} currently supports the latest Ansible version v2.9.7. When you create a {{site.data.keyword.bpshort}} action, you must ensure that your Ansible playbooks can be run with this version. You cannot specify a specific Ansible version for your playbook. 

However, if you use existing Ansible roles or collections in your playbook, you can specify the version of the role or collection that you want to run by using a `requirements.yml` file. For more information about how to reference roles and collections in your playbook, see [Referencing Ansible roles in your playbook](/docs/schematics?topic=schematics-create-playbooks#schematics-roles) and [Referencing Ansible collections in your playbook
](/docs/schematics?topic=schematics-create-playbooks#schematics-collections). To learn more about how to specify versions for roles and collections, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#install-multiple-collections-with-a-requirements-file){: external}

```
roles:
  - name: andrewrothstein.kubectl
    version: 1.1.50
```
{: codeblock}

