---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-28"

keywords: compact, subdirectory, schematics, download, directory

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Compact config repo download
{: #compact-download}

{{site.data.keyword.bpshort}} Workspaces act as a container of a Terraform template with the input data, output state, jobs, and job log files. The workspace uses the Git URL of the Terraform template. For example, if the user provides the URL `https://github.com/terraform-ibm-modules/terraform-ibm-database/tree/main/examples/simple-etcd` to download and securely store the template files. The default execution of the {{site.data.keyword.bpshort}} is to download the whole Git repository and to save securely. {{site.data.keyword.bpshort}} assumes that the Terraform templates have relative references to modules, script, or data files that reside in other folders in the repository. 

However, sometimes, the user may be aware that the Terraform templates are isolated to a specific folder and its `subfolders`, for example, `examples/simple-etcd` in the Git repository. Now the user wants the workspace to download, only the relevant files from the Git repository. This can be achieved by using the **compact download** feature. The compact download feature improves the time it takes to download, and process the files from the Git repository while creating, or updating the workspace.

## Using compact download
{: #compact-active}

You can activate **compact download** feature through `console` by unchecking the `Download entire repo` checkbox on the [Create a {{site.data.keyword.bpshort}} Workspace page](https://cloud.ibm.com/schematics/workspaces/create). By default, the checkbox is selected to download the full Git repository.

You can also activate **compact download** through the `API or CLI` by using `compact` field in the workspace [create](/apidocs/schematics/schematics#create-workspace), or [update](/apidocs/schematics/schematics#replace-workspace) payload, as illustrated in the code block.
```json
{
        "name": "Testmyservice",
        "type": [
                "terraform_v1.0"
        ],
        "description": "Create Terraform workspace to test compact feature",
        "tags": [
                "test:bbbranch"
        ],
        "template_repo": {
                "url": "https://github.com/IBM-Cloud/terraform-provider-ibm/tree/master/examples/ibm-resource-instance"
        },
        "template_data": [{
                "folder": ".",
                "compact": true,
                "type": "terraform_v1.0"
    }]
}
```
{: pre}

## Note
{: #compact-note}

- In case the **compact** field is not specified in the workspace create request payload, the default execution is `full repository download` or `compact : false`.
- In case the **compact** field is absent in the workspace update request payload, the default execution uses the previous `compact` setting.
- Git repository URL is mandatory in both [create](/apidocs/schematics/schematics#create-workspace), or [update](/apidocs/schematics/schematics#replace-workspace) workspace request payload only if compact flag is set.
- The [GET workspace](/apidocs/schematics/schematics#get-workspace) response includes the compact field, only if the **compact** download mode is enabled.
- If the Git repository URL is the root of the repository, as stated in this [template](https://github.com/Cloud-Schematics/LEMP), the compact download, and full download are the exact same thing. It doesn't matter if the compact checkbox is `checked` or `unchecked`.
