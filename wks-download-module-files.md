---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-02"

keywords: schematics remote host files, modules, private repository, netrc, terraform runtime process

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Using private repos for modules
{: #download-modules-pvt-git}

You can use the Terraform template to provision the resource by using the modules which are hosted on the private Git repository. At runtime when {{site.data.keyword.bpshort}} clones the private Git repository of your module templates, only the high level files are cloned. Also you need to pass Git token if your repository is private. Generally, if the template is referring to the module, the modules gets downloaded during `terraform init` command. The `terraform init` parses the high level template files and then downloads the individual modules referred in the high level template files. If your modules are in private Git repository, in {{site.data.keyword.cloud_notm}} catalog, or any other repository. The download fails to clone the files from all level as you do not have a way to pass the credentials to these module in the private repository.

The `__netrc__` configuration can be used with private and public Git repositories such a `GitHub`, `GitLab`, and `Bitbucket`.
{: note}

To overcome the download failure, {{site.data.keyword.bpshort}} supports environment variables `__netrc__`. The `__netrc__` accepts the list of `hostname`, `username` and the `password` argument. This feature is supported only in {{site.data.keyword.bpshort}} [command-line](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) and [`APIs`](/apidocs/schematics/schematics#create-workspace). The syntax is provided as an `env_values` in the JSON payload file.

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


## Usage of private module template
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
          "__netrc__":"[[`<git repository>`,`<git username>`,`<git_password>`]]"
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
          "__netrc__":"[[`github.com`,`testuser`,`ghp_x0000000xxxxxxxx000000efZxxxxxxxV`]]"
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

