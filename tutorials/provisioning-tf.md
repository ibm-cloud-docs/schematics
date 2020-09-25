---

copyright:
  years: 2017, 2020
lastupdated: "2020-09-25"

keywords: provisioning terraform template, provision terraform template using Schematics, terraform template with {{site.data.keyword.bpfull_notm}}, provisioning terraform template using CLI

subcollection: schematics

content-type: tutorial
services: provisioning, vpc-generation2-cluster
account-pan:
completion-time: 60m

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
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
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
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
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vb.net: .ph data-hd-programlang='vb.net'}
{:video: .video}



# Provisioning Terraform template by using {{site.data.keyword.bpfull_notm}}
{: #provisioning-terraform-template}
{: toc-content-type="tutorial"}
{: toc-services="provisioning, vpc-generation2-cluster"}
{: toc-completion-time="60m"}


## Description
{: #provisioning-desc}

In this tutorial, you can learn the procedure to provision the Terraform templates by using the {{site.data.keyword.bplong_notm}} workspace. {{site.data.keyword.containerfull_notm}} supports following clusters.
- [cluster-stand alone-workers](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cluster/cluster-standalone-workers){: external}
- [cluster-worker-pool-zone](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cluster/cluster-worker-pool-zone){: external}
- [roks-on-vpc-gen2](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cluster/roks-on-vpc-gen2){: external}
- [vpc-classic-cluster](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cluster/vpc-classic-cluster){: external} 
- [vpc-gen2-cluster](https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cluster/vpc-gen2-cluster){: external}
{: shordesc}

As part of this tutorial, you are using an {{site.data.keyword.containerfull_notm}} VPC Generation 2 cluster. The `vpc-gen2-cluster` example uses the `ibm_is_vpc`, `ibm_is_subnet`,`ibm_resource_group`, `ibm_resource_instance`, `ibm_container_vpc_cluster`, `ibm_container_vpc_worker_pool`, and `ibm_container_bind_service`.You can configure the default worker pool, multiple zones, and subnets that are provided in the example as per your business scenario. However, as part of this tutorial, you can use the required values wherever possible. <br>
The diagram and the table depicts the user flow of using the Terraform templates in the {{site.data.keyword.bplong_notm}}.

  <img src="../images/vpcgen2cluster.png" alt="Provisioning Terraform templates by using {{site.data.keyword.bplong_notm}}" width="800" style="width: 800px; border-style: none"/>
  
  | Component | Description |
  | -------- | -------- |
  | `Region` | Region increases the availability of cluster's master node and its nodes by replicating across multiple zones of a region. |
  | `VPC` | VPC provides you the security of a private cloud environment with the dynamic scalability of a public cloud. |
  | `zones` | You must have one VPC subnet for each zone in your cluster. The available zones depend on the metro location that you created in the VPC. |
  | `subnet` | VPC subnets is used to provide private IP addresses for your worker nodes and load balancer services in your cluster. You cannot change the number of IP addresses that a VPC subnet has. |
  | `master node` | Controls and manages a set of worker nodes (workloads runtime) and resembles a cluster in Kubernetes.
  | `cluster` |A cluster contains a control plane and one or more compute machines, or nodes. Nodes run the applications and workloads. |
  | `worker node` | Add the zone to your worker pool. When you add a zone to a worker pool, the worker nodes that are defined in your worker pool are provisioned in the zone and considered for future workload scheduling. 
  You can add worker nodes and pool to your VPC cluster by using a  `ibm_container_vpc_worker_pool` provider resource.
  {: note} 

  As per your resource usage the cost is incurred. For more information about the VPC pricing, refer [VPC pricing](https://www.ibm.com/cloud/vpc/pricing){: external}.
  {: important}
   

## Objectives
{: #provisioning-tut-obj}

In this tutorial, you can:
- Explore an IBM provided Terraform template to create a VPC generation 2 cluster. This binds an {{site.data.keyword.cos_full_notm}} service instance and a specified resource group ID with default worker node and a given zone and subnets.
- Learn how to create an {{site.data.keyword.bplong_notm}} workspace.
- Create a Terraform execution plan and apply your Terraform template in {{site.data.keyword.cloud_notm}}.
- Review the {{site.data.keyword.cloud_notm}} resources that you create.

## Time required
{: #provisioning-timereq}

1 hour

## Audience
{: #provisioning-tut-audience}

This tutorial is intended for developer and system administrators who want to learn how to use Terraform templates to create and manage cloud infrastructure services by using {{site.data.keyword.bplong_notm}}.

## Prerequisites
{: #Provisioning-tut-prereq}

The following prerequisites need to be met for the tutorial.
{: shortdesc}

- If you do not have {{site.data.keyword.cloud_notm}} account, create an [{{site.data.keyword.cloud_notm}} Pay-As-You-Go]. For more information about managing {{site.data.keyword.cloud_notm} account, refer [Managing IBM Cloud account](https://cloud.ibm.com/registration).
- Install the IBM Cloud CLI and the Schematics CLI plug-in. For more information about CLI setup, see [Schematics CLI setup](/docs/schematics?topic=schematics-setup-cli).
- Make sure that you are assigned the required permissions in Identity and Access Management to create and work with {{site.data.keyword.bplong_notm}} workspace. Refer [Schematic access](/docs/schematics?topic=schematics-access#access-roles) and to create an {{site.data.keyword.cos_full_notm}} service instance. 
- Follow the instructions to make sure that you are assigned the required permissions in Identity and Access Management to create clusters. For more information about container cluster, refer [Containers clusters](/docs/containers?topic=containers-clusters#cluster_prepare).

## Accessing and reviewing your Terraform template
{: #access-review-template}
{: step}

Follow the steps to identify the correct Terraform templates:
{: shortdesc}

1. Log in to your [GitHub](https://github.com/) account. 
2. Open the Terraform template to create a VPC generation 2 cluster. Click [VPC generation 2 cluster]( https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cluster/vpc-gen2-cluster){: external}
3. Review the Terraform template files. 
    - **main.tf**: This file includes the Terraform code to create a region, VPC, multi zones, two subnets, a VPC generation 2 cluster, and an {{site.data.keyword.cos_full_notm}} service instance that is bound with the cluster, master node, and a worker node.
    - **output.tf**: This file includes the return results to the calling resources. You can then use to populate the arguments.
4. Note the URL of the Terraform template in GitHub to be used in your Schematics workspace setup. 

## Creating your {{site.data.keyword.bplong_notm}} workspace
{: #create-tut-wks}
{: step}

1. Specify your Schematics workspace setting by copying the following workspace JSON file and saving it as `cluster_payload.json` on your local machine. For more information, about the payload parameters, refer [{{site.data.keyword.bplong_notm}} workspace new ](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) command.

**Example of the cluster_payload.json**

```
{
  "name": "mytest1_cluster",
  "type": [
    "terraform_v0.12"
  ],
  "description": "",
  "template_repo": {
  	"url":"https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-cluster/vpc-gen2-cluster"
  },
  "template_data": [
    {
      "folder": ".",
      "type": "terraform_v0.12",
      "variablestore": [
        {
          "name": "worker_pool_name",
          "value": "workerpool",
          "type": "string"
        },
        {
          "name": "service_instance_name",
          "value": "myservice",
          "type": "string"
        },
        {
          "name": "flavor",
          "value": "c2.2x4",
          "type": "string"
        },
        {
          "name": "cluster_name",
          "value": "cluster",
          "type": "string"
        },
        {
          "name": "region",
          "value": "North America",
          "type": "string"
        },
        {
          "name": "worker_count",
          "value": "1",
          "type": "string"
        },
        {
          "name": "resource_group",
          "value": "Default",
          "type": "string"
        }
      ]
    }
  ],
  "githubtoken": "<provide your githubtoken>"
}
```
{: codeblock}

You can edit the payload values for the variable as stated in the table:

| Variable name | Value |
|-------|------|
| `name` | Specify your unique name. |
| `type` | Terraform v0.12 |
| `githubtoken` | Specify your GitHub token. |

2. Create the workspace by using the JSON file from command line interface.

   ```
   ibmcloud schematics workspace new --file <fully qualified path of cluster_payload.JSON file>
   ```
   {: pre}

   For more information to create workspace, refer [CLI commands and syntax](/docs/schematics?topic=schematics-schematics-cli-reference). 
   {: note}

    **Sample example output**
  
   ```
   Creation Time   Mon Aug 10 19:18:55
   Description
   Frozen          false
   ID              mytest1_cluster-62183a6b-fbed-43
   Name            mytest1_cluster
   Status          DRAFT

   Template Variables for: examples-d3d10ae5-76ef-47
   Name                    Value
   worker_pool_name        workerpool
   service_instance_name   myservice
   flavor                  c2.2x4
   cluster_name            cluster
   region                  us-south
   worker_count            1
   resource_group          Default

   OK
   ```
   {: codeblock}

   You can also view the new workspace `mytest1_cluster` in {{site.data.keyword.cloud_notm}} dashboard.
   {: note}

3. Verify that your workspace is created by using `list` command.

   ```
   ibmcloud schematics workspace list
   ```
   **Sample example output**
  
   ```
   Name               ID                              Description     Status      Frozen
   mytest1_cluster  mytest1_cluster-62183a6b-fbed-43                  ACTIVE       False
   
   OK
   ```
   {: preblock}

## Planning and applying the Terraform template
{: #tut-plan-wks}
{: step}

Create a Schematics execution plan. The execution plan shows the {{site.data.keyword.cloud_notm}} resources that must be added, modified, or removed to achieve the state that is described in your Terraform template.

Your workspace must be in an `Active` state to perform a Schematics plan action. For more information on the workspace state, refer [Workspace states](/docs/schematics?topic=schematics-workspace-setup#workspace-states).
{: note}

During the creation of the Terraform execution plan, you are not allowed to make any changes to your workspace.
{: note}

1. Execute the Schematics plan command. This command gives back an activity ID. 

   ```
   ibmcloud schematics plan --id mytest1_cluster-62183a6b-fbed-43
   ```
   {: pre}

   **Sample example output**
   
   ```
   Activity ID 3886e3752a0a83b04732b6666533b464

   OK
   ```
   {: pre}

   The activity ID is used to retrieve the logs of the execution plan.
   {: note}

2. Review the execution plan to view the {{site.data.keyword.cloud_notm}} resources. To retrieve the logs with the activity ID use the generated activity ID from step 1.

   ```
   ibmcloud schematics logs --id mytest1_cluster-62183a6b-fbed-4
   ```
   You can view the output from your working directory, or from the IBM Cloud dashboard to view the workspace status.
     {: note}

3.	Apply your Terraform template in IBM Cloud. When you apply your Terraform template, all the IBM Cloud resources that are specified in the template are created in your IBM Cloud account. 

    This process takes few minutes to complete. During this process, you cannot make any changes to your workspace. 
    {: important}

    ```
    ibmcloud schematics apply --id <workspace_ID>
    ```
    {: pre}
    
    **Sample example output**
    ```
    Do you really want to perform this action? [y/N]> y

    Activity ID 5676e3752a0a84565667666533b4345

    OK
    ```
    
4. Review the logs of your workspace. See step 2 to view the logs with the workspace ID or  activity ID.

5. Verify that the {{site.data.keyword.cloud_notm}} resources are successfully created in your {{site.data.keyword.cloud_notm}}.

   ```
   ibmcloud schematics workspace get --id <WORKSPACE_ID>
   ```
   {: pre}

   Alternatively, through the {{site.data.keyword.cloud_notm}} dashboard, you can view the status of the workspace. From the {{site.data.keyword.cloud_notm}}, select ** Navigation Menu - Schematics - Workspaces - Resources ** to observe the apply state of the resources in your workspace.
   {: note}

6. Command to view the logs, and analyze the state of the workspace and resources creation.

   ```
   ibmcloud schematics logs --id mytest1_cluster-62183a6b-fbed-43
   ```
   
   You can view the output from your working directory, or from the IBM Cloud dashboard plan logs to view the workspace status.
  {: note}
 
   
  
   
## What's next?
{: #tut_what's next}

Great job! You successfully provisioned a VPC Generation 2 cluster by using {{site.data.keyword.bplong_notm}}. You can now learn how to configure the cluster parameters to attach the key management services and load balancer. For more information, about key management services and {{site.data.keyword.cloud_notm}} Kubernetes worker pool, see [Key Management services](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#ks_kms_enable) and [{{site.data.keyword.cloud_notm}} Kubernetes worker pool](/docs/containers-cli-plugin?topic=containers-cli-plugin-kubernetes-service-cli#cs_alb_create).

