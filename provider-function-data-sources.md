---

copyright:
  years: 2017, 2019
lastupdated: "2019-11-07"

keywords: about schematics, schematics overview, infrastructure as code, iac, differences schematics and terraform, schematics vs terraform, how does schematics work, schematics benefits, why use schematics, terraform template, schematics workspace

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

# Function Data Sources
{: #function-data-sources}


## ibm_function_action

Import the details of an existing [IBM Cloud Functions action](https://cloud.ibm.com/docs/openwhisk/openwhisk_actions.html#openwhisk_actions) as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_function_action" "nodehello" {
    name = "action-name"		  
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the action.

### Output parameters

The following attributes are exported:

* `id`- The ID of the action.
* `version` - Semantic version of the item.
* `annotations` - Annotations to describe the action, including those set by you or by IBM Cloud Functions.
* `parameters` - Parameters passed to the action when the action is invoked, including those set by you or by IBM Cloud Functions.
* `limits` - A nested block to describe assigned limits. Nested `limits` blocks have the following structure:
    * `timeout` - The timeout limit to terminate the action, specified in milliseconds. Default value: `60000`.
    * `memory` - The maximum memory for the action, specified in MBs. Default value: `256`.
    * `log_size` - The maximum log size for the action, specified in MBs. Default value: `10`.
* `exec` - A nested block to describe executable binaries. Nested `exec` blocks have the following structure:
    * `image` - When using the `blackbox` executable, the name of the container image name.
    * `init` - When using `nodejs`, the optional zipfile reference.
    * `code` - When not using the `blackbox` executable, the code to execute. 
    * `kind` - The type of action. Accepted values: `nodejs`, `blackbox`, `swift`, `sequence`.
    * `main` - The name of the action entry point (function or fully-qualified method name, when applicable).
    * `components` - The list of fully qualified actions.
* `publish` - Action visibility.



## ibm_function_package

Import the details of an existing [IBM Cloud Functions package](https://cloud.ibm.com/docs/openwhisk/openwhisk_packages.html#openwhisk_packages) as a read-only data source. The fields of the data source can then be referenced by other resources within the same configuration using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_function_package" "package" {
  name = "package_name"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the package.


### Output parameters

The following attributes are exported:

* `id` - The ID of the package.
* `version` - Semantic version of the package.
* `publish` - Package visibility.
* `annotations` - All annotations to describe the package, including those set by you or by IBM Cloud Functions.
* `parameters` - All parameters passed to the package, including those set by you or by IBM Cloud Functions.



## ibm_function_rule

Import the details of an existing [IBM Cloud Functions rule](https://cloud.ibm.com/docs/openwhisk/openwhisk_triggers_rules.html#openwhisk_triggers) as a read-only data source. You can then reference the fields of the data source in other resources within the same configuration by using interpolation syntax.

### Sample Terraform code

```hcl
data "ibm_function_rule" "rule" {
	name = "rule-name"
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the rule.

### Output parameters

The following attributes are exported:

* `id` - The ID of the trigger.
* `publish` - Trigger visibility.
* `version` - Semantic version of the trigger.
* `status` - The status of the rule.
* `trigger_name` - The name of the trigger.
* `action_name` - The name of the action.



## ibm_function_trigger

Import the details of an existing [IBM Cloud Functions trigger](https://cloud.ibm.com/docs/openwhisk/openwhisk_triggers_rules.html#openwhisk_triggers) as a read-only data source. The fields of the data source can then be referenced by other resources within the same configuration using interpolation syntax.


### Sample Terraform code

```hcl
data "ibm_function_trigger" "trigger" {
			name = "trigger-name"		  
}
```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the trigger.

### Output parameters

The following attributes are exported:

* `id` - The ID of the trigger.
* `publish` - Trigger visibility.
* `version` - Semantic version of the trigger.
* `annotations` - All annotations to describe the trigger, including those set by you or by IBM Cloud Functions.
* `parameters` - All parameters passed to the trigger, including those set by you or by IBM Cloud Functions.
