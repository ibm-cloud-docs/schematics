---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-08"

keywords: Schematics, automation, Terraform

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

# Managing the lifecycle of your resources
{: #manage-lifecycle}



## Updating your resources

Deploying changes to your environment is a lightweight process. To change which resources are allocated, you code changes to your Terraform configuration in declarative syntax, meaning you state only the outcome you want. With {{site.data.keyword.bpshort}}, you can preview your changes before deployment.


## With the console
{: #gui}

If you prefer a graphical view to manage the resources in your environments, you can use the {{site.data.keyword.bpshort}} dashboard.
{:shortdesc}

After you publish code changes to your Terraform configuration in your source control repository, or modify variables in the GUI, complete the following steps to deploy updates to your environment:

1. In the **{{site.data.keyword.bpshort}}** dashboard, select **Environments**.

2. Click the row of the specific environment that you want to update.

3. Click **Plan** and inspect your plan log for errors.

4. Click **Apply** to deploy updates.


## With the CLI
{: #cli}

You can programmatically update resources in your environments with the {{site.data.keyword.bpshort}} CLI.
{:shortdesc}

After you publish code changes to your Terraform configuration in your source control repository, complete the following steps to deploy updates to your environment:

1. Run the `plan` command. The {{site.data.keyword.bpshort}} CLI pulls the updated environment configuration from source control and outputs a preview of how your resources differ against your current deployment.

  ```
  bx schematics action plan --id ENVIRONMENT_ID
  ```
  {: codeblock}

  You can run `bx schematics environment list` if you need to retrieve your environment ID.

2. View the plan output in the activity log.

  ```
  bx schematics activity log --id ACTIVITY_ID
  ```
  {: codeblock}

3. Deploy your plan by running the `apply` command.

  ```
  bx schematics action apply --id ENVIRONMENT_ID
  ```
  {: codeblock}


## With the API
{: #api}

You can programmatically manage resources in your environments with the {{site.data.keyword.bpshort}} API.
{:shortdesc}

After you publish code changes to your Terraform configuration in your source control repository, complete the following steps to deploy updates to your environment:

1. Run the `POST v1/environments/<environment_ID>/plan` call. The {{site.data.keyword.bpshort}} API pulls the updated environment configuration from source control and compares it with the Terraform state file to show how your resources differ against your current deployment.

  ```
  curl -X POST \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/plan \
    -H 'accept: application/json' \
    -H 'authorization: bearer <OAuth_token>' \
    -H 'content-type: application/json'
  ```
  {: codeblock}

  Example response:
  ```
  {
    "activityid": "<activity_ID>"
  }
  ```
  {: screen}

2. View the plan output in the activity log.

  ```
  curl -X GET \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/activities/<activity_ID>/log \
    -H 'accept: text/html' \
    -H 'authorization: bearer <OAuth_token>'
  ```
  {: codeblock}

3. Deploy your plan by running the `PUT /v1/environments/<environment_ID>/apply` call.

  ```
  curl -X PUT \
    https://us-south.schematics.bluemix.net/v1/environments/<environment_ID>/apply \
    -H 'accept: application/json' \
    -H 'authorization: bearer <OAuth_token>' \
    -H 'content-type: application/json' \
  ```
  {: codeblock}
  
  
## Removing your resources
{: #destroy-resources}


