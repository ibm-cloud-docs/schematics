---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-19"

keywords: schematics faqs, infrastructure as code, iac, schematics faq, 

subcollection: schematics

content-type: faq

---

{{site.data.keyword.attribute-definition-list}}

# General
{: #general-faq}

Answers to common questions about the {{site.data.keyword.bplong_notm}} are classified into following section.
{: shortdesc}

## What is {{site.data.keyword.bplong_notm}} and how does it work? 
{: #what-is-schematics}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} provides powerful tools to automate your cloud infrastructure provisioning and management process, the configuration and operation of your cloud resources, and the deployment of your app workloads.

To do so, {{site.data.keyword.bpshort}} uses open source projects, such as Terraform, Ansible, Red Hat OpenShift, Operators, and Helm, and delivers these capabilities to you as a managed service. Rather than installing each open source project on your system, and learn the API or CLI. You can declare the tasks that you want to run in {{site.data.keyword.cloud_notm}} and watch {{site.data.keyword.bpshort}} run these tasks for you.

For more information about how {{site.data.keyword.bpshort}} Works, see [About {{site.data.keyword.bplong_notm}}](/docs/schematics?topic=schematics-learn-about-schematics).

## What is Infrastructure as Code?
{: #what-is-iac}
{: faq}

Infrastructure as Code (IaC) helps you codify your cloud environment so that you can automate the provisioning and management of your resources in the cloud. Rather than manually provisioning and configuring infrastructure resources or by using scripts to adjust your cloud environment, you use a high-level scripting language to specify your resource and its configuration. Then, you use tools like Terraform to provision the resource in the cloud by using its API. Your infrastructure code is treated the same way as your app code so that you can apply DevOps core practices such as version control, testing, and continuous monitoring.

## What am I charged for when I use {{site.data.keyword.bpshort}}?
{: #charges}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} Workspaces are provided to you at no cost. However, when you decide to apply your Terraform template in {{site.data.keyword.cloud_notm}} by clicking `Apply plan` from the workspace details page or running the `ibmcloud schematics apply` command, you are charged for the {{site.data.keyword.cloud_notm}} resources that are described in your Terraform template. Review available service plans and pricing information for each resource that you are about to create. Some services come with a limit per {{site.data.keyword.cloud_notm}} account. If you are about to reach the service limit for your account, the resource is not provisioned until you increase the service quota, or remove existing services first.

The {{site.data.keyword.bpshort}} `ibmcloud terraform` command usage displays warning and deprecation message as `Alias Terraform are deprecated. Use schematics or sch` in your command.
{: note}

## Does {{site.data.keyword.bpfull_notm}} support multiple Terraform provider versions?
{: #provider-versions}
{: faq}
{: support}

Yes, {{site.data.keyword.bpfull_notm}} supports multiple Terraform provider versions. You need to add Terraform provider block with the right provider version. By default the provider run current version `1.21.0`, and previous four versions such as `1.20.1`, `1.20.0`, `1.19.0`, `1.18.0` are supported.

Example for a multiple provider configuration:

```terraform
terraform{
    required_providers{
    ibm = ">= 1.21.0" // Error !! version unavailable.
    ibm = ">= 1.20.0" // Execute against latest version.
    ibm = "== 1.20.1" // Executes version v1.20.1. 
    }
}

```

Currently, version 1.21.0 is released. For more information, see [provider version](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-setup_cli#install_provider).
{: note}


## How do I generate IAM access token, if client ID `bx` is used?
{: #createworkspace-generate-tokens}
{: faq}
{: support}

To create IAM access token, use `export IBMCLOUD_API_KEY=<ibmcloud_api_key>` and run the command.

```sh
curl -X POST "https://iam.cloud.ibm.com/identity/token" -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=urn:ibm:params:oauth:grant-type:apikey&apikey=$IBMCLOUD_API_KEY" -u bx:bx.
``` 

For more information, see [IAM access token](/apidocs/iam-identity-token-api#gettoken-password) and [Create API key](/apidocs/iam-identity-token-api#create-api-key). You can set the environment values `export ACCESS_TOKEN=<access_token>`, and `export REFRESH_TOKEN=<refresh_token>`. 

## How do I rectify 'Failed to clone Git repository, might not find remote ref `refs/heads/master` (most likely invalid branch name is passed)'?
{: #template-errors}
{: faq}
{: support}

Usage of the branch `https://github.com/guruprasad0110/tf_cloudless_sleepy_13/` repository, after 1st October 2020, can see this error message. 

If the repository is created after 1 October 2020, the main branch syntax needs to be `https://github.com/username/reponame/tree/main`. For example, `https://github.com/guruprasad0110/tf_cloudless_sleepy_13/tree/main`

## Can I increase the timeout for null-exec and remote-exec resource?
{: #timeout-null-resource}
{: faq}
{: support}

No, the null-exec (`null_resources`) and remote-exec resources has maximum timeout of `60 minutes`. Longer jobs need to be broken into shorter blocks to provision the infrastructure faster. Otherwise, the execution times out automatically after `60 minutes`.

## How can I save user-defined files that are generated by the Terraform modules and use them across multiple Terraform plan, apply, destroy, refresh, or import commands?
{: #persist-file}
{: faq}
{: support}

{{site.data.keyword.bplong_notm}} already stores and securely manages the state file generated by the Terraform engine in a {{site.data.keyword.bpshort}} Workspace. {{site.data.keyword.bpshort}} periodically saves the state file in the secured location. Further the state file is automatically restored before executing the {{site.data.keyword.bpshort}} job or Terraform plan, apply, destroy, refresh, or import commands.

In the same way {{site.data.keyword.bplong_notm}} supports the ability to store user-defined files that are generated by the Terraform template or modules. {{site.data.keyword.bpshort}} expects the user-defined Terraform template or modules to generate and place the files in a predefined location. {{site.data.keyword.bpshort}} will automatically save and restore them before and after running the {{site.data.keyword.bpshort}} jobs or Terraform command.

Your files must be placed in the `/tmp/.schematics` folder and the size limit is set to `10 MB`. {{site.data.keyword.bpshort}} backups and restores all the files in the `/tmp/.schematics` folder.

## How do I identify the best way to synchronize a deleted resource with the Terraform state?
{: #sync-delresource-terraform}
{: faq}
{: support}

Currently, the {{site.data.keyword.bplong_notm}} service does not support the ability to import or synchronize the {{site.data.keyword.cloud_notm}} resource state into the {{site.data.keyword.bpshort}} Workspace. It is planned in the future roadmap.

## How do I overcome the request exceeds the Cluster resource quota of '100' for the account in any region?
{: #clusterquota-warn-faq}
{: faq}
{: support}

```text
Error: Request failed with status code: 403, ServerErrorResponse: {"incidentID":"706efb2c-3461-4b9d-a52c-038fda3929ea,706efb2c-3461-4b9d-a52c-038fda3929ea","code":"E60b6","description":"This request exceeds the 'Cluster' resource quota of '100' for the account in this region. Your account already has '100' of the resource in the region, and the request would add '1'. Revise your request, remove any unnecessary resources, or contact IBM support to increase your quota.","type":"General"}
```

You see this quota validation error when the `Cluster` resource quota of `100` for the account in this region is exceeded. You can consider deleting the existing resources and try running operation again.

## While creating Red Hat OpenShift or Kubernetes resources, can I tune 90 minutes time out to higher?
{: #resourcetimeout-warn-faq}
{: faq}
{: support}

Yes, you can increase the timeout for Red Hat OpenShift or Kubernetes resources. For more information, see [ibm_container_vpc_cluster](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/container_vpc_cluster#timeouts) provides the following Timeouts configuration options.

## How can I rectify the 403 Error while validating the location in the account. Verify you have permission to the location in the global catalog settings?
{: #global-setting-location}
{: faq}
{: support}

You can verify the location access to create or view the resource in the catalog settings for your account. For more information, see [Manage location settings in global catalog](/docs/schematics?topic=schematics-access-ibm-cloud-catalog).

## Can I start or stop the {{site.data.keyword.vsi_is_short}} based on tags and through scheduler or cron job?
{: #vm-tags-faq}
{: faq}
{: support}

 Yes, you can use {{site.data.keyword.openwhisk_short}} to set the managed operations such as start, stop query based on tags and also through scheduler or cron job to trigger the {{site.data.keyword.bpshort}} action. For more information, see [VSI operations and schedule solution](https://github.com/Cloud-Schematics/vsi-operations-scheduler-solution){: external} GitHub repository.
 
## Might I create a worker node in an existing worker node pool?
{: #workernode-kubernetes-faq}
{: faq}
{: support}

Yes, you can create or add a worker node inside an existing worker node pool by using {{site.data.keyword.IBM_notm}} container worker pool resource in a Kubernetes cluster through {{site.data.keyword.bpshort}}. Or Terraform by using {{site.data.keyword.IBM_notm}} container worker pool zone attachment resource. For more information, see [`ibm_container_worker_pool_zone_attachment`](https://registry.terraform.io/providers/IBM-Cloud/ibm/latest/docs/resources/container_worker_pool_zone_attachment){: external}.

## Where can I view the list of public and private allowed IP addresses of `us-south`, `us-east`, `eu-gb`, and `eu-de` regions?
{: #privateip-workspace-faq}
{: faq}
{: support}

You can view the list of public and private allowed IP addresses of `us-south`, `us-east`, `eu-gb`, and `eu-de` regions in [{{site.data.keyword.bpshort}} allowed IP addresses](/docs/schematics?topic=schematics-allowed-ipaddresses).

## Can I manually add, or remove a resource from the service dashboard directly?
{: #add-remove-resource-faq}
{: faq}
{: support}

When you provision resources with {{site.data.keyword.bplong_notm}}, the state of your resources is stored in a local {{site.data.keyword.bplong_notm}} state file. This state file is the single source of truth for {{site.data.keyword.bplong_notm}} to determine what resources are provisioned in your {{site.data.keyword.cloud_notm}} account. If you manually add a resource without {{site.data.keyword.bplong_notm}}, this resource is not stored in the {{site.data.keyword.bplong_notm}} state file, and as a consequence cannot be managed with {{site.data.keyword.bplong_notm}}. 

When you manually remove a resource that you provisioned with {{site.data.keyword.bplong_notm}}, the state file is not updated automatically and becomes out of sync. When you create your next Terraform execution plan or apply a new template version, {{site.data.keyword.bpshort}} verifies that the {{site.data.keyword.cloud_notm}} resources in the state file exist in your {{site.data.keyword.cloud_notm}} account with the state that is captured in your state file. If the resource is not found, the state file is updated, and the Terraform execution plan are changed.

To keep your {{site.data.keyword.bplong_notm}} state file and the {{site.data.keyword.cloud_notm}} resources in your account in sync, use {{site.data.keyword.bplong_notm}} to provision, or remove your resources. 
{: important}

## What changes can I make to my resources?
{: #resource-faq}
{: faq}
{: support}

You can choose to add, modify, or remove infrastructure code in your Terraform template through GitHub, or update variable values from the {{site.data.keyword.bpshort}} Workspaces dashboard. 

## How can I compare the required state of my cloud resources against the actual state of my resources?
{: #required-resource-state-faq}
{: faq}
{: support}

To create a deviation report and view the changes between the infrastructure and platform services that you specified in your Terraform configuration files. You can use Terraform execution plans. A Terraform execution plan summarizes what actions {{site.data.keyword.bpshort}} needs to take to provision the cloud environment that is described in your Terraform configuration files. These actions can include adding, modifying, or removing {{site.data.keyword.cloud_notm}} resources.

## What are the deviations that cannot be detected?
{: #edit-resource-faq}
{: faq}
{: support}

- A Terraform execution plan is based on the [Terraform state file](/docs/schematics?topic=schematics-schematics-cli-reference#state-file-cmds) that is created when you run your first {{site.data.keyword.bpshort}} apply action. 
- Resources that you provisioned in other {{site.data.keyword.bpshort}} Workspaces by using automation tools such as `Ansible`, or `Chef` that are added without {{site.data.keyword.bpshort}} are not considered included in the Terraform execution plan.

## How must I remove resources with {{site.data.keyword.bplong_notm}}?
{: #remove-resource-faq}
{: faq}
{: support}

You can use the {{site.data.keyword.bplong_notm}} console or CLI to remove all the resources that you provisioned with {{site.data.keyword.bpshort}}. To stay in synchronize with your Terraform template, make sure to remove the associated infrastructure code from your Terraform template. So that, your resources are not added again when you apply a new version of your Terraform template. 

## What happens if I choose to delete my resource directly from the resource dashboard?
{: #delete-resource-directly-faq}
{: faq}
{: support}

When you manually remove a resource that you provisioned with {{site.data.keyword.bplong_notm}}, the state file is not updated automatically and becomes out of sync. When you create next Terraform execution plan, or apply a new template version. The {{site.data.keyword.bpshort}} verifies that the {{site.data.keyword.cloud_notm}} resources in the state file exist in your {{site.data.keyword.cloud_notm}} account with the state that is captured. If the resource is not found, the state file is updated, and the Terraform execution plan is changed. 

Although the state file is updated before new changes to your {{site.data.keyword.cloud_notm}} resources are applied, do not manually remove resources from the resource dashboard to avoid unexpected results. Instead, use the {{site.data.keyword.bplong_notm}} console or CLI to remove your resources, or remove the associated infrastructure code from your Terraform template. 
{: important}

## Does {{site.data.keyword.bpshort}} supports `ibmcloud terraform` command?
{: #ibmcloud-terraform-cmd-faq}
{: faq}
{: support}

Using `ibmcloud terraform` command from CLI release v1.8.0 displays a warning message as `Alias Terraform are deprecated. Use schematics or sch in your commands`. For more information, see [CLI version history](/docs/schematics?topic=schematics-cli_version-releases).

## Can I access private network through {{site.data.keyword.bpshort}}?
{: #private-endpoint-faq}
{: faq}
{: support}

Yes, from [CLI release v1.8.0](/docs/schematics?topic=schematics-cli_version-releases) {{site.data.keyword.bpshort}} supports private {{site.data.keyword.bpshort}} endpoint to access your private network. For more information, see [private {{site.data.keyword.bpshort}} endpoint](/docs/schematics?topic=schematics-private-endpoints#private-cse).

## How can I resolve the error message when connecting to Bastion host IP addresses through {{site.data.keyword.bplong_notm}}?
{: #bastion-ipaddress-faq}
{: faq}
{: support}

**Error:**

```text
timeout - last error: Error connecting to bastion: dial tcp
 2022/03/02 03:59:37 Terraform apply | 52.118.101.204:22: connect: connection timed out
 2022/03/02 03:59:37 Terraform apply | 
 2022/03/02 03:59:37 Terraform apply | Error: file provisioner error
```
{: screen}

You can access your {{site.data.keyword.bpshort}} Workspaces, and connect to Bastion host IP addresses for your region or zone based on the private or public endpoint IP addresses. For more information, see [Opening the IP addresses for the {{site.data.keyword.bplong_notm}} in your firewall](/docs/schematics?topic=schematics-allowed-ipaddresses).

## How do I create a cluster by using Terraform on {{site.data.keyword.cloud_notm}} environment?
{: #newcluster-workspace-faq}
{: faq}
{: support}

You can see create [single and multizone {{site.data.keyword.openshiftshort}}, and {{site.data.keyword.containershort_notm}} cluster](/docs/ibm-cloud-provider-for-terraform?topic=ibm-cloud-provider-for-terraform-tutorial-tf-clusters#create-cluster) tutorial.

## Can I always set Terraform to use the current or default version?
{: #terraform-defaultversion-faq}
{: faq}
{: support}

Yes, in the payload or JSON file, if the value for the `type` and `template_type` parameter is not declared, at runtime, the default Terraform version is considered. For more information, see [specifying version constraints for the Terraform](/docs/schematics?topic=schematics-version-constraints#version-constraints-terraform).
You can specify the Terraform version in the payload by using the `type` or `template_type` parameter. However, check whether the version value for the `type` and `template_type` contains the same version.

## If I set `"type”: = “terraform_v1.0"` in the JSON file as shown in the code block, will `Terraform version 1.0 continue to use even if Terraform version 2.0 or higher` are released?
{: #terraform-type-faq}
{: faq}
{: support}

    ```terraform
    //Sample JSON file
    {
    "name": "<workspace_name>",
    "type": "terraform_v1.0",
    "resource_group": "<resource_group>",
    "location": "",
    "description": "<workspace_description>",
    "template_repo": {
    "url": "http://xxxxx.git",
    "branch": "main"
    },
    "template_data": [{
    "folder": "",
    "type": "terraform_v1.0"
    }]
    }
    ```
    {: codeblock}

No, if the Terraform version is specified in the payload or template, only the version that is specified in `versions.tf` is considered during provisioning. To consider the current Terraform version, you can configure the `required_version` parameter as `required_version = ">=1.0.0. <2.0"`. For more information, see [Version constraints for the Terraform](/docs/schematics?topic=schematics-version-constraints#tf-version-constraint).

## Can I specify only the provider version in the `version` parameter? Or is it mandatory to provide the `required_version` parameter in the `versions.tf` file?
{: #terraform-reqparam-faq}
{: faq}
{: support}

Yes, you need to specify the `version = "x.x.x"` as it signifies the {{site.data.keyword.cloud_notm}} provider version. Whereas `required_version = ">1.0.0, <2.0"` signifies the Terraform version to provision. For more information, see [Version constraints for the Terraform](/docs/schematics?topic=schematics-version-constraints#tf-version-constraint).
If the version parameter is not declared in your `versions.tf` file, the current version of the provider plug-in is automatically used in {{site.data.keyword.bpshort}}. For more information, see [Version constraints for the Terraform providers](/docs/schematics?topic=schematics-version-constraints#provider-version-contraint).

## What is the difference between delete, and destroy in {{site.data.keyword.bpshort}}?
{: #faq-delete-destroy}
{: faq}
{: support}

Destroy delete the associated cloud resource from the workspace. Delete workspace is to used to delete the workspace. The recommendation is to destroy the resource first from the workspace, and then set delete workspace. For more information, see [Deleting a workspace](/docs/schematics?topic=schematics-workspace-setup#del-workspace)

## Can I delete and destroy operation as one step?
{: #faq-delete-destroy-operation}
{: faq}
{: support}

No, you cannot delete and destroy operation in one step. It is the [process](/docs/schematics?topic=schematics-workspace-setup#del-workspace) that you need to follow to destroy first and delete.
