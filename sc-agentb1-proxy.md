---

copyright:
  years: 2017, 2025
lastupdated: "2025-04-07"

keywords: schematics agent proxy server, proxy server, agent proxy, command-line, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Configuring {{site.data.keyword.bpshort}} agents to use a proxy server
{: #proxy-agent-overview}

Many environments do not allow direct connectivity to the public internet, but require that connections are routed by a secure proxy gateway. {{site.data.keyword.bpshort}} Agent configuration policies allow use of a proxy server where direct access to the internet is not allowed. You can use a HTTP or HTTPS proxy.
{: shortdesc}

The proxy configuration must be the same on each host in the cluster. Therefore, when setting up the proxy or modifying it, you must update the deployment, ConfigMap, and NetworkPolicy YAML specification file. Then restart the deployment to apply the proxy server configuration.

## Before you begin
{: #proxy-prereq}

Confirm the following requirements are in place before you configure proxy server access. The example here uses a squid proxy on port 3128.
{: shortdesc}

- A proxy server that can reach [Google](https://www.google.com){: external} using HTTP or HTTPS and is accessible from the cluster.
- The proxy server IP address and port for example, `http://53.25.191.193:3128`
- The curl command-line tool is available on the cluster pods.

## Configuring a proxy server
{: #proxy-configure}

1. From the [Kubernetes cluster console](https://cloud.ibm.com/kubernetes/clusters){: external}. Click **Kubernetes dashboard**
2. Switch to **schematics-runtime** in the namespace selection drop down box.
3. Under the `Config and Storage` section Click **Config Maps** to display the maps for the `schematics-runtime` namespace. 
    - Click  the `schematics-runtime-job-config` configmap resource and select the pencil icon to edit the YAML file to add the `HTTPS/HTTP` proxy statements. 

    - Add `HTTPS/HTTP` proxy entries under the `data` property in the lower section of the file :

        ```yaml
        data:
          HTTPS_PROXY: http://<ipaddress>:<port>
          HTTP_PROXY: http://<ipaddress>:<port>
        ```
        {: codeblock}
        
    - Click update to save the configmap and confirm that the new proxy entries are showing in the file list. 
    - Repeat for the `schematics-runtime-ansible-job-config` configmap resource. 

4. Under the `Cluster` section Click **Network Policies** to display the policies for the schematics-runtime namespace. 
   - Edit the `whitelist-runtime-egress-gen-ports` NetworkPolicy file to add port used by your `HTTPS/HTTP` proxy. 
   - Add the `HTTPS/HTTP` proxy port to the ports property.
      ```yaml
      - ports:
        - protocol: TCP
          port: 3128
     ```
     {: codeblock}

5. Restart the `schematics-runtime` deployment and verify all the pods are running up.
   - Under the `Workloads` section Click **Deployments** to display the current deployments for the schematics-runtime namespace. 
   - Select the `runtime-job` resource and restart the pod.  
   - Select the `runtime-ansible-job` resource and restart the pod.  

## Verifying proxy server access
{: #proxy-verify}

Verify the cluster pods can access the Internet through the proxy server.
{: shortdesc}

- Log into the `schematics-runtime` pod and run the following `curl` command to verify that the proxy environment variables are being correctly set in the container.
- Under `Workloads > Pods` select the first `job-runtime` and `Exec` into it.  
    
  ```curl
  curl --head https://www.google.com
  ```
  {: codeblock}

If the proxy is working for `HTTPS` the `curl` command returns a `200 OK HTTP response`.


If the curl command fails to connect, verify that the proxy is accessible from the cluster and can route access to the target website. Use the following `curl` command with the proxy option to validate that the proxy is accessible.  

  ```curl
  curl --proxy http://<ipaddress>:<port> --head https://www.google.com
  ```
  {: codeblock}

If the website is not accessible using the proxy parameter, verify the network egress rule for the cluster is set correctly and that there is network access to the proxy.

If the website is accessible using the proxy parameter, verify that the configmap was updated correctly and the `HTTPS/HTTP` proxy environment variables are set by running the `env` command.  

The environment variables `HTTP_PROXY` and `HTTPS_PROXY` should be listed in the output. If not listed recheck the configmap definitions.
