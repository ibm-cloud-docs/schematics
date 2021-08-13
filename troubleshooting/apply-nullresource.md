---

copyright:
  years: 2017, 2021
lastupdated: "2021-08-13"

keywords: schematics, schematics action, create schematics actions, run ansible playbooks, delete schematics action, 

subcollection: schematics
content-type: troubleshoot

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
{:audio: .audio}
{:authenticated-content: .authenticated-content}
{:beta: .beta}
{:c#: .ph data-hd-programlang='c#'}
{:c#: data-hd-programlang="c#"}
{:cli: .ph data-hd-interface='cli'}
{:codeblock: .codeblock}
{:curl: #curl .ph data-hd-programlang='curl'}
{:curl: .ph data-hd-programlang='curl'}
{:deprecated: .deprecated}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:download: .download}
{:external: .external target="_blank"}
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
{:java: #java .ph data-hd-programlang='java'}
{:java: .ph data-hd-programlang='java'}
{:java: data-hd-programlang="java"}
{:javascript: .ph data-hd-programlang='javascript'}
{:javascript: data-hd-programlang="javascript"}
{:middle: .ph data-hd-position='middle'}
{:navgroup: .navgroup}
{:new_window: target="_blank"}
{:node: .ph data-hd-programlang='node'}
{:note: .note}
{:objectc: .ph data-hd-programlang='Objective C'}
{:objectc: data-hd-programlang="objectc"}
{:org_name: data-hd-keyref="org_name"}
{:php: .ph data-hd-programlang='PHP'}
{:php: data-hd-programlang="php"}
{:pre: .pre}
{:preview: .preview}
{:python: .ph data-hd-programlang='python'}
{:python: data-hd-programlang="python"}
{:right: .ph data-hd-position='right'}
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
{:step: data-tutorial-type='step'} 
{:subsection: outputclass="subsection"}
{:support: data-reuse='support'}
{:swift: #swift .ph data-hd-programlang='swift'}
{:swift: .ph data-hd-programlang='swift'}
{:swift: data-hd-programlang="swift"}
{:table: .aria-labeledby="caption"}
{:term: .term}
{:terraform: .ph data-hd-interface='terraform'}
{:tip: .tip}
{:tooling-url: data-tooling-url-placeholder='tooling-url'}
{:topicgroup: .topicgroup}
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



# How can I find the root cause of why {{site.data.keyword.bpshort}} apply is failing?
{: #nullresource-errors}

You want to apply a Terraform template in {{site.data.keyword.cloud_notm}} that runs scripts on a target resource. To run the script, you use the Terraform `null_resource` in your configuration file. However, when you run the {{site.data.keyword.bpshort}} apply action, the action fails and you receive an error message that can include internal, timeout, connection, or input errors. 
{: tsSymptoms}

When {{site.data.keyword.bpshort}} tries to run your script on the target resource, a script error occurs during the execution. Because {{site.data.keyword.bpshort}} cannot resolve errors that occur in user-provided scripts, the apply action is marked as `failed`.
{: tsCauses}

To troubleshoot the error in the script, follow these steps:
{: tsResolve}

1. From the workspace **Activity** page, select the {{site.data.keyword.bpshort}} apply action that failed.
2. Click **View log** to see the detailed log output. 
3. In the log file, find the last action that {{site.data.keyword.bpshort}} started before the error occurs. For example in the following log output, {{site.data.keyword.bpshort}} tried to run a copy script in the `instances_module` module by using the Terraform `null_resource`.
    ```
    2021/05/24 05:03:41 Terraform apply | module.instances_module.module.compute_remote_copy_rpms.null_resource.remote_copy[0]: Still creating... [5m0s elapsed]
    2021/05/24 05:03:41 Terraform apply | 
    2021/05/24 05:03:42 Terraform apply | 
    2021/05/24 05:03:42 Terraform apply | 
    2021/05/24 05:03:42 Terraform apply | Error: timeout - last error: ssh: rejected: connect failed (Connection timed out)
    ```
    {: screen}

4. Find the script that is executed in the Terraform `null_resource` and analyze where the error might come from by using the error message from the {{site.data.keyword.bpshort}} log output. 
5. If you cannot resolve the error, contact the owner of the script to further troubleshoot the error. 



