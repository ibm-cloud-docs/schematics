---

copyright:
  years: 2023
lastupdated: "2023-01-05"

keywords: context-based restrictions, access allowlist, network security

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Protecting {{site.data.keyword.bpshort}} services with context-based restrictions
{: #access-control-cbr}

Context-based restrictions give account owners and administrators the ability to define and enforce access restrictions for {{site.data.keyword.cloud}} resources based on the context of access requests. Access to {{site.data.keyword.bpshort}} services can be controlled with context-based restrictions and Identity and Access Management (IAM) policies. You can manage access by using [context-based restrictions (CBR)](https://cloud.ibm.com/context-based-restrictions/overview){: external}. 
{: shortdesc}

## Managing CBR settings
{: #manage-cbr-settings}

With [context-based restrictions](/docs/account?topic=account-context-restrictions-whatis), you can define and enforce user and service access restrictions to {{site.data.keyword.bpshort}} resources based on specified criteria.
{: shortdesc}

To restrict access, you must be the account owner or have an access policy with the administrator role on all account management services.
{: note}

## Overview
{: #cbr-overview}

To restrict access, you must create [zones](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create) and [rules](/docs/account?topic=account-context-restrictions-create&interface=ui#context-restrictions-create-rules). 
{: shortdesc}

First, create a zone with the appropriate details for network or resource definitions. Then, attach that zone to the specified resource to restrict access. You can create zones and rules by using a RESTful [API](/apidocs/context-based-restrictions#introduction) or with [context-based restrictions](https://cloud.ibm.com/context-based-restrictions/overview){: external}. After you create or update a zone or a rule, it might take a few minutes for the change to take effect.

You need to allowlist {{site.data.keyword.bpshort}} in other service CBR rules if you need Schematics to connect to respective services. For example, if you configure a CBR rule for {{site.data.keyword.containerlong_notm}}, then {{site.data.keyword.bpshort}} cannot access a cluster in your account unless you include {{site.data.keyword.bpshort}} in the allowlist.

CBR rules do not apply to provision or deprovision processes. Currently, CBR is supported by {{site.data.keyword.bpshort}} public endpoints in both `US` and `EU` regions. CBR support with {{site.data.keyword.bpshort}} private endpoints is limited to `EU` region only.
{: note}

If a CBR rule is blocking access, then the {{site.data.keyword.bpshort}} workspace page cannot list any workspaces. For other operations such as workspace create, plan, and apply requests that are blocked by CBR can surface as access denied errors.
{: important}

## Understanding network zones
{: #cbr-network-zones}

By creating network zones, you can define an allowlist of network locations where access requests originate to determine when a rule can be applied. The list of network locations can be specified by using IP addresses, such as individual addresses, ranges or subnets, and Virtual Private Cloud (VPC) IDs.
{: shortdesc}

After you create a network zone, you can add it to a rule.

### Creating network zones by using the CBR API
{: #cbr-create-zones-api}
{: api}

The API supports defining [network zones](/apidocs/context-based-restrictions#introduction) by connecting to public and private endpoints.
{: shortdesc}

Use `GET /v1/zones` to list the zones. By using `POST /v1/zones`, you can create a new zone with the appropriate information. For more information about a request body example, see [Creating network zones by using the API](/docs/account?topic=account-context-restrictions-create&interface=api#network-zones-create-api).

You can determine which services are available by checking for [reference targets](/apidocs/context-based-restrictions#list-available-serviceref-targets).
{: note}

After you create zones, you can [update](/apidocs/context-based-restrictions#replace-zone) or [delete](/docs/account?topic=account-context-restrictions-remove&interface=ui) them.

### Creating network zones by using the CBR UI
{: #cbr-create-zone-ui}
{: ui}

After you set the prerequisites and requirements, you can create zones in the UI. For more information about the steps to follow, see [Creating context-based restrictions](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create).
{: shortdesc}

Instead of creating a zone by using UI inputs, you can use the **JSON** code form to create a zone. {: note}

After you create zones, you can also [update](/apidocs/context-based-restrictions#replace-zone) and [delete](/docs/account?topic=account-context-restrictions-remove&interface=ui) them.

## Understanding network rules
{: #cbr-network-rules}

After you create your zones, you can attach the zones to your network resources by creating rules. When you add resources to a rule, you can choose from the available [types of endpoints](/docs/account?topic=account-context-restrictions-whatis#context-restrictions-endpint-type) that are specific to your network topology. 
{: shortdesc}


### Create network rules by using the CBR API
{: #cbr-create-rules-api}
{: api}

You can define network rules with the API by using the information that you collected from creating network zones.
{: shortdesc}

By using `GET /v1/rules` with the endpoints that you chose, you can view a list of current rules. Use `POST /v1/rules` to create new rules. For more information about a request body example, see [Creating rules by using the API](/docs/account?topic=account-context-restrictions-create&interface=api#context-restrictions-create-rules-api).

After you create rules, you can [update](/apidocs/context-based-restrictions#replace-rule) and [delete](/apidocs/context-based-restrictions#delete-rule) them.

### Creating network rules by using the CBR UI
{: #cbr-create-rules-ui}
{: ui}

After you set the prerequisites and requirements, you can create zones in the UI. For more information about the steps to follow, see [Creating context-based restrictions](/docs/account?topic=account-context-restrictions-create&interface=ui#network-zones-create).
{: shortdesc}

You can use the CBR UI to [add resources and contexts](/docs/account?topic=account-context-restrictions-create&interface=ui#context-restrictions-create-rules) to your network rules. 

Unlike IAM policies, context-based restrictions don't assign access. Context-based restrictions check that an access request comes from an allowed context that you configure. Also, the rules might not take effect immediately due to synchronization and resource availability.
{: important}

After you create rules, you can [update](/apidocs/context-based-restrictions#replace-rule) and [delete](/apidocs/context-based-restrictions#delete-rule) them.

## Next steps
{: #cbr-next-steps}

You must follow the creation or modification of zones or rules with adequate testing to ensure access and availability.
