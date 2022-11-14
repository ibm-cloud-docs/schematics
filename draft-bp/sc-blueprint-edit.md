---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-14"

keywords: schematics blueprints template, blueprints yaml, editing, edit, vscode, yaml 

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Editing blueprint templates 
{: #edit-blueprints}

Create and customize a blueprint template to deploy the infrastructure for your solution. Templates are written in YAML and can be created and edited in any editor or IDE with YAML language support. 

The instructions here outline using [VSCode](https://code.visualstudio.com/){: external}  to edit and configure blueprint templates and input files. The [Red Hat YAML VSCode extension](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml){: external}  provides a framework for editing blueprint YAML files, using a [blueprint schema](https://github.com/Cloud-Schematics/vscode-blueprint-schema){: external} defined using [JSON-Schema](https://json-schema.org){: external}. 
{: shortdesc}

YAML language and blueprint schema support:
- Validation of YAML structure and blueprint keywords
  - Blueprint specific schema validation 
- Autocomplete
  - Auto complete for template keywords and schema sections
- Hover support
  - Hovering over a keyword reveals the related schema description and links to documentation
- Outlining for complex templates  


## Configuring your VSCode environment
Follow the Blueprints YAML language extension and blueprint schema [installation and configuration instructions](https://github.com/Cloud-Schematics/vscode-blueprint-schema){: external} to install the extension and schema.      


## Create template repository and local clone
{{site.data.keyword.bpshort}} Blueprints works with definitions sourced from a version control system. All input files, templates, and modules are all maintained in online Git source control. 

### Select a template 
Initially you can start by customizing the [sample template](https://github.com/Cloud-Schematics/blueprint-sample-template){: external} or use one of the deployable template examples in the [IBM Cloud Schematics GitHub repository](https://github.com/orgs/Cloud-Schematics/repositories/?q=topic:blueprint){: external}. 

Alternatively work with your own template library in Github or Gitlab. These can be either public or private repos.  

### Clone template repo in Git
Create a copy of the source template in your own Git repository or version control system. These instructions refer to use with Github where both the template and input file are stored in the same repo. Separate repo's can be used to version the template and inputs independently.  

- Navigate to the main page of the source template repository  
- Click **Use this template/repository** 
- Select **Create a new repository** 
- Use the Owner drop-down menu, and select the account where want to create the new repository
- Type a name for your repository
- Click **Create repository from template**

### Clone repo to local machine
Clone your new template repository to your local machine for editing:
- Navigate to the main page of your new repository
- Click **<> Code** (Download Code)
- [Clone the repo](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) to your local machine using: HTTPS, SSH, GitHub CLI or Github Desktop

## Editing in VSCode
- Open the local folder containing the template repo contents as a new VSCode workspace
- Optionally rename the template and input files for your blueprint. 
- Select the blueprint template YAML file for editing
- Just above the top of the file contents, the selected schema is displayed.  
- Select the blueprint input YAML file for editing

With the extension and schema configured, VSCode will provide assisted editing for blueprint templates.

### Blueprint usage and configuration documentation
To customize the template and input file to deploy your own infrastructure, refer to the Blueprint documentation for an overview of [blueprint templates and configuration](https://cloud.ibm.com/docs/schematics?topic=schematics-blueprint-templates&interface=ui). 

### Push edited template 
After editing the template push it back to the Git repo to make it available to be deployed by Schematics. 

Click on the “Source Control” button in the Activity Bar. It’s located on the side of VS Code’s client. Alternatively, you can use the keyboard shortcut “Ctrl+Shift+G” or "Ctrl+Shift+G" on MacOS to open the “Source Control” screen.

- Enter a commit message
- Click **Commit**
- Click **Sync changes**

In this example it is assumed that both the template and input file are in the same Git repo. 


The blueprint template and inputs are now ready for deploying in {{site.data.keyword.bpshort}}. 

## Next steps
The next stage of working with blueprints is [deploying blueprints](/docs/schematics?topic=schematics-deploy-blueprints).