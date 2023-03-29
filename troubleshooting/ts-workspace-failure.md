---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-29"

keywords: workspace create failure, terraform error, terraform fails, workspace fails

subcollection: schematics

content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}


# Workspace fails 
{: #wks-failure}

Review the following sections to help debugging `workspace new` failures. 

## Workspace create fails with message `Request Entity Too Large`
{: #wks-new-fails1}

When you create a workspace from an existing `terraform.tfstate` file, it fails with error code `413` and a message `Required Entiry Too Large`.    
{: tsSymptoms}

Your workspace create did not reach the {{site.data.keyword.bpshort}}, due to the size limitation of the `terraform.tfstate` file.
{: tsCauses}

Example error message

```text
FAILED
Could not create the workspace. Please verify that your request is correct. If the problem persists, contact IBM Cloud support.

Status Code: 413
Message:

<title>413 Request Entity Too Large</title>
```
{: screen}

Check the size of an existing `terraform.tfstate` file size. Rerun the workspace create operation with a size that is less than 2 MB state file.
{: tsResolve} 
