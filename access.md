---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-26"

keywords: schematics, automation, terraform

subcollection: schematics

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:preview: .preview}
{:external: target="_blank" .external}

# Managing user access
{: #access}

Schematics is designed to be used by teams. Within the team is one *service owner* and many *service users*. Each have their own service instance, but their capabilities are defined by whether they are a service owner or user, what kind of account they have, and what their user role is designated as within IAM.
{: shortdesc}

A service owner must have a paid {{site.data.keyword.cloud_notm}} account. Charges are incurred by creating resources, which can be done only in the service owner account, not in the accounts of the users. Service owners can allow access to their resources to other users. When given permission, service users can provision existing resources whether they have a paid or free {{site.data.keyword.cloud_notm}} account.


## User roles
{: #access-roles}

Service owners can assign the following roles to other users.
{: shortdesc}

<table summary="The table shows user permissions by access role. Rows are to be read from the left to right, with the access role in column one, and the permission descriptions in column two.">
<caption>User permissions by service user type, account type, and access role</caption>
  <thead>
  <th>Service user type</th>
  <th>{{site.data.keyword.cloud_notm}} account type</th>
  <th>Access role</th>
  <th>Permissions</th>
  </thead>
  <tbody>
    <tr>
      <td>Service owner</td>
      <td>Paid</td>
      <td>Any</td>
      <td><ul>
          <li>Create workspace</li>
          <li>Update workspace</li>
          <li>Delete workspace</li>
          <li>View workspace</li>
          </ul></td>
    </tr>
    <tr>
      <td>Service user</td>
      <td>Paid</td>
      <td>Manager</td>
      <td><ul>
          <li>Create workspace</li>
          <li>Update workspace</li>
          <li>Delete workspace</li>
          <li>View workspace</li>
          </ul></td>
    </tr>
     <tr>
      <td>Service user</td>
      <td>Free</td>
      <td>Manager</td>
      <td><ul>
          <li>Update workspace</li>
          <li>View workspace</li>
          </ul></td>
    </tr>
    <tr>
      <td>Service user</td>
      <td>Paid or free</td>
      <td>Writer</td>
      <td><ul>
          <li>Update workspace</li>
          <li>View workspace</li>
          </ul></td>
    </tr>
    <tr>
      <td>Service user</td>
      <td>Paid or free</td>
      <td>Reader</td>
      <td><ul>
          <li>View workspace</li>
          </ul></td>
    </tr>
  </tbody>
  </table>


## Setting up access to your service
{: #access-setup}

Service owners must set up an access group and invite users to the service and the group.
{: shortdesc}

1. [Invite users to the IBM Cloud account](/docs/iam?topic=iam-iamuserinv).

2. [Create a resource group](/docs/resources?topic=resources-rgs#create_rgs).

3. [Create an access group and assign policies for the Schematics service](/docs/iam?topic=iam-groups).

    a. Click **Manage** > **Access (IAM)** > **Access groups**.

    b. Click **Access policies**.

    c. Click **Assign access**.

    d. Click **Assign access to resources**.

    e. Select **Schematics**.

    f. In the **Service access** section, select **Manager**, **Writer**,and **Reader**, or the combination of Schematics roles to give your access group.

4. [For the resource group that you created, assign access to resources within the service owner's account.](/docs/iam?topic=iam-groups).

    a. In the **Access polcies** tab, click **Assign access** again.

    b. Click **Assign access** within a resource group.

    c. Select the resource group that you created.

    d. In **Assign access to a resource group**, select an access role.

    e. For the **Service**, select **Schematics**.

    d. In the **Service access** section, select the Schematics roles to map to the access role.

    e. Click **Assign**.

5. [Invite users to the access group](/docs/iam?topic=iam-groups#create_ag).

Next, you can [create Terraform configuration files](/docs/schematics?topic=schematics-create-tf-config) and [create a workspace](/docs/schematics?topic=schematics-workspace-setup). Then, depending on the permissions that are assigned, the user that you added can start working with [Terraform resources](https://www.terraform.io/docs/index.html){: external}.