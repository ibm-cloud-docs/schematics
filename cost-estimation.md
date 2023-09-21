---

copyright:
  years: 2017, 2023
lastupdated: "2023-09-21"

keywords: schematics cost, schematics cost estimation, cost estimation, cost, cost-estimation

subcollection: schematics

---


{{site.data.keyword.attribute-definition-list}}

# Estimating infrastructure costs
{: #cost-estimation}

Cost estimation is available for Schematics templates when you perform a `Generate Plan` operation. This estimate is meant to be a starting point to help you determine how much your account could be charged for deploying {{site.data.keyword.cloud_notm}} resources and services. This estimated amount is subject to change as the template is customized, and it does not include all resources, usage, licenses, fees, discounts, or taxes. 
{: shortdesc}

Review the list of [supported and unsupported resources](https://github.com/IBM-Cloud/terraform-cost-estimator/blob/main/supportedResources.md#common-asumptions-taken){: external} for cost estimation.   

## Viewing the cost for your infrastructure
{: #cost-deploy}

After you add the template to your workspace, you can configure the input values. By doing so, you can tailor the template to match your needs. Adjusting the inputs may adjust the estimated cost.

1. Go to the **Workspaces** page, and select your workspace.
2. Go to **Settings** page.
3. Enter the input values and save to configure the template. For more information, see [Running a workspace plan](/docs/schematics?topic=schematics-sch-plan-wk).
4. Click on `Generate Plan` to perform the plan operation and a new cost estimate is computed. This might take a few minutes. 
5. After the plan is complete, you can view the estimated cost for the template on the **Jobs** page using the **Cost estimate** button.
6. If the cost is acceptable, proceed to perform `Apply Plan`. 

This estimated amount is subject to change as the workspace is customized and deployed, and it does not include all resources, usage, licenses, fees, discounts, or taxes.
{: important}

## Accepting the estimated cost 
{: #cost-accept}

The cost estimate is presented as advisory information after execution of the generate plan. No action is required to accept the cost. If the presented cost is not as expected, you must review the workspace configuration and input values. Revise the config and values and regenerate the plan until the cost is acceptable.   


Performing an `Apply Plan` operation is taken as acceptance of the estimated cost.  
{: important}