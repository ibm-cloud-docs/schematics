---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-03"

keywords: schematics blueprints template, blueprints yaml, editing, edit, vscode, yaml 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Editing blueprint templates 
{: #edit-blueprints}

Create and customize a blueprint template to deploy the infrastructure for your solution. Templates are written in YAML and can be created and edited in any editor or IDE with YAML language support. 

The instructions here outline using [VSCode](https://code.visualstudio.com/){: external} to edit and configure blueprint templates and input files. The [Red Hat YAML VSCode extension](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml){: external}  provides a framework for editing blueprint YAML files, using a [blueprint schema](https://github.com/Cloud-Schematics/vscode-blueprint-schema){: external} defined using [JSON-Schema](https://json-schema.org){: external}. 
{: shortdesc}

The YAML language extension and blueprint schema provide:
- Validation of YAML structure and blueprint keywords
   - Blueprint specific schema validation 
- Autocomplete
   - Auto complete for blueprint template keywords and schema sections
- Hover support
   - Hovering over a keyword reveals the related schema description and links to Blueprints documentation
- Outlining for complex templates

The following steps outline editing and creating blueprint templates using VSCode and the YAML language extension. 


## Configure your VSCode environment
{: #bp-config-vscode}

Install the YAML extension and blueprint schema. Follow the Blueprints YAML language extension and blueprint schema [installation and configuration instructions](https://github.com/Cloud-Schematics/vscode-blueprint-schema){: external}. 


{{site.data.keyword.bpshort}} Blueprints works with definitions sourced from a version control system. All input files, templates, and modules are all maintained in online Git source control. Optionally [configure VScode](https://code.visualstudio.com/docs/sourcecontrol/overview) for your Git environment.


## Sourcing a blueprint template 
{: #bp-select-template}

Initially you can start by customizing the [sample template](https://github.com/Cloud-Schematics/blueprint-basic-example){: external} or use one of the deployable template examples in the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external}. 

## Clone source template repo in Git
{: #bp-clone-repo}

Create a copy of the source template in your own Git repository or version control system. These instructions refer to use with Github where both the template and input file are stored in the same repo. Separate repo's can be used to version the template and inputs independently. 

Alternatively work with your own template library in Github or Gitlab. These can be either public or private repos. 

Instructions to clone one of the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external} examples:

- Navigate to the main page of the source template repository  
- Click **Use this template/repository** 
- Select **Create a new repository** 
- Use the Owner drop-down menu, and select the account where want to create the new repository
- Type a name for your repository
- Click **Create repository from template**

## Clone repo to local machine
{: #bp-clone-repo-local}

Clone your new template repository to your local machine for editing:
- Navigate to the main page of your new repository
- Click **<> Code** (Download Code)
- [Clone the repo](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) to your local machine using: HTTPS, SSH, GitHub CLI or Github Desktop

## Editing in VSCode
{: #bp-edit-vscode}

- Open the local folder containing the template repo contents as a new VSCode workspace
- Optionally rename the template and input files for your blueprint. 
- Select the blueprint template YAML file for editing
   - The YAML language extension will provide YAML syntax and blueprint schema validation 
- Just above the top of the file contents, the selected blueprint schema is displayed. 
- Select the blueprint input YAML file for editing
   - The YAML language extension will provide YAML syntax validation 

With the extension and schema configured, VSCode will provide assisted editing for blueprint templates.

### Blueprint usage and configuration documentation
{: #bp-usage-config-doc}

To customize the template and input file to deploy your own infrastructure, refer to the Blueprint documentation for an overview of [blueprint templates and configuration](/docs/schematics?topic=schematics-blueprint-templates&interface=ui). 

### Push edited template 
{: #bp-push-template}

After editing the template push it back to the Git repo to make it available to be deployed by Schematics. 

Within VSCode, click on the “Source Control” button in the Activity Bar. It’s located on the side of VS Code’s client. Alternatively, you can use the keyboard shortcut `Ctrl+Shift+G` or `Ctrl+Shift+G` on `MacOS` to open the `Source Control` screen.

- Enter a commit message
- Click **Commit**
- Click **Sync changes**

In this example it is assumed that both the template and input file are in the same Git repo.

The blueprint template and inputs are now ready for deploying in {{site.data.keyword.bpshort}}. 

## Next steps
{: #bp-edit-nextsteps}

After pushing your initial template or update, a Git release of the template and input file can be created to enforce version control of changes. See the [Github](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository){: external} or [Gitlab](https://docs.gitlab.com/ee/user/project/releases/){: external} documentation for creating versioned blueprints releases.   

Review the section [Blueprint versioning]{/docs/schematics?topic=schematics-blueprint-versioning} to understand more about versioning blueprint environments. 

The next stage of working with your blueprint is [deploying blueprints](/docs/schematics?topic=schematics-deploy-blueprints).
