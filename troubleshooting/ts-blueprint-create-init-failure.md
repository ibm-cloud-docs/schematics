---

copyright:
  years: 2017, 2022
lastupdated: "2022-06-18"

keywords: blueprint create init failure, blueprint init error, create init fails 

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do Blueprint create fails in the Blueprint create_init step?
{: #bp-create_init-fails}

When you create a Blueprint in {{site.data.keyword.bpshort}}, the create fails during initialization of the {{site.data.keyword.bpshort}} Workspaces for the modules. 
{: tsSymptoms}

Blueprints are created in two steps. The first step retrieves and validates the Blueprint definition. The second step creates the required module Workspaces based on the module statements in the definition. This step clones the specified module source repos. An incorrectly specified repository URL results in an initialization failure.  
{: tsCauses}

Sample error 

```text
Could not create the blueprint. Please verify that your request is correct. If the problem persists, contact IBM Cloud support.

message:        Invalid blueprint definitions. Error - Blueprint json validation failed - Field `blueprint.workitems` missing or invalid
messageid:        M1156
requestid:        d11875e0-da14-4231-8121-c371b674a37a
statuscode:        400
timestamp:        2022-06-14T12:08:32.810397074Z
```
{: screen}

- Correct the errors as identified in the validation messages of the Blueprint or input files.
{: tsResolve}

- Push the changes to the Blueprint and input files to Git repository.

- Rerun the Blueprint create operation with the corrected syntax. 
 



