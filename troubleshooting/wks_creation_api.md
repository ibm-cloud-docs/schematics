---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-23"

keywords: schematics, schematics workspace create, schematics workspace create

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}


# Why do {{site.data.keyword.bpshort}} Workspaces create using the API/UI/CLI fails?
{: #wks-create-api}

The {{site.data.keyword.bpshort}} create workspace fails when you attempt to create using the API by using following CURL command.
{: tsSymptoms}

```sh
curl --request POST --url https://cloud.ibm.com/schematics/workspaces -H "Authorization: Bearer scfQ" -d '{"name":"test_api","type": ["terraform_v0.12"],"location": "eu-de","description": "via api","resource_group": "5e1f06f5b2b24a319f6cd5be86f531dd","tags": [],"template_repo": {"url": "https://github.ibm.com/Rise-with-SAP/iac-hec-sap"},"template_data": [{"folder": ".","type": "terraform_v0.12","variablestore": []}]}'
```
{: screen}


When {{site.data.keyword.bpshort}} runs the CURL command, an error state {{site.data.keyword.bpshort}} cannot find the complete information in the payload. And the workspace create is marked with `Bad request` message. 
{: tsCauses}


```json
{
"requestid": "3b57ed5d-8610-4a86-9864-8d8197b80336",
"timestamp": "2021-09-22T14:49:04.565693526Z",
"messageid": "M1008",
"message": "Bad request. Check that the information you entered in the payload is complete and formatted correctly in JSON.",
"statuscode": 400
}
```
{: screen}


Verify your CURL or the payload contains that the `location` and the `url` are pointing to the same region where you want to create or update the workspace.
{: tsResolve}

**`For example`**

- For creating workspace in `US` region: Use `location` as **`us-east`** or **`us-south`** and `url` as **`https://us-south.schematics.cloud.ibm.com/`** or **`https://us-east.schematics.cloud.ibm.com/`**. By default **`https://cloud.ibm.com/schematics/workspaces`** points to **`https://cloud.ibm.com/schematics/overview`** endpoint.
- For workspace in the `EU` region: Use `location` as **`eu-de`** or **`eu-gb`** and `url` as **`https://eu-de.schematics.cloud.ibm.com`** and **`https://eu-gb.schematics.cloud.ibm.com`** endpoint.
