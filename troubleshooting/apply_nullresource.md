---

copyright:
  years: 2017, 2022
lastupdated: "2022-10-27"

keywords: schematics, schematics action, create schematics actions, run ansible playbooks, delete schematics action, 

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}


# How can you find the root cause of why {{site.data.keyword.bpshort}} apply is failing?
{: #nullresource-errors}

You want to apply a Terraform template in {{site.data.keyword.cloud_notm}} that runs scripts on a target resource. To run the script, you use the Terraform `null_resource` in your configuration file. However, when you run the {{site.data.keyword.bpshort}} apply action, the action fails and you receive an error message that can include internal, timeout, connection, or input errors. 
{: tsSymptoms}

When {{site.data.keyword.bpshort}} tries to run your script on the target resource, a script error occurs during the execution. Because {{site.data.keyword.bpshort}} cannot resolve errors that occur in user-provided scripts, the apply action is marked as `failed`.
{: tsCauses}

To troubleshoot the error in the script, follow these steps:
{: tsResolve}

1. From the workspace **Activity** page, select the {{site.data.keyword.bpshort}} apply action that failed.
2. Click **Jobs** to see the detailed log output. 
3. In the log file, find the last action that {{site.data.keyword.bpshort}} started before the error occurs. For example, in the following log output, {{site.data.keyword.bpshort}} tried to run a copy script in the `instances_module` module by using the Terraform `null_resource`.
    ```text
    2021/05/24 05:03:41 Terraform apply | module.instances_module.module.compute_remote_copy_rpms.null_resource.remote_copy[0]: Still creating... [5m0s elapsed]
    2021/05/24 05:03:41 Terraform apply | 
    2021/05/24 05:03:42 Terraform apply | 
    2021/05/24 05:03:42 Terraform apply | 
    2021/05/24 05:03:42 Terraform apply | Error: timeout - last error: ssh: rejected: connect failed (Connection timed out)
    ```
    {: screen}

4. Find the script that is ran in the Terraform `null_resource` and analyze where the error might come from by using the error message from the {{site.data.keyword.bpshort}} log output. 
5. If you cannot resolve the error, contact the script owner to further troubleshoot the error. 



