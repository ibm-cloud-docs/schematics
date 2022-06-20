---

copyright:
  years: 2017, 2022
lastupdated: "2022-06-20"

keywords: blueprint validate failure, blueprint validate error, validate fails 

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

The {{site.data.keyword.bpshort}} Agent feature is currently in beta and should not be used for production workloads.
{: beta}

# Why do Blueprint create fails in the Blueprint validate step?
{: #bp-validate-fails}

When you create a Blueprint in {{site.data.keyword.bpshort}}, the create fails before the Blueprint is created with an error that the Blueprint or input files contain invalid definitions.  
{: tsSymptoms}

Prior to creating the Blueprint, {{site.data.keyword.bpshort}} validates the syntax of the YAML input files. 
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
 



