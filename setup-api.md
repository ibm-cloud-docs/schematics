---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-02"

keywords: schematics api, schematics command-line, schematics commands, terraform commands, terraform API, setting up schematics api, api

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Setting up the API
{: #setup-api}

You can use the {{site.data.keyword.bplong}} API to automate {{site.data.keyword.bpshort}} capabilities in {{site.data.keyword.cloud_notm}}. To use the CLI, see [Setting up the CLI](/docs/schematics?topic=schematics-setup-cli).
{: shortdesc}

To find an overview of supported {{site.data.keyword.bplong}} `APIs`, API endpoints, and required API header and body information, see the [{{site.data.keyword.bplong}} API documentation](/apidocs/schematics){: external}. 
{: tip}

## Automating deployments with the API
{: #cs_api}

Learn how to use the {{site.data.keyword.cloud_notm}} API to retrieve an Identity and Access Management token so that you can access the {{site.data.keyword.bpshort}} API. 
{: shortdesc}

To authenticate with {{site.data.keyword.bplong_notm}}, you must provide an {{site.data.keyword.iamlong}} (IAM) token that is generated with your {{site.data.keyword.cloud_notm}} credentials. Depending on the way you authenticate with {{site.data.keyword.cloud_notm}}, you can choose between the following options to automate the creation of your {{site.data.keyword.cloud_notm}} IAM token.


|{{site.data.keyword.cloud_notm}} ID|My options|
|-----------------------------------|----------|
|Unfederated ID|:**Generate an {{site.data.keyword.cloud_notm}} API key:** As an alternative to using the {{site.data.keyword.cloud_notm}} username and password, you can [use {{site.data.keyword.cloud_notm}} API keys](/docs/account?topic=account-userapikey#create_user_key){: external}. {{site.data.keyword.cloud_notm}} API keys are dependent on the {{site.data.keyword.cloud_notm}} account they are generated for. You cannot combine your {{site.data.keyword.cloud_notm}} API key with a different account ID in the same {{site.data.keyword.cloud_notm}} IAM token. To access workspaces that were created with an account other than the one your {{site.data.keyword.cloud_notm}} API key is based on, you must log in to the account to generate a new API key.:**{{site.data.keyword.cloud_notm}} username and password:** You can follow the steps in this topic to fully automate the creation of your {{site.data.keyword.cloud_notm}} IAM access token.|
|Federated ID|:**Generate an {{site.data.keyword.cloud_notm}} API key:** [{{site.data.keyword.cloud_notm}} API keys](/docs/account?topic=account-userapikey#create_user_key){: external} are dependent on the {{site.data.keyword.cloud_notm}} account they are generated for. You cannot combine your {{site.data.keyword.cloud_notm}} API key with a different account ID in the same {{site.data.keyword.cloud_notm}} IAM token. To access workspaces that were created with an account other than the one your {{site.data.keyword.cloud_notm}} API key is based on, you must log in to the account to generate a new API key.:**Use a one-time passcode:** If you authenticate with {{site.data.keyword.cloud_notm}} by using a one-time passcode, you cannot fully automate the creation of your {{site.data.keyword.cloud_notm}} IAM token because the retrieval of your one-time passcode requires a manual interaction with your web browser. To fully automate the creation of your {{site.data.keyword.cloud_notm}} IAM token, you must create an {{site.data.keyword.cloud_notm}} API key instead.</ul>|
{: caption="ID types and options" caption-side="top"}

1. Create your {{site.data.keyword.cloud_notm}} IAM access token. The body information that is included in your request varies based on the {{site.data.keyword.cloud_notm}} authentication method that you use. [Review the parameter table](#table1)

   You can find the {{site.data.keyword.cloud_notm}} IAM token in the **`access_token`** field of your API output. Note the {{site.data.keyword.cloud_notm}} IAM token to retrieve additional header information in the next steps.
    ```sh
    POST `https://iam.cloud.ibm.com/identity/token`
    ```
    {: codeblock}

    **Example output**

    ```text
        {
        "access_token": "<iam_access_token>",
        "refresh_token": "<iam_refresh_token>",
        "uaa_token": "<uaa_token>",
        "uaa_refresh_token": "<uaa_refresh_token>",
        "token_type": "Bearer",
        "expires_in": 3600,
        "expiration": 1493747503
        "scope": "ibm openid"
        }
    ```
    {: screen}

2. Retrieve the ID of the {{site.data.keyword.cloud_notm}} account that you want to work with. Replace `<iam_access_token>` with the {{site.data.keyword.cloud_notm}} IAM token that you retrieved from the **`access_token`** field of your API output in the previous step. In your API output, you can find the ID of your {{site.data.keyword.cloud_notm}} account in the **resources.metadata.guid** field.
    ```sh
    GET https://accounts.cloud.ibm.com/coe/v2/accounts
    ```
    {: codeblock}

    | Input parameters | Values |
    | ---- |  ---- |
    | Headers | :`Content-Type: application/json`:`Authorization: bearer <iam_access_token>`:`Accept: application/json` |
    {: caption="Input parameters to get an {{site.data.keyword.cloud_notm}} account ID." caption-side="top"}

    **Example output**

    ```text
    {
        "next_url": null,
        "total_results": 5,
        "resources": [
            {
                "metadata": {
                    "guid": "<account_ID>",
                    "url": "/coe/v2/accounts/<account_ID>",
                    "created_at": "2020-09-29T02:49:41.842Z",
                    "updated_at": "2020-08-16T18:56:00.442Z",
                    "anonymousId": "1111a1aa1a1111a1aa11aa11111a1111"
                },
                "entity": {
                    "name": "<account_name>",
                }
            }
          ]
    }
    ```
    {: screen}

3. Generate a new {{site.data.keyword.cloud_notm}} IAM token that includes your {{site.data.keyword.cloud_notm}} credentials and the account ID that you want to work with.

    If you use an {{site.data.keyword.cloud_notm}} API key, you must use the {{site.data.keyword.cloud_notm}} account ID the API key was created for. To access {{site.data.keyword.bpshort}} Workspaces or actions in account `B`, log in to account `B` and create an {{site.data.keyword.cloud_notm}} API key that is based on account `B`.
    {: note}

    ```sh
    POST https://iam.cloud.ibm.com/identity/token
    ```
    {: codeblock}


    | Input parameters | Values |
    | ---- | --- |
    | Header | :`Content-Type: application/x-www-form-urlencoded`:`Authorization: Basic Yng6Yng=`[^] </br> `Yng6Yng=` equals the URL-encoded authorization for the username **bx** and the password **bx**.|
    | Body for {{site.data.keyword.cloud_notm}} username and password | :`grant_type: password`:`response_type: cloud_iam uaa`:`username`: Your {{site.data.keyword.cloud_notm}} username. :`password`: Your {{site.data.keyword.cloud_notm}} password. :`uaa_client_ID: cf`:`uaa_client_secret:` </br> Add the `uaa_client_secret` key with no value specified.:`bss_account`: The {{site.data.keyword.cloud_notm}} account ID that you retrieved in the previous step. |
    | Body for {{site.data.keyword.cloud_notm}} API keys | :`grant_type: urn:ibm:params:oauth:grant-type:apikey`:`response_type: cloud_iam uaa`:`apikey`: Your {{site.data.keyword.cloud_notm}} API key.:`uaa_client_ID: cf`:`uaa_client_secret:` </br> Add the `uaa_client_secret` key with no value specified.:`bss_account`: The {{site.data.keyword.cloud_notm}} account ID that you retrieved in the previous step. |
    | Body for {{site.data.keyword.cloud_notm}} one-time passcode | :`grant_type:urn:ibm:params:oauth:grant-type:passcode`:`response_type: cloud_iam uaa`:`passcode`: Your {{site.data.keyword.cloud_notm}} passcode. :`uaa_client_ID: cf`:`uaa_client_secret:` </br> Add the `uaa_client_secret` key with no value specified. :`bss_account`: The {{site.data.keyword.cloud_notm}} account ID that you retrieved in the previous step. |
    {: caption="Input parameters to get IAM tokens." caption-side="top"}

    **Example output**

    ```sh
    {
        "access_token": "<iam_token>",
        "refresh_token": "<iam_refresh_token>",
        "token_type": "Bearer",
        "expires_in": 3600,
        "expiration": 1493747503
    }
    ```
    {: screen}

    You can find the {{site.data.keyword.cloud_notm}} IAM token in the **`access_token`** and the refresh token in the **`refresh_token`** field of your API output.

4. Use the {{site.data.keyword.bpshort}} API to list all the workspaces in your account. 

    **Syntax to list all workspaces**
    ```sh
        GET https://schematics.cloud.ibm.com/v1/workspaces/ 
    ```
    {: codeblock}


        | Input parameters | Values |
        | ----- |  --- |
        | Header | `Authorization: bearer <iam_token>`|
        {: caption="Input parameters to work with the {{site.data.keyword.bplong_notm}} API." caption-side="top"}

    **Syntax to retrieve information about a specific workspace**:
    ```sh
    GET https://schematics.cloud.ibm.com/v1/workspaces/{id}
    ```
    {: codeblock}

    | Input parameters | Values |
    | ----- | --- |
    | Header | `Authorization: bearer <iam_token>`: your Your {{site.data.keyword.cloud_notm}} IAM access token.|
    | Path | `id <workspace_ID>`: The ID of the workspace. To retrieve the workspace ID, run `ibmcloud schematics workspace list` |
    {: caption="Input parameters to work with the {{site.data.keyword.bplong_notm}} API." caption-side="top"}

5. Review the [{{site.data.keyword.bplong_notm}} API documentation](/apidocs/schematics/schematics#introduction){: external} to find a list of supported `APIs`.

## Refreshing {{site.data.keyword.cloud_notm}} IAM access tokens and obtaining new refresh tokens with the API
{: #api_refresh}

Every {{site.data.keyword.iamlong}} (IAM) access token that is issued via the API expires after one hour. You must refresh your access token regularly to assure access to the {{site.data.keyword.cloud_notm}} API. You can use the same steps to obtain a new refresh token.
{: shortdesc}

Before you begin, make sure that you have an {{site.data.keyword.cloud_notm}} IAM refresh token or an {{site.data.keyword.cloud_notm}} API key that you can use to request a new access token.
- **Refresh token:** Follow the instructions in [Automating the workspace creation and management process with the {{site.data.keyword.cloud_notm}} API](#cs_api).
- **API key:** Retrieve your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} API key as follows.
    1. From the menu bar, click **Manage** > **Access (IAM)**.
    2. Click the **Users** page and then select yourself.
    3. In the **API keys** pane, click **Create an {{site.data.keyword.cloud_notm}} API key**.
    4. Enter a **Name** and **Description** for your API key and click **Create**.
    5. Click **Show** to see the API key that was generated for you.
    6. Copy the API key so that you can use it to retrieve your new {{site.data.keyword.cloud_notm}} IAM access token.

Use the following steps if you want to create an {{site.data.keyword.cloud_notm}} IAM token or if you want to obtain a new refresh token.

1. Generate a new {{site.data.keyword.cloud_notm}} IAM access token by using the refresh token or the {{site.data.keyword.cloud_notm}} API key.
    ```sh
    POST https://iam.cloud.ibm.com/identity/token
    ```
    {: codeblock}


    | Input parameters | Values |
    | ---- | --- |
    | Header | :`Content-Type: application/x-www-form-urlencoded`:`Authorization: Basic Yng6Yng=` </br></br>  `Yng6Yng=` equals the URL-encoded authorization for the username **bx** and the password **bx**. |
    | Body when using the refresh token |:`grant_type: refresh_token`:`response_type: cloud_iam uaa`:`refresh_token:` Your {{site.data.keyword.cloud_notm}} IAM refresh token. :`uaa_client_ID: cf`:`uaa_client_secret:`:`bss_account:` Your {{site.data.keyword.cloud_notm}} account ID. </br></br> Add the `uaa_client_secret` key with no value specified. |
    | Body when using the {{site.data.keyword.cloud_notm}} API key | :`grant_type: urn:ibm:params:oauth:grant-type:apikey`:`response_type: cloud_iam uaa`:`apikey:` Your {{site.data.keyword.cloud_notm}} API key. :`uaa_client_ID: cf`:`uaa_client_secret:`  Add the `uaa_client_secret` key with no value specified. |
    {: caption="Input parameters for a new {{site.data.keyword.cloud_notm}} IAM token." caption-side="top"}

    **Example API output**

    ```text
    {
        "access_token": "<iam_token>",
        "refresh_token": "<iam_refresh_token>",
        "uaa_token": "<uaa_token>",
        "uaa_refresh_token": "<uaa_refresh_token>",
        "token_type": "Bearer",
        "expires_in": 3600,
        "expiration": 1493747503
    }
    ```
    {: screen}


    You can find your new {{site.data.keyword.cloud_notm}} IAM token in the **`access_token`**, and the refresh token in the **`refresh_token`** field of your API output.

2. Continue working with the [{{site.data.keyword.bplong_notm}} API documentation](/apidocs/schematics){: external} by using the token from the previous step.


## Table1
{: #table1}

| Input parameters | Values |
| ---- | ---- |
| Header | :`Content-Type: application/x-www-form-urlencoded`:Authorization: Basic Yng6Yng=[^] </br>`Yng6Yng=` equals the URL-encoded authorization for the username **bx** and the password **bx**.|
| Body for {{site.data.keyword.cloud_notm}} username and password | :`grant_type: password`:`response_type: cloud_iam uaa`:`username`: Your {{site.data.keyword.cloud_notm}} username.:`password`: Your {{site.data.keyword.cloud_notm}} password.:`uaa_client_id: cf`:`uaa_client_secret:`</br> Add the `uaa_client_secret` key with no value specified. |
| Body for {{site.data.keyword.cloud_notm}} API keys | :`grant_type: urn:ibm:params:oauth:grant-type:apikey`:`response_type: cloud_iam uaa`:`apikey`: Your {{site.data.keyword.cloud_notm}} API key:`uaa_client_id: cf`:`uaa_client_secret:` </br> Add the `uaa_client_secret` key with no value specified. |
| Body for {{site.data.keyword.cloud_notm}} one-time passcode | :`grant_type:urn:ibm:params:oauth:grant-type:passcode`:`response_type: cloud_iam uaa`:`passcode`: Your {{site.data.keyword.cloud_notm}} one-time passcode. Run `ibmcloud login --sso` and follow the instructions in your CLI output to retrieve your one-time passcode by using your web browser.:`uaa_client_id: cf`:`uaa_client_secret:` </br> Add the `uaa_client_secret` key with no value specified. |
{: caption="Table" caption-side="bottom"}


