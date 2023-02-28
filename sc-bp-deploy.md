---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-28"

keywords: schematics blueprints, deploy blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Deploying blueprints
{: #deploy-blueprints}

Deploying a blueprint environment using a blueprint template and input values is a two-step operation. The two-step process ensures controlled application of change first to the stored blueprint configuration, then secondly to the cloud resources. In a future release a plan step will be added to allow preview of all resource changes before deploying.  
{: shortdesc} 

1. **Create the blueprint configuration in {{site.data.keyword.bpshort}}:** This first step creates and saves a blueprint configuration in {{site.data.keyword.bpshort}}. The configuration specifies the blueprint template used to create the reference architecture and the inputs that will we used. {{site.data.keyword.bpshort}} retrieves the user specified blueprint template from its Git repo, input values and performs validation. The automation modules defined in the template are imported from their source repositories. The required {{site.data.keyword.bpshort}} linked modules are initialized to manage deployment of the IaC modules and creation of cloud resources in the next step.
2. **Apply the blueprint configuration:** {{site.data.keyword.bpshort}} runs the IaC module automation code in dependency order to create the environment and cloud resources. This step runs a Terraform Apply operation for each module to create the resources as specified by the blueprint template.      

The deployment steps are illustrated in the diagram.

![blueprint deployment](/images/new/sc-bp-deploy.svg){: caption="blueprint deployment" caption-side="bottom"}

1. Prepare the configuration for the blueprint. The configuration specifies the source of the blueprint template in Git, the input files to customize the environment instance and version information.  
    - Create and save the blueprint configuration in {{site.data.keyword.bpshort}}. Refer to the section [Creating a blueprint config](/docs/schematics?topic=schematics-create-blueprint-config) for guidance on creating the config via the CLI or UI.  
    - This step retrieves the YAML template and input file from Git. 
2. After the config is successful created and validated, the blueprint will be in an `Incomplete` state.  
3. {{site.data.keyword.bpshort}} automatically initializes its dependencies. The set of blueprint automation modules defined by the template is initialized to manage the cloud resources of the environment. Each module shown in the UI or CLI represents an automation module in the blueprint template.  
   - The saved blueprint configuration can be reviewed before environment and resource deployment. 
   - This step is performed internally. In a future release the resulting plan results will be presented for review.    
4. Deploy the blueprint with the `blueprint apply` command or UI apply operation. Refer to the section [Apply changed to a blueprint environment](/docs/schematics?topic=schematics-apply-blueprint) for guidance applying the config via the CLI or UI.  . 
    - Based on your blueprint configuration, {{site.data.keyword.bpshort}} creates an internal deployment plan and runs the IaC module code in dependency order to create the environment and resources. In a future release the deployment plan will be presented for review in a separate step before it can be applied. 
5. For each module, {{site.data.keyword.bpshort}} runs a Terraform Apply to create cloud resources. 
6. After all the modules are applied, {{site.data.keyword.bpshort}} returns the blueprint outputs. 
    - The outputs can be used as input to further configuration steps or used directly to access resources in the environment.   

## Next steps
{: #deploy-nextsteps}

The next stage of working with blueprint is [Maintaining blueprint environments](/docs/schematics?topic=schematics-update-op-blueprints).

Explore deploying [{{site.data.keyword.bpshort}}blueprints by using the command-line](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) tutorial to create cloud resources with a blueprint.
