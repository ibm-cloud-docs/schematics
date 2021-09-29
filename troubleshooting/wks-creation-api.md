---

copyright:
  years: 2017, 2021
lastupdated: "2021-09-29"

keywords: schematics, schematics workspace create, schematics workspace create

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}



# Why do {{site.data.keyword.bpshort}} workspace create through API fails?
{: #wks-create-api}

When you attempt to create the {{site.data.keyword.bpshort}} workspace through API by using following CURL command.
{: tsSymptoms}

```
curl --request POST --url https://schematics.cloud.ibm.com/v1/workspaces -H "Authorization: Bearer scfQ" -d '{"name":"test_api","type": ["terraform_v0.12"],"location": "eu-de","description": "via api","resource_group": "5e1f06f5b2b24a319f6cd5be86f531dd","tags": [],"template_repo": {"url": "https://github.ibm.com/Rise-with-SAP/iac-hec-sap"},"template_data": [{"folder": ".","type": "terraform_v0.12","variablestore": []}]}'
```
{: screen}

Following error is displayed when you execute the {{site.dta.keyword.bpshort}} workspace create API.
{: tsCauses}

```
{
"requestid": "3b57ed5d-8610-4a86-9864-8d8197b80336",
"timestamp": "2021-09-22T14:49:04.565693526Z",
"messageid": "M1008",
"message": "Bad request. Check that the information you entered in the payload is complete and formatted correctly in JSON.",
"statuscode": 400
}
```
{: screen}


Verify the payload `location` and the `URL endpoint` used to create the {{site.data.keyword.bpshort}} workspace are identical.
{: tsResolve}

**For example**

- For creating workspace in US: Use  **location** as **us-east** or **us-south** in the payload with the URL as `https://schematics.cloud.ibm.com/v1/workspaces`
- For workspace in the EU: Use **location** as **eu-de** or **eu-gb** in the payload with the URL as `https://eu.schematics.cloud.ibm.com/v1/workspaces`
