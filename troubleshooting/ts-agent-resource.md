---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-02"

keywords: schematics resource group not found, schematics resource crn error, schematics resource crn not found

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agents are a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agent](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.

# How can you provide value to `schematics_resource_crn` variable?
{: #agent-crn-not-found}

When you run an {{site.data.keyword.bplong_notm}} plan or apply action during {{site.data.keyword.bpshort}} infrastructure workspace setup, the resource group that you try to retrieve by using the `{{site.data.keyword.bpshort}}_resource_crn` cannot be found. Following error are received.
{: tsSymptoms}

```text
 2022/05/31 09:02:30 Terraform plan | Error: No value for required variable
 2022/05/31 09:02:30 Terraform plan | 
 2022/05/31 09:02:30 Terraform plan |   on variables.tf line 48:
 2022/05/31 09:02:30 Terraform plan |   48: variable "schematics_resource_crn" {
 2022/05/31 09:02:30 Terraform plan | 
 2022/05/31 09:02:30 Terraform plan | The root module input variable "schematics_resource_crn" is not set, and has
 2022/05/31 09:02:30 Terraform plan | no default value. Use a -var or -var-file command line argument to provide a
 2022/05/31 09:02:30 Terraform plan | value for this variable.
 2022/05/31 09:02:30 [1m[31mTerraform PLAN error: Terraform PLAN errorexit status 1[39m[0m
 2022/05/31 09:02:30 [1m[31mCould not execute job: Error : Terraform PLAN errorexit status 1[39m[0m
```
{: screen}

You do not have the needed permissions to use the resource group in {{site.data.keyword.iamlong}}, and provide the value for the `schematics_resource_crn` variable.
{: tsCauses}

Check whether you have access for the `Default` or `job-runner` resource group for creating {{site.data.keyword.bpshort}} Workspace for the {{site.data.keyword.bpshort}} Agent infrastructure setup. Then, you need to provide the `schematics_resource_crn` value to create a VPE for {{site.data.keyword.bpshort}} service by using Terraform. When the VPE is created, the Agent running in your cluster, communicates to the {{site.data.keyword.bpshort}} service over {{site.data.keyword.cloud_notm}} private endpoint. To fetch the {{site.data.keyword.cloud_notm}} service instance value, run `ibmcloud resource service-instance schematics` command.
{: tsResolve}

Example : To retrieve the service instance in all resource groups from your account.

```sh
ibmcloud resource service-instance schematics
```
{: pre}

Output : From the listed output, you need to fetch the `ID` value for the `schematics_resource_crn` variable.

```text
Retrieving service instance schematics in all resource groups under account ...
Multiple service instances found
OK
                          
Name:                  schematics   
ID:                    crn:v1:bluemix:public:schematics:eu-de:a/1f7277194bb748cdbxxxxxxcb:ffc6dd7e-f129-4b04-aa5b-8xxxxx4b61d::   
GUID:                  crn:v1:bluemix:public:schematics:eu-de:a/1f7277194bb74xxxxxxx5a7cb:ffc6dd7e-f129-4b04-aa5b-895cxxxxx61d::   
Location:              eu-de   
Service Name:          schematics   
Service Plan Name:     lite   
Resource Group Name:      
State:                 active   
Type:                  service_instance   
Sub Type:                 
Created at:            2021-09-29T08:19:35Z   
Created by:            iam-ServiceId-69153c99-6bc5-4246-a497-16bxxxxx272   
Updated at:            2021-09-29T08:19:35Z   
Last Operation:                        
                       Status    create succeeded      
                       Message   Instance provisioning is completed.     
```
{: screen}
