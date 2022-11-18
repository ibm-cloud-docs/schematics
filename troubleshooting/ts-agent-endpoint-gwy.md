---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-18"

keywords: endpoint gateway failed, schematics endpoint gateway error, wrong number of segments in crn

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agents are a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agent](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.

# Why are your getting create endpoint gateway that is failed with wrong number of segments in CRN?
{: #agent-endpoint-error}

When you run an {{site.data.keyword.bplong_notm}} plan or apply action, {{site.data.keyword.bpshort}} Workspaces produces the endpoint gateway that is failed with wrong number of segments in CRN. Following error message is received.
{: tsSymptoms}

```text
2022/05/31 12:19:31 Terraform apply | Error: Create Endpoint Gateway failed wrong number of segments in CRN
 2022/05/31 12:19:31 Terraform apply | {
 2022/05/31 12:19:31 Terraform apply |     "StatusCode": 400,
 2022/05/31 12:19:31 Terraform apply |     "Headers": {
 2022/05/31 12:19:31 Terraform apply |         "Cache-Control": [
 2022/05/31 12:19:31 Terraform apply |             "max-age=0, no-cache, no-store, must-revalidate"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "Cf-Cache-Status": [
 2022/05/31 12:19:31 Terraform apply |             "DYNAMIC"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "Cf-Ray": [
 2022/05/31 12:19:31 Terraform apply |             "713fa8882c4b9f0a-DFW"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "Content-Length": [
 2022/05/31 12:19:31 Terraform apply |             "257"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "Content-Type": [
 2022/05/31 12:19:31 Terraform apply |             "application/json"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "Date": [
 2022/05/31 12:19:31 Terraform apply |             "Tue, 31 May 2022 12:19:31 GMT"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "Expect-Ct": [
 2022/05/31 12:19:31 Terraform apply |             "max-age=604800, report-uri=\"https://report-uri.cloudflare.com/cdn-cgi/beacon/expect-ct\""
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "Expires": [
 2022/05/31 12:19:31 Terraform apply |             "-1"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "Pragma": [
 2022/05/31 12:19:31 Terraform apply |             "no-cache"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "Server": [
 2022/05/31 12:19:31 Terraform apply |             "cloudflare"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "Strict-Transport-Security": [
 2022/05/31 12:19:31 Terraform apply |             "max-age=31536000; includeSubDomains"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "Vary": [
 2022/05/31 12:19:31 Terraform apply |             "Accept-Encoding"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "X-Content-Type-Options": [
 2022/05/31 12:19:31 Terraform apply |             "nosniff"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "X-Request-Id": [
 2022/05/31 12:19:31 Terraform apply |             "ecxxxxe-8900-4e46-a0f5-aec827c4369e"
 2022/05/31 12:19:31 Terraform apply |         ],
 2022/05/31 12:19:31 Terraform apply |         "X-Xss-Protection": [
 2022/05/31 12:19:31 Terraform apply |             "1; mode=block"
 2022/05/31 12:19:31 Terraform apply |         ]
 2022/05/31 12:19:31 Terraform apply |     }
```
{: screen}

When {{site.data.keyword.bpshort}} runs your script or template on the target resource during the execution, the {{site.data.keyword.bpshort}} cannot resolve errors that occur in user-provided scripts, the apply action is marked as failed.
{: tsCauses}

Follow the steps to troubleshoot the error in your {{site.data.keyword.bpshort}} Workspaces:
{: tsResolve}

- From the **`{{site.data.keyword.bpshort}} Workspaces settings`**, select the {{site.data.keyword.bpshort}} apply action that failed.
- Click **`Jobs`** to see the detailed log output.
- In the log file, find the last action that {{site.data.keyword.bpshort}} started before the error occurs. 
- From the {{site.data.keyword.bpshort}} Workspaces settings page, and edit the `create_endpoint_gateway` default value from `true` to `false`. By setting, `true` you create a VPE endpoint gateway for {{site.data.keyword.bpshort}} and set `false` if no endpoint gateway is needed.
