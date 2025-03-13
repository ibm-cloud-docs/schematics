---

copyright:
  years: 2017, 2025
lastupdated: "2025-03-13"

keywords: schematics cost, schematics cost estimation, cost estimation, cost, cost-estimation

subcollection: schematics

---


{{site.data.keyword.attribute-definition-list}}

# Estimating infrastructure costs
{: #cost-estimation}

Cost estimation is available for the {{site.data.keyword.bpshort}} templates in `Generate Plan` operation. This estimate is meant to be a starting point to help you determine how much your account might be charged for deploying Cloud resources and services. This estimated amount is subject to change as the template is customized, and it does not include all resources, usage, licenses, fees, discounts, or taxes.
{: shortdesc}

Review the list of [supported and unsupported resources](https://github.com/IBM-Cloud/terraform-cost-estimator/blob/main/supportedResources.md#list-of-resources-supported){: external} for cost estimation.

## Viewing the infrastructure costs
{: #cost-deploy}

After you add the template to your workspace, you can configure the input values. By doing so, you can tailor the template to match your needs. Adjusting the inputs might adjust the estimated cost.
{: shortdesc}

1. In the {{site.data.keyword.cloud_notm}} console, click the **Menu** icon ![hamburger icon](images/icon_hamburger.svg) > **Platform Automation** > **Schematics** > **Terraform**.
2. Select your workspace.
3. Go to the workspace **Settings** page.
4. Enter the input values and save to configure the template. For more information, see [running a workspace plan](/docs/schematics?topic=schematics-sch-plan-wks&interface=ui).
5. Click `Generate Plan` to run the planned operation and a new cost estimate is computed. Generation might take `1` or `2` minutes, based on your template infrastructure.
6. You can view the estimated cost for the template on the **Jobs** page by using the **Cost estimate**.
7. Review the cost, if acceptable, proceed to run `Apply Plan`.

This estimated amount is subject to change as the workspace is customized and deployed, and it does not include all resources, usage, licenses, fees, discounts, or taxes.
{: important}

## Accepting the estimated cost 
{: #cost-accept}

The cost estimate is presented as advisory information after execution of the `generate plan`. No action is required to accept the cost. If the presented cost is not as expected, you must review the workspace configuration and input values. Regenerate the cost estimator and proceed when you are ready.

Running an `Apply Plan` operation is considered as an acceptance of the estimated cost.
{: note}
