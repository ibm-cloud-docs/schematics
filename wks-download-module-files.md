---

copyright:
  years: 2017, 2025
lastupdated: "2025-09-08"

keywords: module, modules, private, private repository, private repo, private git repo, netrc, terraform, git token

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Using Terraform templates and modules in the repositories
{: #download-modules-pvt-git}

{{site.data.keyword.bpshort}} and Terraform support downloading Terraform templates and modules from a variety of repository types: Terraform Registry, GitHub, GitLab, S3/COS buckets, IBM Catalog, Artifactory so on. See [Module Sources](https://developer.hashicorp.com/terraform/language/modules/configuration#modules-in-package-sub-directories){: external} in the Terraform documentation.

When using {{site.data.keyword.bpshort}}, the downloading of Terraform templates and modules before performing a Terraform Plan or Apply operation is a two step process. At workspace create time, {{site.data.keyword.bpshort}} clones only the repository containing your template and any embedded modules in sub-folders. Any modules referenced using the module `source` parameter are not downloaded at workspace create time. Credentials to access the templates/configs in private repositories, must be passed to {{site.data.keyword.bpshort}} at workspace create time.

Modules referenced with the `source` parameter are downloaded during the `terraform init` phase of a plan or apply operation. The `terraform init` command parses the template files and downloads any modules from the repositories referenced by the `source` field. Modules residing in private repositories require additional credentials to be passed to Terraform. These credentials are defined and passed separately to those used by {{site.data.keyword.bpshort}}.

To download modules from a private Git repository, an {{site.data.keyword.cloud_notm}} catalog, or any other repository, Terraform supports the use of a [`netrc`](https://everything.curl.dev/usingcurl/netrc.html){: external} configuration to pass any required access id's and tokens.

|  Repository </br>  | Template </br> Public repo | Template </br>Private repository | Module </br>Public repository | Module </br>private repository | Comment </br>  |
| --- |--- | --- | --- | --- | --- |
| GitHub | Yes | Git token - 1  | Yes | Git token - 2 |
| GitLab | Yes | Git token - 1 | Yes | Git token - 2 |
| IBM GitLab | Yes | Git token - 1 | Yes | Git token - 2 |
| Terraform.io | No | No | Yes | NA |
{: caption="Supported Git repositories" caption-side="top"}}

1. Git token defined at workspace create time.
2. Git token defined by using `netrc`.


When using {{site.data.keyword.bpshort}}, `netrc`support for module credentials can be configured using the `__netrc__` environment variable to the pass credentials. The `__netrc__` environment variable accepts the list of `hostname`, `username` and the `password` argument. The setting of environment variables is supported only using the {{site.data.keyword.bpshort}} [command-line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) and [`APIs`](/apidocs/schematics/schematics#create-workspace). The syntax is provided using the `env_values` parameter in the JSON payload file.

The `__netrc__` expects `hostname`, `username`, and `password` argument in the same order that are listed in the syntax.
{: important}

**Syntax of `env_values` with list of `__netrc__`:**

```json
"env_values":[
            {
               "__netrc__":"[['example.com', 'user1', 'pass1']['example1.com', 'user2' , 'pass2']]"
            }
         ]
```
{: codeblock}

## Using private modules with templates
{: #netrc-example}

{{site.data.keyword.bpshort}} internally creates the `.netrc` file based on the `env_values` configured in the JSON file. Here is a syntax and sample `testexample.json` example file to clone all the files to create and apply the {{site.data.keyword.bpshort}} workspaces through command-line and API.

**Syntax with the description:**

```json
{
  "name": "<workspace_name>",
  "shared_data": {
    "region": "<region_name>"
  },
  "type": [
    "<terraform_version>"
  ],
  "description": "<description of the workspace>",
  "template_repo": {
    "url": "<your Git repository with the module>"
  },
  "template_data": [
    {
      "folder": ".",
      "type": "<terraform_version>",
      "env_values": [
        {
          "__netrc__":"[['<git repository>','<git username>','<git_password>']]"
        }
      ]
    }
  ]
}
```
{: codeblock}

Example `testexample.json` with `netrc` payload

```json
{
  "name": "testnetrcworkspaceexample",
  "shared_data": {
    "region": "us-south"
  },
  "type": [
    "terraform_v1.4"
  ],
  "description": "terraform workspace",
  "template_repo": {
    "url": "https://github.com/xxxx/test-template-private-module"
  },
  "template_data": [
    {
      "folder": ".",
      "type": "terraform_v1.4",
      "env_values": [
        {
          "__netrc__":"[['github.com','testuser','ghp_x0000000xxxxxxxx000000efZxxxxxxxV']]"
        }
      ]
    }
  ]
}
```
{: codeblock}

Example to create workspace

```sh
ibmcloud schematics workspace new --file testexample.json
```
{: pre}

Run `ibmcloud schematics workspace get --id WORKSPACE_ID` command to analyze the success workspace creation or use [user interface](https://cloud.ibm.com/schematics), to view all the files from the modules are cloned and used in your workspace to provision.
