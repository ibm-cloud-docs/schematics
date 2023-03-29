---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-29"

keywords: schematics agent proxy server, proxy server, agent proxy, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bplong_notm}} Agent [beta-1](/docs/schematics?topic=schematics-schematics-relnotes&interface=cli#schematics-mar2223) release delivers a simplified Agents installation process.
{: attention}

{{site.data.keyword.bpshort}} Agent are a [beta-1 feature](/docs/schematics?topic=schematics-agent-beta1-limitations) that are available for evaluation and testing purposes. It is not intended for production usage.
{: beta}

# Setting proxy server for {{site.data.keyword.bpshort}} Agent
{: #proxy-agent-overview}

 A proxy server is an information system or application that acts as an intermediary for clients requesting information system resources such as files, connections, web pages, or services from other servers. Client requests established through an initial connection to the proxy server are evaluated to manage complexity and to provide additional protection by limiting direct connectivity.
 {: shortdesc}

 Schematics Agent organisation policies supports proxy server which cannot allow direct access to internet connectivity. You can use an HTTP or HTTPS proxy.  

Configuring Kubernetes cluster to use these proxies can be as simple as setting standard environment variables in configuration or JSON files. This can be done during an cluster installation or configured after installation.

The proxy configuration must be the same on each host in the cluster. Therefore, when setting up the proxy or modifying it, you must update the deployment, ConfigMap, and NetworkPolicy YAML specification file. Then restart deployment to apply the proxy server.

## Before you begin
{: #proxy-prereq}

Consider the following steps before you setup the proxy server.
{: shortdesc}

- The proxy server address is http://proxy.company.com:3128.
- The proxy server can reach www.google.com by using HTTP or HTTPS.
- The curl command-line tool is available on the cluster pods.

## Configuring proxy server
{: #proxy-configure}

1. From the [Kubernetes clusters console](https://cloud.ibm.com/kubernetes/clusters){: external}. Click **Kubernetes dashboard**
2. Switch to your **runtime** namespace from the drop down box.
3. Click **Config Maps** to edit the `job12` configmap resource `YAML` specification file. Add `HTTPS/HTTP` proxy as stated.
    - Add `HTTPS/HTTP` proxy entries to `kubectl.kubernetes.io/last-applied-configuration` annotation.
    
        ```yaml
        "HTTP_PROXY":"http://52.118.208.29:3128","HTTPS_PROXY":"http://52.118.208.29:3128"
        ```
        {: codeblock}

    - Add `HTTPS/HTTP` proxy entries to data property:

        data:
        ```yaml
        HTTPS_PROXY: http://52.118.208.29:3128
        HTTP_PROXY: http://52.118.208.29:3128
        ```
        {: codeblock}

    - Remove `resourceVersion` property before saving changes to the file.
4. Edit the `job12` deployments `YAML` specification file to add `HTTPS/HTTP` proxy as stated:
   - Add `HTTPS/HTTP` proxy entries to `kubectl.kubernetes.io/last-applied-configuration` annotation.
    
    ```yaml
    {"name":"HTTPS_PROXY","valueFrom":{"configMapKeyRef":{"key":"HTTPS_PROXY","name":"job12"}}},{"name":"HTTP_PROXY","valueFrom":{"configMapKeyRef":{"key":"HTTP_PROXY","name":"job12"}}}
    ```
    {: codeblock}

   -  Add `HTTPS/HTTP` proxy environment variables that need to be set in the YAML specification.

      ```yaml
        - name: HTTPS_PROXY
              valueFrom:
                configMapKeyRef:
                  name: job12
                  key: HTTPS_PROXY
        - name: HTTP_PROXY
              valueFrom:
                configMapKeyRef:
                  name: job12
                  key: HTTP_PROXY
      ```
      {: codeblock}
    
   - Remove `resourceVersion` property before saving changes to the file.
5. Add the proxy server port to `egress` rule.
   - Edit the `whitelist-runtime-egress-gen-ports` NetworkPolicy file to add `HTTPS/HTTP` proxy as below:
   - Add `HTTPS/HTTP` proxy entries to the ports property.
      ```yaml
      - ports:
        - protocol: TCP
          port: 3128
     ```
     {: codeblock}

   - Remove `resourceVersion` property before saving changes to the file.
6. Restart `job12` deployment and verify all the pods are running up.

## Verifying proxy server
{: #proxy-verify}

Verify the cluster pods can access the Internet through the proxy server.
{: shortdesc}

- Log into `job12` pod and run following `curl` command.
    
    ```curl
    curl --proxy http://proxy.company.com:3128 --head http://www.google.com
    ```
    {: codeblock}

    ```curl
    curl --proxy http://proxy.company.com:3128 --head https://www.google.com
    ```
    {: codeblock}

- If the proxy is working for `HTTP` and `HTTPS` the `curl` command returns a `200 OK HTTP response`.
