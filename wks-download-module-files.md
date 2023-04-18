---

copyright:
  years: 2017, 2023
lastupdated: "2023-04-18"

keywords: schematics remote host files, modules, private repository, netrc, terraform runtime process

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Using modules in private repos
{: #download-modules-pvt-git}

You can use Terraform templates to provision resources using modules which are hosted in private Git repositories. At workspace create time {{site.data.keyword.bpshort}} will only clone the Git repository containing your template and any embedded modules in sub-folders. Any modules referenced in additional Git repos or Catalogs are not downloaded. Referenced modules are downloaded during the `terraform init` phase of a plan or apply operation. The `terraform init` parses the template files and downloads any modules referenced by the template files. To download modules from a private Git repository, an {{site.data.keyword.cloud_notm}} catalog, or any other repository, the credentials for the repository must be passed. 

To provide the credentials a `__netrc__` configuration can be used with private and public Git repositories such a `GitHub`, `GitLab`, and `Bitbucket`.
{: note}

{{site.data.keyword.bpshort}} supports using the environment variable `__netrc__` to pass credentials. The `__netrc__` variable accepts the list of `hostname`, `username` and the `password` argument. This feature is supported only in {{site.data.keyword.bpshort}} [command-line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) and [`APIs`](/apidocs/schematics/schematics#create-workspace). The syntax is provided using the `env_values` parameter in the JSON payload file.

The `__netrc__` expects `hostname`, `username`, and `password` argument in the same order that are listed in the syntax. 
{: important}

**Syntax of `env_values` with list of `__netrc__`:**

```json
"env_values":[
            {
               "__netrc__":"[['example.com', 'user1', 'pass1']['example1.com', 'user2' , 'pass2']]"
            }
         ],
```
{: codeblock}


## Using private modules with templates
{: #netrc-example}

{{site.data.keyword.bpshort}} internally creates the `.netrc` file based on the `env_values` configured in the JSON file. Here is a syntax and sample `testexample.json` example file to clone all the files to create and apply the {{site.data.keyword.bpshort}} Workspaces through command-line and API.

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

**Example `testexample.json` with `netrc` payload:**

```json
{
  "name": "testnetrcworkspaceexample",
  "shared_data": {
    "region": "us-south"
  },
  "type": [
    "terraform_v1.0"
  ],
  "description": "terraform workspace",
  "template_repo": {
    "url": "https://github.com/xxxx/test-template-private-module"
  },
  "template_data": [
    {
      "folder": ".",
      "type": "terraform_v1.0",
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

**Example to create workspace:**
```sh
ibmcloud schematics workspace new --file testexample.json
```
{: pre}

Run `ibmcloud schematics workspace get --id WORKSPACE_ID` command to analyze the success workspace creation or use [user interface](https://cloud.ibm.com/schematics), to view all the files from the modules are cloned and used in your workspace to provision.

