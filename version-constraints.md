---

copyright:
  years: 2017, 2021
lastupdated: "2021-05-28"

keywords: schematics utilities, commands and utilities, utilities, jobs

subcollection: schematics

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}

  
  
# Specifying version constraints in {{site.data.keyword.bpshort}}
{: #version-constraints}

Use version constraints to declare the Terraform, Terraform provider, or Ansible version that you want to use for your Terraform template or Ansible playbook. 
{: shortdesc}

{{site.data.keyword.bpshort}} concurrently supports multiple images that each package a specific version of the {{site.data.keyword.cloud_notm}} Provider plug-in, other Terraform providers, such as the REST API provider, and the Ansible executable. Each image is built, tested, and verified by {{site.data.keyword.cloud_notm}}. Included Terraform provider versions are tested and packaged for a specific Terraform CLI version. 

It is good practice to declare the version that your Terraform template or Ansible playbook requires by using version constraints. This way, you can ensure that your templates and playbooks continue to work, even if Terraform, Terraform providers, or Ansible publish new versions that might introduce breaking changes. 

## Overview of {{site.data.keyword.bpshort}} images and packaged Terraform providers
{: #schematics-image-ov}

Use the `ibmcloud schematics version` command to retrieve a list of {{site.data.keyword.bpshort}} images and the Terraform provider and Ansible executable versions that are packaged in each image. For example in the following CLI output, the {{site.data.keyword.cloud_notm}} Provider plug-in version v1.23.1 is packaged with the REST API provider version v1.10.0, and was tested on Terraform v0.12.
{: shortdesc}

{{site.data.keyword.bpshort}} supports the 5 most recent versions of  **IBM Cloud provider plugin for Terraform** binaries in its image.  Following are some of the constraints that you must follow when using the {{site.data.keyword.cloud_notm}} provider in your Terraform template. 

It is recommended to use Terraform v0.13 or higher.
{: note}

* **If you are using Terraform v0.12**, you must see that your Terraform template is only using one of these 5 {{site.data.keyword.cloud_notm}} provider versions. If you specify an older version of the {{site.data.keyword.cloud_notm}} provider in the template, you see the following error `Provider ibm not available for installation.` in the logs.
		
* **If you are using Terraform v0.13 or higher**, you can arbitrarily choose any version of the {{site.data.keyword.cloud_notm}} provider, in your template. Then, {{site.data.keyword.bpshort}} automatically download the {{site.data.keyword.cloud_notm}} provider either locally from the cache or remotely from the [Hashicorp Terraform Registry](https://registry.terraform.io/namespaces/IBM-Cloud)

To use any of the pre-defined {{site.data.keyword.bpshort}} images, you must explicitly declare the version of the {{site.data.keyword.cloud_notm}} Provider plug-in in your Terraform template that includes the provider versions that you want. For more information, see [Specifying version constraints for the Terraform CLI and Terraform providers](/docs/schematics?topic=schematics-version-constraints#version-constraints-terraform. Note that you cannot change the default version for the Ansible executable. You can only [specify the version of referenced Ansible roles and collections](/docs/schematics?topic=schematics-version-constraints#version-constraints-terraform).
{: important}

```
ibmcloud schematics version
```
{: pre}

Example output: 
```
Template Type   Version   
Terraform       terraform_v0.11   
Additional terraform Providers   Version   
Ansible                          v2.9.7   
Ansible Provisioner              v2.3.3   
IBM Cloud Provider               v0.31.0   
Open shift client                v3.11.0   
Provider for REST API            v1.10.0   
                                    
Template Type   Version   
Terraform       terraform_v0.12   
Additional terraform Providers   Version   
IBM Cloud Provider               v1.25.0   
Open shift client                v3.11.0   
Provider for REST API            v1.10.0   
Ansible                          v2.9.7   
Ansible Provisioner              v2.3.3   
                                    
Template Type   Version   
Terraform       terraform_v0.13   
Additional terraform Providers   Version   
Ansible                          v2.9.7   
Ansible Provisioner              v2.3.3   
IBM Cloud Provider               v1.25.0   
Open shift client                v3.11.0   
Provider for REST API            v1.10.0   
                                    
Template Type   Version   
Terraform       terraform_v0.14   
Additional terraform Providers   Version   
Provider for REST API            v1.10.0   
Ansible                          v2.9.7   
Ansible Provisioner              v2.3.3   
IBM Cloud Provider               v1.25.0   
Open shift client                v3.11.0 
```
{: screen}


## Specifying version constraints for the Terraform CLI and Terraform providers
{: #version-constraints-terraform}

You can choose to specify the Terraform CLI version and the version of any of the providers that you want to use by using Terraform version constraints. For more information, about how to specify version constraints, see the [Terraform documentation](https://www.terraform.io/docs/language/expressions/version-constraints.html){: external}. 
{: shortdesc}

### Version constraints for the Terraform CLI
{: #tf-version-constraint}

When you create a {{site.data.keyword.bpshort}} workspace and choose a Terraform version such as v0.13, your Terraform templates are executed by using the default patch version that is set in {{site.data.keyword.bpshort}}. For example, if you choose version v0.13, your templates are applied by using Terraform v0.13.4. You can use the `required_providers` block in your `provider` definition to force the Terraform engine in {{site.data.keyword.bpshort}} to pull a later version. 
{: shortdesc}

You can only specify versions that are higher than the default `MAJOR.MINOR.PATH` version that is set in {{site.data.keyword.bpshort}}. 
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

To use any of the pre-defined [{{site.data.keyword.bpshort}} images](#schematics-image-ov), you must explicitly declare the version of the {{site.data.keyword.cloud_notm}} Provider plug-in in your Terraform template that includes the provider versions that you want. 

If {{site.data.keyword.cloud_notm}} Provider plug-in version is not declared in your Terraform template, the latest version of the provider plug-in is automatically used in {{site.data.keyword.bpshort}}. 
{: note}

**Example to specify a pre-defined {{site.data.keyword.bpshort}} image**: </br>

The following example shows how to use the {{site.data.keyword.bpshort}} image that was built for the {{site.data.keyword.cloud_notm}} Provider plug-in v1.23.1. This image includes specific versions for other providers, such as the REST API provider. 

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

**Example to use specific Terraform provider versions**: </br>

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

{{site.data.keyword.bpshort}} currently supports the latest Ansible version v2.9.7 only. When you create a {{site.data.keyword.bpshort}} action, you must ensure that your Ansible playbooks can be run with this version. You cannot specify a specific Ansible version for your playbook. 

However, if you use existing Ansible roles or collections in your playbook, you can specify the version of the role or collection that you want to run by using a `requirements.yml` file. For more information about how to reference roles and collections in your playbook, see [Referencing Ansible roles in your playbook](/docs/schematics?topic=schematics-create-playbooks#schematics-roles) and [Referencing Ansible collections in your playbook](/docs/schematics?topic=schematics-create-playbooks#schematics-collections). To learn more about how to specify versions for roles and collections, see the [Ansible documentation](https://docs.ansible.com/ansible/latest/galaxy/user_guide.html#install-multiple-collections-with-a-requirements-file){: external}.

```
roles:
  - name: andrewrothstein.kubectl
    version: 1.1.50
```
{: codeblock}

