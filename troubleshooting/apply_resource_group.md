---

copyright:
  years: 2017, 2025
lastupdated: "2025-01-17"

keywords: schematics resource group not found, schematics resource group error, schematics resource group does not exist, schematics resource group doesn't exist 

subcollection: schematics
content-type: troubleshoot

---

{{site.data.keyword.attribute-definition-list}}

# Why can't {{site.data.keyword.bpshort}} find my resource group?
{: #rg-not-found}

When you run an {{site.data.keyword.bplong_notm}} plan or apply action, the resource group that you try to retrieve by using the `ibm_resource_group` data source cannot be found. The following error message is thrown.
{: tsSymptoms}

```text
Error retrieving resource group <resource-group>: ResourceGroupDoesnotExist: Given resource Group : "<resource-group>" doesn't exist

```
{: screen}

You do not have the wanted permissions in {{site.data.keyword.iamshort}} to view the resource group. If you specified an API key in your Terraform template or in {{site.data.keyword.bpshort}}, this API key does not have the wanted permissions in IAM to view the resource group.
{: tsCauses}

Make sure that the **Viewer** permission on the resource group is assigned to you or the API key that you use. For more information, see [Adding resources to a resource group](/docs/account?topic=account-rgs&interface=ui#add_to_rgs).
{: tsResolve}
