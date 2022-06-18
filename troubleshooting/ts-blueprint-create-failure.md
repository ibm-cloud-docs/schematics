---

copyright:
  years: 2017, 2022
lastupdated: "2022-06-18"

keywords: blueprint create failure, blueprint download error, create fails 

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why do Blueprint create fails in the Blueprint download step?
{: #bp-create-fails}

When you create a Blueprint in {{site.data.keyword.bpshort}}, the create fails before the Blueprint is created with an error that the Blueprint or input files cannot be located or cloned. 
{: tsSymptoms}

Prior to creating the Blueprint, {{site.data.keyword.bpshort}} downloads the input files and Blueprint from the Git repository specified on the create command and validates the YAML schema. 
{: tsCauses}

Sample error

```text
Could not create the blueprint. Please verify that your request is correct. If the problem persists, contact IBM Cloud support.

requestid:        a2ca938c-244e-40a4-9b51-c8fafef2fa07
statuscode:        400
timestamp:        2022-06-14T12:06:42.456463261Z
message:        Invalid blueprint definitions. Error - Failed to clone git repository, repository not found (check url, also check the scope 'repo' of the personal access token if SCHEMATICSGITTOKEN is used)
messageid:        M1156
```
{: screen}

Check that the source repository for the Blueprint and input files are correctly specified, the referenced files and repository exists and accessible with Git tokens specified if necessary.
{: tsResolve} 

Rerun the Blueprint create operation with the correct repository reference.



