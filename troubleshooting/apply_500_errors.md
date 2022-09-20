---

copyright:
  years: 2017, 2022
lastupdated: "2022-09-20"

keywords: schematics 500 errors, schematics 5xx errors, schematics server error

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}


# Why are you getting 5xx HTTP errors?
{: #server-errors}

When you run a {{site.data.keyword.bpshort}} apply action, the action fails with 5xx HTTP errors such as in the following example: 
{: tsSymptoms}

```text
Error: Request failed with status code: 500, ServerErrorResponse: {"incidentID":"11aa11aaaa11-IAD","code":"A0002","description":"Could not connect to a backend service. Try again later.","type":"Authentication"}
```
{: screen}


5xx HTTP errors indicate an issue with the {{site.data.keyword.cloud_notm}} service that you try to create, update, or delete, and usually cannot be resolved by the user. These issues can include networking errors, timeouts, or the service temporarily unavailable.
{: tsCauses}

Because this error does not originate within {{site.data.keyword.bpshort}}, wait a few minutes to rerun the {{site.data.keyword.bpshort}} apply action again. If the apply action continues to fail, note the incident ID and find more detailed logs for this incident ID in your {{site.data.keyword.loganalysislong_notm}} service instance. If you cannot resolve this issue, contact support by opening a support case for the service that you want to work with. Make sure to include the incident ID. For more information, see [Using the Support Center](/docs/get-support?topic=get-support-using-avatar).
{: tsResolve}


