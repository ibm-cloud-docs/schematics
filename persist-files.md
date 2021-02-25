---

copyright:
  years: 2017, 2021
lastupdated: "2021-02-25"

keywords: persist file, store files in pod, action persist file

subcollection: schematics

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"} 
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}


# Persist files in {{site.data.keyword.bpshort}}	
{: #persist-files}

{{site.data.keyword.bplong_notm}} persists the state and the files across your actions in the temporary folder. Then, you can use the files in subsequent activities to read from the temporary location. The persistence allows to:
- Write the files in the `/tmp/.schematics` directory. 
- The files are available for the next Terraform action such as `plan`, `apply`, `destroy`, `refresh`. 
- The size limit of the directory is set to 10 MB.
{: shortdesc}

The sample use case provides the issue you can come across when working with resources, and the importance of persistence.
 
**Use case**

When you create a Schematics workspace and apply the plan. Plan is applied and creates `addhost.sh` file in the path.module directory. However, if you again run the Terraform apply or destroy on the same workspace, the Terraform refresh command fails with an error stating `addhost.sh` file doesn't exist.

**Solution**

You can edit your Terraform configuration file to store files in `/tmp/.schematics` folder. IBM Cloud Schematics persists the state and the files in the /tmp/.schematics folder. Then, you use the files in subsequent activities from the `/tmp/.schematics` location.
