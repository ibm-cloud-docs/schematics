---

copyright:
  years: 2017, 2022
lastupdated: "2022-07-04"

keywords: blueprint install, install blueprint, blueprint

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [Beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations) for the Beta release.
{: beta}

# Installing a Blueprint
{: #install-blueprint}

Blueprint install is the second step required to create or modify cloud resources when using Blueprints. Install runs the IaC automation code for each Workspace. For each module, {{site.data.keyword.bpshort}} performs a Terraform apply or Ansible playbook run to create or configure the specified cloud resources. 
{: shortdesc}



The following command performs a Blueprint install for the Blueprint with the ID `eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936`

**Syntax:**

```sh
ibmcloud schematics blueprint install -id eu-de.BLUEPRINT.Blueprint-Basic-Example.21735936
```
{: screen}


On successful completion the install command will return **fullfilment_success**. 

## Verfiy Blueprint install success 
{: #bp-verify-install}

Verify that the Blueprint has been installed successfully. When you install the Blueprint from the CLI, the command displays details of the Workspaces being installed and an continuously updating status of the progress of the {{site.data.keyword.bpshort}} jobs executing the IAC automation code. The command only returns on completion.


```text
Modules to be installed
SNO   Type        Name                   Status   
1     Terraform   basic-resource-group   INACTIVE   
2     Terraform   basic-cos-storage      INACTIVE   
      
Blueprint job running eu-gb.JOB.basic.f012ad25

Waiting:0    Draft:0    Connecting:0    InProgress:0    Inactive:0    Active:2    Failed:0   

Type        Name                   Status               Job ID   
Blueprint   basic                  FULFILMENT_SUCCESS   eu-gb.JOB.basic.f012ad25   
Terraform   basic-resource-group   ACTIVE                  
Terraform   basic-cos-storage      ACTIVE                  
            
Blueprint fulfilment_success at Tue May 31 11:44:12 BST 2022
OK
```
{: screen}

On successful completion the install command will return **fullfillment_success**.  

For more information, about how to diagnose and resolve issues if the install fails, refer to the [Troubleshooting section](/docs/schematics?topic=schematics-bp-install-fails).

## Next steps
{: #bp-install-nextsteps}

After installing the Blueprint, the desired cloud resources are now deployed. The resources can be located on the Blueprint Resources tab in the UI. Or via the Console Resource list. 






















4. Verify that the Blueprint Install has run successfully to create Cloud resources . When you run the install command from the CLI, the command displays the status of the running {{site.data.keyword.bpshort}} jobs and the command only returns on completion. 


On successful completion the install command will return **install_success**. 


If the install fails refer to the {HowTo Troubleshooting section]() for more information on how to diagnose and resolve the error.  







5. Show the status of all Blueprints in Schematics.  


```
ibmcloud schematics blueprint list 
```


-- other parameters


Example output 


```




```


6. Retrieve Blueprint outputs




```
ibmcloud schematics blueprint get -id blueprint_id --output
```


--output 






Example output


```