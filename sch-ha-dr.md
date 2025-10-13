---

copyright:
  years: 2025
lastupdated: "2025-10-13"

keywords: HA for Schematics, DR for Schematics, Schematics recovery time objective, Schematics recovery point objective

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Understanding high availability and disaster recovery for {{site.data.keyword.bpshort}}
{: #ha-dr}

[High availability](#x2284708){: term} (HA) is the ability for a service to remain operational and accessible in the presence of unexpected failures. [Disaster recovery](#x2113280){: term} is the process of recovering the service instance to a working state.
{: shortdesc}

{{site.data.keyword.bplong}} is a highly available regional service that fulfills the defined [Service Level Objectives (SLO)](/docs/resiliency?topic=resiliency-slo) with the Standard plan. For more information about the available {{site.data.keyword.cloud_notm}} regions and data centers for Secrets Manager, see [Service and infrastructure availability by location](/docs/overview?topic=overview-services_region).

## High availability architecture
{: #ha-architecture}

{{site.data.keyword.bpshort}} is deployed as two highly available service instances in two separate geographical locations, the `US` and `EU`. Within each region, the service is deployed across two multizone regions, such as `us-south` and `us-east` in the US region, `eu-de` and `eu-gb` for the `EU` region, `ca-tor` in the Toronto region, and `ca-mon` in the Montreal region. This setup help ensure that the service is still available, even if one region fails. Data is not shared across geographical locations.

{{site.data.keyword.bpshort}} instances are highly available with no configuration required.

### High availability features
{: #ha-features}

{{site.data.keyword.bpshort}} supports the following high availability features:

| **Feature** | **Description** | **Consideration** |
|------------|-----------------|-------------------|
| Multi-zone deployment | {{site.data.keyword.bpshort}} distributes workloads across multiple availability zones within a region to help ensure resilience. | Help ensure that your resources are provisioned in supported multi-zone regions for optimal availability. |
| Remote Terraform state storage | Stores Terraform state files remotely to prevent data loss and enable recovery. | Use {{site.data.keyword.cos_full}} for secure and durable state management. |
| State locking | Prevents concurrent modifications to infrastructure by locking the Terraform state during operations. | Avoid manual state changes to maintain consistency and prevent conflicts. |
| Automated recovery | Automatically retries failed operations and restores services if transient errors. | Monitor retry logs and configure alerting for the persistent failures. |
| Logging and Monitoring | Provides detailed logs and metrics for all {{site.data.keyword.bpshort}} activities to support proactive issue resolution. | Integrate with {{site.data.keyword.monitoringlong_notm}} and {{site.data.keyword.loganalysislong_notm}} for centralized observability. |
{: caption="HA features for Scheamtics" caption-side="bottom"}

## Disaster recovery architecture
{: #disaster-recovery-intro}

{{site.data.keyword.bpshort}} is designed with resilience and Disaster Recovery (DR) to help ensure that infrastructure processes are reliable and recoverable. {{site.data.keyword.bpshort}} uses {{site.data.keyword.cloud_notm}}’s robust architecture to minimize downtime and data loss during unexpected failures.

### Disaster recovery features
{: #dr-features}

{{site.data.keyword.bpshort}} supports the following high availability features:

| **Feature** | **Description** | **Consideration** |
|------------|-----------------|-------------------|
| Remote state storage | Terraform state files are stored in {{site.data.keyword.cos_full}} to help durability and recoverability. | Configure Object Storage with right access policies and encryption settings. |
| Multi-Zone execution | {{site.data.keyword.bpshort}} runs across multiple zones within a region to maintain service continuity. | Choose regions with multi-zone support and validate resource availability. |
| Automated retry mechanism | {{site.data.keyword.bpshort}} automatically retries failed operations due to transient issues. | Monitor retry logs and set alerts for persistent failures. |
| Logging and Monitoring | Detailed logs and metrics help identify and resolve issues quickly during disaster scenarios. | Integrate with {{site.data.keyword.logs_full_notm}} for centralized visibility. |
{: caption="DR features for {{site.data.keyword.bpshort}}" caption-side="bottom"}

As a customer, you can create and support the following other disaster recovery options:

| Feature | Description | Consideration |
| -------------- | -------------- | -------------- |
| Backup and restore |   A service instance by using customer written script. | Customer must create the script and persist the backup copy where it can be used during recovery. |
{: caption="Customer DR features for {{site.data.keyword.bpshort}}" caption-side="bottom"}

### Planning for DR
{: #features-for-disaster-recovery}

The DR steps must be practiced regularly. As you build your plan, consider the following failure scenarios and resolutions.

| Failure | Resolution |
| -------------- | -------------- |
| Hardware failure (single point) | (Example) {{site.data.keyword.IBM_notm}} provides a database that is resilient from single point of hardware failure within a zone. No customer configuration required. |
| Zone failure | IBM provides an instance that is resilient from a zone failure - no configuration required.|
| Data corruption | Restore a point in time uncorrupted version from the external source of truth or backup and restore. |
| Regional failure | Switch critical workloads to use the restored version in a recovery region. Restore the instance by using external source of truth, backup and restore. |
{: caption="DR scenarios for {{site.data.keyword.bpshort}}" caption-side="bottom"}

## Backup and restore customer-provided feature
{: #feature-backup-restore-feature}

You need to follow these steps to manually back up your workspace in {{site.data.keyword.bpshort}}. To avoid that a successfully provisioned resource is deleted and re-created, you must `untaint` the resource.

1. List the workspaces in your account and note the ID of the workspace that includes the failed resource.

    ```sh
    ibmcloud schematics workspace list
    ```
    {: pre}

2. Retrieve the template ID of your workspace. To template ID is shown as a string after the `Template Variables for: <template_ID>` section of your CLI output.

    ```sh
    ibmcloud schematics workspace get --id <workspace_ID>
    ```
    {: pre}

3. Retrieve the [Terraform state file](/docs/schematics?topic=schematics-schematics-cli-reference#state-list) for your workspace and note the name of the resource that is tainted.

    ```sh
    ibmcloud schematics state pull --id <workspace_ID> --template <template_ID>
    ```
    {: pre}

If the workspace details are extracted and you want to restore the workspace with terraform state file, use [Workspace Create](/docs/schematics?topic=schematics-schematics-cli-reference#schematics-workspace-new) command. The restore creates a workspace in your account for your reference.

### Live syrnchorization
{: #features-live-sync-feature}

You can write a script to download the workspaces and import to backup instances or your {{site.data.keyword.cos_full}} bucket.

### Planning for disaster recovery
{: #features-for-disaster-recovery-feature}

The disaster recovery steps must be practiced regularly. As you build your plan, consider the following failure scenarios and resolutions.

| Failure | Resolution |
| -------------- | -------------- |
| Hardware failure (single point) | IBM provides an instance that is resilient from single point of hardware failure within a zone - no configuration required. |
| Zone failure | IBM provides an instance that is resilient from a zone failure - no configuration required. |
| Data corruption | Use rotation to restore the previous secret version in an available service instance. |
| Data corruption | Restore a point in time uncorrupted version from the backup and restore. |
| Regional failure | Switch critical workloads to use the restored version in a recovery region. Restore the instance by using backup and restore, or live synchronization. |
{: caption="DR scenarios for {{site.data.keyword.bp_short}}" caption-side="bottom"}

## Your responsibilities for HA and DR
{: #feature-responsibilities}

The following information can help you create and continuously practice your plan for HA and DR.
{: shortdesc}

Interruptions in network connectivity and short periods of unavailability of a service might occur. It is your responsibility to make sure that application source code includes [client availability retry logic](/docs/resiliency?topic=resiliency-high-availability-design#client-retry-logic-for-ha) to maintain high availability of the application.
{: note}

For more information about responsibility ownership between you and {{site.data.keyword.cloud_notm}} for {{site.data.keyword.bpshort}}, see [Your responsibilities in using {{site.data.keyword.bpshort}}](/docs/schematics?topic=schematics-sc-responsibilities&interface=ui).

198779

## Recovery time objective (RTO) and recovery point objective (RPO)
{: #rto-rpo-features}

If you accidentally deleted the root key, open a support case for the respective service, and include the following information:

- Your service instance's CRN
- Your backup Key Protect or HPCS instance's CRN
- The new Key Protect or HPCS root key ID
- The original Key Protect or HPCS instance's CRN and key ID, if available

See recovering from an accidental key loss for authorization in the Key Protect and [HPCS](/docs/hs-crypto?topic=hs-crypto-restore-keys&interface=ui) docs.

## Change management
{: #change-management}

Change management includes tasks such as upgrades, configuration changes, and deletion.

Grant users and processes the IAM roles and actions with the least privilege that is required for their work. For more information, see [How can I prevent accidental deletion of services](/docs/resiliency?topic=resiliency-dr-faq#prevent-accidental-deletion)?
{: tip}

Consider creating a manual backup before you upgrade to a new version of {{site.data.keyword.bpshort}}.

## How {{site.data.keyword.IBM_notm}} supports disaster recovery planning
{: #ibm-disaster-recovery}

{{site.data.keyword.IBM}} takes specific recovery actions for {{site.data.keyword.bpshort}} if a disaster occurs.

### How {{site.data.keyword.IBM_notm}} recovers from zone failures
{: #ibm-zone-failure}


If a zone failure occurs, {{site.data.keyword.cloud_notm}} automatically resolves the outage when the zone comes back online. At that point, the global load balancer resumes sending API requests to the restored instance node without any customer action required.
{: shortdesc}

A complete failure of any one of the dependent services across all zones in a Multi-Zone Region (MZR) triggers the initiation of the {{site.data.keyword.bpshort}} IT DR plan. This plan facilitates failover to an alternative region or MZR to maintain service continuity. Failover can occur in either direction, transferring all workloads from the failed region to the operating region. The recovery process varies depending on whether the failover is from `Master to Slave`, or `Slave to Master`.

### How {{site.data.keyword.IBM_notm}} recovers from regional failures
{: #ibm-regional-failure}

When a region is restored after a failure, IBM attempts to restore the service instance from the regional state that results in no loss of data. The service instance that is restored with the same connection strings.

- `RTO=few minutes`
- `RPO=0 minutes`

If regional state is corrupted, the service restores to the state of the last internal backup. All data that is associated with the service is backed up daily by the service in a cross-region Cloud Object Storage bucket that is managed by the service. A potential for 24 hours of data loss might exist. These backups are not available for customer-managed disaster recovery. When a service is recovered from backups, the instance ID restores so that the clients need not update with new connection strings.

- Maximum Acceptable Outage (MAO): Less than 24 hours
- Recovery Time Objective (RTO): Less than 24 hours
- Recovery Point Objective (RPO): Less than 24 hours in practice recovery time for region failure without data corruption is approximately 1 hour, with a few seconds of potential data loss. Recovery from data correction is by DB restore and takes up to 6 to 8 hours, with up to 24 hours data loss.

When IBM can’t restore the service instance, the customer must restore as described in the disaster recovery section.

## How {{site.data.keyword.IBM_notm}} maintains services
{: #ibm-service-maintenance}

All upgrades follow the {{site.data.keyword.IBM_notm}} service and have a recovery plan and rollback process in-place. Regular upgrades for new features and maintenance occur as part of normal operations. Such maintenance can occasionally cause short interruption intervals that are handled by [client availability retry logic](/docs/resiliency?topic=resiliency-high-availability-design#client-retry-logic-for-ha). Changes are rolled out sequentially, region by region and zone by zone within a region. Updates are backed out at the first sign of a defect.

Complex changes are enabled and disabled with feature flags to control exposure.

Changes that impact customer workloads are detailed in notifications. For more information, see [monitoring notifications and status](/docs/account?topic=account-viewing-cloud-status) for planned maintenance, announcements, and release notes that impact this service.
