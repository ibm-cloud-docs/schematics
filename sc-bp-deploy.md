---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-07"

keywords: schematics blueprints, deploy blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Deploying blueprints
{: #deploy-blueprints}

Deploying a blueprint environment by using a blueprint template and input values is a two-step operation in {{site.data.keyword.bpshort}}. The two-step process ensures controlled application of change first to the blueprint configuration and templates, then second to the cloud resources. In a future release a plan step will be added to allow preview of all resource changes before deploying.  
{: shortdesc} 
- `Create the blueprint configuration in {{site.data.keyword.bpshort}}:` This first step creates and saves a blueprint configuration in {{site.data.keyword.bpshort}}. The configuration specifies the blueprint template used to create the reference architecture and the inputs that will we used. {{site.data.keyword.bpshort}} retrieves the user specified blueprint template from its Git repo, input values and performs validation. The automation modules defined in the template are imported from their source repositories. The required {{site.data.keyword.bpshort}} linked modules are initialized to manage deployment of the IaC modules and creation of cloud resources in the next step.
- `Apply the blueprint configuration:` {{site.data.keyword.bpshort}} runs the IaC module automation code in dependency order to create the environment and cloud resources. This step runs a Terraform Apply operation for each module to create the resources as specified by the blueprint template.      

The deployment steps are illustrated in the diagram.

![blueprint deployment](/images/new/sc-bp-deploy.svg){: caption="blueprint deployment" caption-side="bottom"}

1. Prepare the configuration for the blueprint. The configuration specifies the source of the blueprint template in Git, the input files to customize the environment instance and version information.  
2. Create and save the blueprint configuration in {{site.data.keyword.bpshort}}. For more information, see the section [Creating a blueprint config](/docs/schematics?topic=schematics-create-blueprint-config).
    - This step retrieves the YAML template and input file from Git. 
3. After successful validation, the blueprint configuration moves to a `Inactive` state.  
4. {{site.data.keyword.bpshort}} automatically initializes its dependencies. The set of blueprint automation modules defined by the template is initialized to manage the cloud resources of the environment. Each module in the UI or CLI represents an automation module in the blueprint template. 
    - The saved blueprint configuration can be reviewed before environment and resource deployment. 
    - This step is performed internally. In a future release the resulting plan results will be presented for review.    
5. Deploy the blueprint with the `blueprint apply` command or UI Run Apply operation. For more information, see the section [Apply blueprint](/docs/schematics?topic=schematics-apply-blueprint). 
    - Based on your blueprint configuration, {{site.data.keyword.bpshort}} creates an internal deployment plan and runs the IaC module code in dependency order to create the environment and resources. In a future release the deployment plan will be presented for review in a separate step before it can be applied. 
6. For each module, {{site.data.keyword.bpshort}} runs a Terraform Apply to create cloud resources. 
7. After all the modules are applied, {{site.data.keyword.bpshort}} returns the blueprint outputs. 
    - The outputs can be used as input to further configuration steps or used directly to access resources in the environment.   

## Next steps
{: #deploy-nextsteps}

The next stage of working with blueprint is [Maintaining blueprint environments](/docs/schematics?topic=schematics-update-op-blueprints).

Explore deploying [Schematics blueprints by using the command-line](/docs/schematics?topic=schematics-deploy-schematics-blueprint-cli) tutorial to create cloud resources with a blueprint.
