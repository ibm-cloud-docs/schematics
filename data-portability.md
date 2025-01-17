---

copyright:
  years: 2024
lastupdated: "2025-01-17"

keywords:

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Understanding data portability for {{site.data.keyword.bpshort}}
{: #data-portability}

Data portability involves a set of tools and procedures that enable customers to export the digital artifacts that are needed to implement similar workload and data processing on different service providers or on-premises software. It includes procedures for copying and storing the service customer content, including the related configuration that is used by the service to store and process the data, on the customer's own location.
{: shortdesc}

## Responsibilities
{: #data-portability-responsibilities}

{{site.data.keyword.cloud_notm}} services provide interfaces and instructions to guide the customer to copy and store the service customer content, including the related configuration, on their own selected location.
{: shortdesc}

The customer is responsible for the use of the exported data and configuration for data portability to other infrastructures, that includes:

- The planning and execution for setting up alternative infrastructure on different cloud providers or on-premises software that provide similar capabilities to the {{site.data.keyword.IBM_notm}} services.
- The planning and execution for the porting of the required application code on the alternative infrastructure, including the adaptation of customer's application code, deployment automation.
- The conversion of the exported data and configuration to the format that is required by the alternative infrastructure and adapted applications.

For more information about your responsibilities for {{site.data.keyword.bplong_notm}}, see [Shared responsibilities for {{site.data.keyword.bplong_notm}}](https://cloud.ibm.com/docs/schematics?topic=schematics-sc-responsibilities).

## Data export procedures
{: #data-portability-procedures}

{{site.data.keyword.bplong_notm}} provides the mechanisms to export your content that is uploaded, stored, and processed when you use the service.

In addition, {{site.data.keyword.bplong_notm}} provides mechanisms to export settings and configurations that are used to process the customer's content. For more details, see [Accessing workspace state and outputs](/docs/schematics?topic=schematics-remote-state#data-sources).

## Exported data formats
{: #data-portability-data-formats}

You can access information about the resources that you manage in a workspace from other workspaces in your account by using the [`ibm_schematics_output`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/schematics_output){: external} and [`ibm_schematics_state`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/data-sources/schematics_state){: external} data sources.

{{site.data.keyword.cloud}} uses the `local` built-in Terraform state support and does not use Terraform [backend](https://developer.hashicorp.com/terraform/language){: external} support. No additional configuration is required within your `Terraform configs` to enable {{site.data.keyword.cloud}} remote state management. {{site.data.keyword.bpshort}} does not use the Terraform `remote_state` data source, instead you use the `ibm_schematics_output` data source to access the information.

## Data ownership
{: #data-portability-ownership}

All exported data is classified as customer content and is therefore applied to them full customer ownership and licensing rights, as stated in [{{site.data.keyword.cloud_notm}} Service Agreement](https://www.ibm.com/support/customer/csol/terms/?id=Z126-6304_WS&cc=in&lc=en){: external}.
