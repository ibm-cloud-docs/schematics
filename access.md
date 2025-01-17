---

copyright: 
  years: 2017, 2021
lastupdated: "2025-01-17"

keywords: access global catalog, catalog visibility, staging environment

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Location based access
{: #access-ibm-cloud-catalog}

## Manage location settings in the catalog
{: #configure-location}

An [account owner or administrator](/docs/account?topic=account-account-services#catalog-management-account-management), can manage the settings for {{site.data.keyword.bpshort}} resource based on the location. The Account owner can restrict the location in the catalog for the invited users in the catalog. If the location is restricted for a user, then user cannot access or create new resources in the {{site.data.keyword.bpshort}} for that particular location.

Follow the steps to configure the {{site.data.keyword.bpshort}} resource based on the location.

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage** > **Catalogs** > **Settings**.
2. Click **Edit** icon for the Location.
3. Set one or more **Filter by Location** to customize which location you need to be restricted.

    {{site.data.keyword.bpshort}} supports only [Locations and service endpoints](/docs/schematics?topic=schematics-locations).
    {: note}

4. Click **Update** in the Filter by Location page.
5. Use the **Preview** table to confirm your selections, and click **Update**.
6. In the settings page, click **Update** to restrict the access to read, create, or update in the location. For more information about global catalog settings, see [Managing catalog settings](/docs/account?topic=account-filter-account).

Account owner can edit the location in {{site.data.keyword.cloud}} catalog settings only after `30 minutes`. As the catalog settings created are valid for `30 minutes`.
{: important}

Example

- When the account owner restricts the `us-east` location for the user in the catalog settings.
- If that user creates a workspace and provisions the resources in the `North America` region, where North America consists `us-east` and `us-south`.
- In this case, the user can view the resources are provisioned only in `us-south` but not in the `us-east` region.
- The reason is that the location visibility is restricted to the user.
