---

copyright:
  years: 2017, 2021
lastupdated: "2021-03-29"

keywords: schematics api, schematics command line, schematics commands, terraform commands, terraform API, setting up schematics api, api

subcollection: schematics

---

{:DomainName: data-hd-keyref="APPDomain"}
{:DomainName: data-hd-keyref="DomainName"}
{:android: data-hd-operatingsystem="android"}
{:api: .ph data-hd-interface='api'}
{:apikey: data-credential-placeholder='apikey'}
{:app_key: data-hd-keyref="app_key"}
{:app_name: data-hd-keyref="app_name"}
{:app_secret: data-hd-keyref="app_secret"}
{:app_url: data-hd-keyref="app_url"}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:fuzzybunny: .ph data-hd-programlang='fuzzybunny'}
{:generic: data-hd-operatingsystem="generic"}
{:generic: data-hd-programlang="generic"}
{:gif: data-image-type='gif'}
{:go: .ph data-hd-programlang='go'}
{:help: data-hd-content-type='help'}
{:hide-dashboard: .hide-dashboard}
{:hide-in-docs: .hide-in-docs}
{:important: .important}
{:ios: data-hd-operatingsystem="ios"}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note .note}
{:note: .note}
{:objectc data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:route: data-hd-keyref="route"}
{:row-headers: .row-headers}
{:ruby: .ph data-hd-programlang='ruby'}
{:ruby: data-hd-programlang="ruby"}
{:runtime: architecture="runtime"}
{:runtimeIcon: .runtimeIcon}
{:runtimeIconList: .runtimeIconList}
{:runtimeLink: .runtimeLink}
{:runtimeTitle: .runtimeTitle}
{:screen: .screen}
{:script: data-hd-video='script'}
{:service: architecture="service"}
{:service_instance_name: data-hd-keyref="service_instance_name"}
{:service_name: data-hd-keyref="service_name"}
{:shortdesc: .shortdesc}
{:space_name: data-hd-keyref="space_name"}
{:step: data-tutorial-type='step'}
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:tutorial: data-hd-content-type='tutorial'}
{:ui: .ph data-hd-interface='ui'}
{:unity: .ph data-hd-programlang='unity'}
{:url: data-credential-placeholder='url'}
{:user_ID: data-hd-keyref="user_ID"}
{:vbnet: .ph data-hd-programlang='vb.net'}
{:video: .video}

# Setting up the API
{: #setup-api}

You can use the {{site.data.keyword.bplong}} API to automate {{site.data.keyword.bpshort}} capabilities in {{site.data.keyword.cloud_notm}}. To use the CLI, see [Setting up the CLI](/docs/schematics?topic=schematics-setup-cli).
{: shortdesc}

To find an overview of supported {{site.data.keyword.bplong}} APIs, API endpoints, and required API header and body information, see the [{{site.data.keyword.bplong}} API documentation](/apidocs/schematics){: external}. 
{: tip}

## Automating deployments with the API
{: #cs_api}

Learn how to use the {{site.data.keyword.cloud_notm}} API to retrieve an Identity and Access Management token so that you can access the {{site.data.keyword.bpshort}} API. 
{: shortdesc}

To authenticate with {{site.data.keyword.bplong_notm}}, you must provide an {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) token that is generated with your {{site.data.keyword.cloud_notm}} credentials. Depending on the way you authenticate with {{site.data.keyword.cloud_notm}}, you can choose between the following options to automate the creation of your {{site.data.keyword.cloud_notm}} IAM token.


|{{site.data.keyword.cloud_notm}} ID|My options|
|-----------------------------------|----------|
|Unfederated ID|<ul><li>**Generate an {{site.data.keyword.cloud_notm}} API key:** As an alternative to using the {{site.data.keyword.cloud_notm}} username and password, you can [use {{site.data.keyword.cloud_notm}} API keys](/docs/account?topic=account-userapikey#create_user_key){: external}. {{site.data.keyword.cloud_notm}} API keys are dependent on the {{site.data.keyword.cloud_notm}} account they are generated for. You cannot combine your {{site.data.keyword.cloud_notm}} API key with a different account ID in the same {{site.data.keyword.cloud_notm}} IAM token. To access workspaces that were created with an account other than the one your {{site.data.keyword.cloud_notm}} API key is based on, you must log in to the account to generate a new API key.</li><li>**{{site.data.keyword.cloud_notm}} username and password:** You can follow the steps in this topic to fully automate the creation of your {{site.data.keyword.cloud_notm}} IAM access token.</li></ul>|
|Federated ID|<ul><li>**Generate an {{site.data.keyword.cloud_notm}} API key:** [{{site.data.keyword.cloud_notm}} API keys](/docs/account?topic=account-userapikey#create_user_key){: external} are dependent on the {{site.data.keyword.cloud_notm}} account they are generated for. You cannot combine your {{site.data.keyword.cloud_notm}} API key with a different account ID in the same {{site.data.keyword.cloud_notm}} IAM token. To access workspaces that were created with an account other than the one your {{site.data.keyword.cloud_notm}} API key is based on, you must log in to the account to generate a new API key.</li><li>**Use a one-time passcode:** If you authenticate with {{site.data.keyword.cloud_notm}} by using a one-time passcode, you cannot fully automate the creation of your {{site.data.keyword.cloud_notm}} IAM token because the retrieval of your one-time passcode requires a manual interaction with your web browser. To fully automate the creation of your {{site.data.keyword.cloud_notm}} IAM token, you must create an {{site.data.keyword.cloud_notm}} API key instead.</ul>|
{: caption="ID types and options" caption-side="top"}
{: summary="ID types and options with the input parameter in column 1 and the value in column 2."}

1.  Create your {{site.data.keyword.cloud_notm}} IAM access token. The body information that is included in your request varies based on the {{site.data.keyword.cloud_notm}} authentication method that you use.

    ```
    POST https://iam.cloud.ibm.com/identity/token
    ```
    {: codeblock}

    <table summary="Input parameters to retrieve IAM tokens with the input parameter in column 1 and the value in column 2.">
    <caption>Input parameters to get IAM tokens.</caption>
    <thead>
        <th>Input parameters</th>
        <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li><code>Content-Type: application/x-www-form-urlencoded</code></li> <li>Authorization: Basic Yng6Yng=<p>**Note**: `Yng6Yng=` equals the URL-encoded authorization for the username **bx** and the password **bx**.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.cloud_notm}} username and password</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: Your {{site.data.keyword.cloud_notm}} username.</li>
    <li>`password`: Your {{site.data.keyword.cloud_notm}} password.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:`</br>**Note**: Add the `uaa_client_secret` key with no value specified.</li></ul></td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.cloud_notm}} API keys</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: Your {{site.data.keyword.cloud_notm}} API key</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br>**Note**: Add the `uaa_client_secret` key with no value specified.</li></ul></td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.cloud_notm}} one-time passcode</td>
      <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:passcode`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: Your {{site.data.keyword.cloud_notm}} one-time passcode. Run `ibmcloud login --sso` and follow the instructions in your CLI output to retrieve your one-time passcode by using your web browser.</li>
    <li>`uaa_client_id: cf`</li>
    <li>`uaa_client_secret:` </br>**Note**: Add the `uaa_client_secret` key with no value specified.</li></ul>
    </td>
    </tr>
    </tbody>
    </table>

    Example output for using an API key:

    ```
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

    You can find the {{site.data.keyword.cloud_notm}} IAM token in the **access_token** field of your API output. Note the {{site.data.keyword.cloud_notm}} IAM token to retrieve additional header information in the next steps.

2.  Retrieve the ID of the {{site.data.keyword.cloud_notm}} account that you want to work with. Replace `<iam_access_token>` with the {{site.data.keyword.cloud_notm}} IAM token that you retrieved from the **access_token** field of your API output in the previous step. In your API output, you can find the ID of your {{site.data.keyword.cloud_notm}} account in the **resources.metadata.guid** field.

    ```
    GET https://accounts.cloud.ibm.com/coe/v2/accounts
    ```
    {: codeblock}

    <table summary="Input parameters to get {{site.data.keyword.cloud_notm}} account ID with the input parameter in column 1 and the value in column 2.">
    <caption>Input parameters to get an {{site.data.keyword.cloud_notm}} account ID.</caption>
    <thead>
  	<th>Input parameters</th>
  	<th>Values</th>
    </thead>
    <tbody>
  	<tr>
  		<td>Headers</td>
      <td><ul><li>`Content-Type: application/json`</li>
        <li>`Authorization: bearer <iam_access_token>`</li>
        <li>`Accept: application/json`</li></ul></td>
  	</tr>
    </tbody>
    </table>

    Example output:

    ```
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
    ...
    ```
    {: screen}

3.  Generate a new {{site.data.keyword.cloud_notm}} IAM token that includes your {{site.data.keyword.cloud_notm}} credentials and the account ID that you want to work with.

    If you use an {{site.data.keyword.cloud_notm}} API key, you must use the {{site.data.keyword.cloud_notm}} account ID the API key was created for. To access {{site.data.keyword.bpshort}} workspaces or actions in other accounts, log into this account and create an {{site.data.keyword.cloud_notm}} API key that is based on this account.
    {: note}

    ```
    POST https://iam.cloud.ibm.com/identity/token
    ```
    {: codeblock}

    <table summary="Input parameters to retrieve IAM tokens with the input parameter in column 1 and the value in column 2.">
    <caption>Input parameters to get IAM tokens.</caption>
    <thead>
        <th>Input parameters</th>
        <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li><code>Content-Type: application/x-www-form-urlencoded</code></li> <li>`Authorization: Basic Yng6Yng=`<p>**Note**: `Yng6Yng=` equals the URL-encoded authorization for the username **bx** and the password **bx**.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.cloud_notm}} username and password</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username`: Your {{site.data.keyword.cloud_notm}} username. </li>
    <li>`password`: Your {{site.data.keyword.cloud_notm}} password. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br>**Note**: Add the `uaa_client_secret` key with no value specified.</li>
    <li>`bss_account`: The {{site.data.keyword.cloud_notm}} account ID that you retrieved in the previous step.</li></ul>
    </td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.cloud_notm}} API keys</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey`: Your {{site.data.keyword.cloud_notm}} API key.</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br>**Note**: Add the `uaa_client_secret` key with no value specified.</li>
    <li>`bss_account`: The {{site.data.keyword.cloud_notm}} account ID that you retrieved in the previous step.</li></ul>
      </td>
    </tr>
    <tr>
    <td>Body for {{site.data.keyword.cloud_notm}} one-time passcode</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:passcode`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode`: Your {{site.data.keyword.cloud_notm}} passcode. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:` </br>**Note**: Add the `uaa_client_secret` key with no value specified.</li>
    <li>`bss_account`: The {{site.data.keyword.cloud_notm}} account ID that you retrieved in the previous step.</li></ul></td>
    </tr>
    </tbody>
    </table>

    Example output:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    You can find the {{site.data.keyword.cloud_notm}} IAM token in the **access_token** and the refresh token in the **refresh_token** field of your API output.

4.  Use the {{site.data.keyword.bpshort}} API to list all the workspaces in your account. 
  * **Syntax to list all workspaces**:
     ```
     GET https://schematics.cloud.ibm.com/v1/workspaces/ 
     ```
     {: codeblock}

     <table summary="Input parameters to work with the {{site.data.keyword.bplong_notm}} API with the input parameter in column 1 and the value in column 2.">
     <caption>Input parameters to work with the {{site.data.keyword.bplong_notm}} API.</caption>
     <thead>
     <th>Input parameters</th>
     <th>Values</th>
     </thead>
     <tbody>
     <tr>
     <td>Header</td>
     <td><li>`Authorization: bearer <iam_token>`</td>
     </tr>
     </tbody>
     </table>
  * **Syntax to retrieve information about a specific workspace**:
     ```
     GET https://schematics.cloud.ibm.com/v1/workspaces/{id}
     ```
     {: codeblock}

     <table summary="Input parameters to work with the {{site.data.keyword.bplong_notm}} API with the input parameter in column 1 and the value in column 2.">
     <caption>Input parameters to work with the {{site.data.keyword.bplong_notm}} API.</caption>
     <thead>
     <th>Input parameters</th>
     <th>Values</th>
     </thead>
     <tbody>
     <tr>
     <td>Header</td>
     <td>`Authorization: bearer &lt;iam_token&gt;`: Your {{site.data.keyword.cloud_notm}} IAM access token.</td>
     </tr>
     <tr>
     <td>Path</td>
     <td>`id: &lt;workspace_ID&gt;`: The ID of the workspace. To retrieve the workspace ID, run &lt;ibmcloud schematics workspace list&gt;</td>
     </tbody>
     </table>

5.  Review the [{{site.data.keyword.bplong_notm}} API documentation](/apidocs/schematics#introduction){: external} to find a list of supported APIs.


<br />


## Refreshing {{site.data.keyword.cloud_notm}} IAM access tokens and obtaining new refresh tokens with the API
{: #api_refresh}

Every {{site.data.keyword.cloud_notm}} Identity and Access Management (IAM) access token that is issued via the API expires after one hour. You must refresh your access token regularly to assure access to the {{site.data.keyword.cloud_notm}} API. You can use the same steps to obtain a new refresh token.
{: shortdesc}

Before you begin, make sure that you have an {{site.data.keyword.cloud_notm}} IAM refresh token or an {{site.data.keyword.cloud_notm}} API key that you can use to request a new access token.
- **Refresh token:** Follow the instructions in [Automating the workspace creation and management process with the {{site.data.keyword.cloud_notm}} API](#cs_api).
- **API key:** Retrieve your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/) API key as follows.
   1. From the menu bar, click **Manage** > **Access (IAM)**.
   2. Click the **Users** page and then select yourself.
   3. In the **API keys** pane, click **Create an IBM Cloud API key**.
   4. Enter a **Name** and **Description** for your API key and click **Create**.
   4. Click **Show** to see the API key that was generated for you.
   5. Copy the API key so that you can use it to retrieve your new {{site.data.keyword.cloud_notm}} IAM access token.

Use the following steps if you want to create an {{site.data.keyword.cloud_notm}} IAM token or if you want to obtain a new refresh token.

1.  Generate a new {{site.data.keyword.cloud_notm}} IAM access token by using the refresh token or the {{site.data.keyword.cloud_notm}} API key.
    ```
    POST https://iam.cloud.ibm.com/identity/token
    ```
    {: codeblock}

    <table summary="Input parameters for new IAM token with the input parameter in column 1 and the value in column 2.">
    <caption>Input parameters for a new {{site.data.keyword.cloud_notm}} IAM token</caption>
    <thead>
    <th>Input parameters</th>
    <th>Values</th>
    </thead>
    <tbody>
    <tr>
    <td>Header</td>
    <td><ul><li><code>Content-Type: application/x-www-form-urlencoded</code></li>
      <li>`Authorization: Basic Yng6Yng=`</br></br>**Note:** `Yng6Yng=` equals the URL-encoded authorization for the username **bx** and the password **bx**.</li></ul></td>
    </tr>
    <tr>
    <td>Body when using the refresh token</td>
    <td><ul><li>`grant_type: refresh_token`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`refresh_token:` Your {{site.data.keyword.cloud_notm}} IAM refresh token. </li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li>
    <li>`bss_account:` Your {{site.data.keyword.cloud_notm}} account ID. </li></ul>**Note**: Add the `uaa_client_secret` key with no value specified.</td>
    </tr>
    <tr>
      <td>Body when using the {{site.data.keyword.cloud_notm}} API key</td>
      <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey:` Your {{site.data.keyword.cloud_notm}} API key. </li>
    <li>`uaa_client_ID: cf`</li>
        <li>`uaa_client_secret:`</li></ul>**Note:** Add the `uaa_client_secret` key with no value specified.</td>
    </tr>
    </tbody>
    </table>

    Example API output:

    ```
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

    You can find your new {{site.data.keyword.cloud_notm}} IAM token in the **access_token**, and the refresh token in the **refresh_token** field of your API output.

2.  Continue working with the [{{site.data.keyword.bplong_notm}} API documentation](/apidocs/schematics){: external} by using the token from the previous step.

<br />

