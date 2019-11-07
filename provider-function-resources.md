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

# Function Resources
{: #function-resources}


## ibm_function_action

Create, update, or delete [IBM Cloud Functions actions](https://cloud.ibm.com/docs/openwhisk/openwhisk_actions.html#openwhisk_actions). Actions are stateless code snippets that run on the IBM Cloud Functions platform. An action can be written as a JavaScript, Swift, or Python function, a Java method, or a custom executable program packaged in a Docker container. To bundle and share related actions, use the `function_package` resource.


### Sample Terraform code

####  Simple JavaScript action

```hcl
resource "ibm_function_action" "nodehello" {
  name = "action-name"

  exec = {
    kind = "nodejs:6"
    code = "${file("hellonode.js")}"
  }
}

```
#### Passing parameters to an action

```hcl
resource "ibm_function_action" "nodehellowithparameter" {
  name = "hellonodeparam"

  exec = {
    kind = "nodejs:6"
    code = "${file("hellonodewithparameter.js")}"
  }

  user_defined_parameters = <<EOF
        [
    {
        "key":"place",
        "value":"India"
    }
        ]
        EOF
}

```

#### Packaging an action as a Node.js module

``` hcl
resource "ibm_function_action" "nodezip" {
  name = "nodezip"

  exec = {
    kind = "nodejs:6"
    code = "${base64encode("${file("nodeaction.zip")}")}"
  }
}

```

#### Creating action sequences

``` hcl
resource "ibm_function_action" "swifthello" {
  name = "actionsequence"

  exec = {
    kind = "sequence"
    components = ["/whisk.system/utils/split","/whisk.system/utils/sort"]
  }
}

```

### Creating Docker actions

``` hcl
resource "ibm_function_action" "swifthello" {
  name = "dockeraction"

  exec = {
    kind = "janesmith/blackboxdemo"
    image = "${file("helloSwift.swift")}"
  }
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the action.
* `limits` - (Optional, list) A nested block to describe assigned limits. Nested `limits` blocks have the following structure:
    * `timeout` - The timeout limit to terminate the action, specified in milliseconds. Default value: `60000`.
    * `memory` - The maximum memory for the action, specified in MBs. Default value: `256`.
    * `log_size` - The maximum log size for the action, specified in MBs. Default value: `10`.
* `exec` - (Required, list) A nested block to describe executable binaries. Nested `exec` blocks have the following structure:
    * `image` - (Optional, string) When using the `blackbox` executable, the name of the container image name.  
     **NOTE**: Conflicts with `exec.components`, `exec.code`.
    * `init` - (Optional, string) When using `nodejs`, the optional zipfile reference.  
     **NOTE**: Conflicts with `exec.components`, `exec.image`.
    * `code` - (Optional, string) When not using the `blackbox` executable, the code to execute.  
    **NOTE**: Conflicts with `exec.components`, `exec.image`.
    * `kind` - (Required, string) The type of action. You can find supported kinds in the [IBM Cloud Functions docs](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes).
    * `main` - (Optional, string) The name of the action entry point (function or fully-qualified method name, when applicable).  
    **NOTE**: Conflicts with `exec.components`, `exec.image`.
    * `components` - (Optional, string) The list of fully qualified actions.  
    **NOTE**: Conflicts with `exec.code`, `exec.image`.
* `publish` - (Optional, boolean) Action visibility.
* `user_defined_annotations` - (Optional, string) Annotations defined in key value format.
* `user_defined_parameters` - (Optional, string) Parameters defined in key value format. Parameter bindings included in the context passed to the action. Cloud Function backend/API.

### Output parameters

The following attributes are exported:

* `id` - The ID of the new action.
* `version` - Semantic version of the item.
* `annotations` - All annotations to describe the action, including those set by you or by IBM Cloud Functions.
* `parameters` - All parameters passed to the action when the action is invoked, including those set by you or by IBM Cloud Functions.


### Import

`ibm_function_action` can be imported using the ID.

Example:

```
$ terraform import ibm_function_action.nodeAction hello

```



## ibm_function_package

Create, update, or delete [IBM Cloud Functions packages](https://cloud.ibm.com/docs/openwhisk/openwhisk_packages.html#openwhisk_packages). You can use packages to bundle together a set of related actions, and share them with others. To create actions, use the `function_action` resource.

### Sample Terraform code

#### Create a package

```hcl
resource "ibm_function_package" "package" {
  name = "package-name"

  user_defined_annotations = <<EOF
        [
    {
        "key":"description",
        "value":"Count words in a string"
    },
    {
        "key":"sampleOutput",
        "value": {
                        "count": 3
                }
    },
    {
        "key":"final",
        "value": [
                        {
                                "description": "A string",
                                "name": "payload",
                                "required": true
                        }
                ]
    }
]
EOF
}
```

#### Create a package using a binding

``` hcl
resource "ibm_function_package" "bindpackage" {
  name              = "bindalaram"
  bind_package_name = "/whisk.system/alarms/alarm"

  user_defined_parameters = <<EOF
        [
    {
        "key":"cron",
        "value":"0 0 1 0 *"
    },
    {
        "key":"trigger_payload ",
        "value":"{'message':'bye old Year!'}"
    },
    {
        "key":"maxTriggers",
        "value":1
    },
    {
        "key":"userdefined",
        "value":"test"
    }
]
EOF
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the package.
* `publish` - (Optional, boolean) Package visibility.
* `user_defined_annotations` - (Optional, string) Annotations defined in key value format.
* `user_defined_parameters` - (Optional, string) Parameters defined in key value format. Parameter bindings included in the context passed to the package.
* `bind_package_name` - (Optional, string) Name of package to be binded.

### Output parameters

The following attributes are exported:

* `id` - The ID of the new package.
* `version` - Semantic version of the item.
* `annotations` - All annotations to describe the package, including those set by you or by IBM Cloud Functions.
* `parameters` - All parameters passed to the package, including those set by you or by IBM Cloud Functions.

### Import

`ibm_function_package` can be imported using the ID.

Example:

```
$ terraform import ibm_function_package.sample hello

```



## ibm_function_rule

Create, update, or delete [IBM Cloud Functions triggers](https://cloud.ibm.com/docs/openwhisk/openwhisk_triggers_rules.html#openwhisk_triggers). Events from external and internal event sources are channeled through a trigger, and rules allow your actions to react to these events. To set triggers, use the `function_trigger` resource.

### Sample Terraform code

```hcl
resource "ibm_function_action" "action" {
  name = "hello"

  exec = {
    kind = "nodejs:6"
    code = "${file("test-fixtures/hellonode.js")}"
  }
}

resource "ibm_function_trigger" "trigger" {
  name = "alarmtrigger"

  feed = [
    {
      name = "/whisk.system/alarms/alarm"

      parameters = <<EOF
					[
						{
							"key":"cron",
							"value":"0 */2 * * *"
						}
					]
                EOF
    },
  ]
}

resource "ibm_function_rule" "rule" {
  name         = "alarmrule"
  trigger_name = "${ibm_function_trigger.trigger.name}"
  action_name  = "${ibm_function_action.action.name}"
}

```

### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the rule.
* `trigger_name` - (Required, string) The name of the trigger.
* `action_name` - (Required, string) The name of the action.

### Output parameters

The following attributes are exported:

* `id` - The ID of the new rule.
* `publish` - Rule visibility.
* `version` - Semantic version of the item.
* `status` - The status of the rule.

### Import

`ibm_function_rule` can be imported using the ID.

Example: 

```
$ terraform import ibm_function_rule.sampleRule alarmrule

```



## ibm_function_trigger

Create, update, or delete [IBM Cloud Functions triggers](https://cloud.ibm.com/docs/openwhisk/openwhisk_triggers_rules.html#openwhisk_triggers). Events from external and internal event sources are channeled through a trigger, and rules allow your actions to react to these events. To set rules, use the `function_rule` resource.

### Sample Terraform code

#### Creating triggers

```hcl
resource "ibm_function_trigger" "trigger" {
  name = "trigger-name"

  user_defined_parameters = <<EOF
                        [
                                {
                                        "key":"place",
                                        "value":"India"
                           }
                   ]
           EOF

  user_defined_annotations = <<EOF
           [
                   {
                          "key":"Description",
                           "value":"Sample code to display hello"
                  }
          ]
  EOF
}
```

#### Creating a trigger feed
```hcl
resource "ibm_function_trigger" "feedtrigger" {
  name = "alarmFeed"

  feed = [
    {
      name = "/whisk.system/alarms/alarm"

      parameters = <<EOF
                [
                        {
                                "key":"cron",
                                "value":"0 */2 * * *"
                        }
                ]
                EOF
    },
  ]

  user_defined_annotations = <<EOF
                 [
         {
                 "key":"sample trigger",
                 "value":"Trigger for hello action"
         }
                 ]
                 EOF
}
```


### Input parameters

The following arguments are supported:

* `name` - (Required, string) The name of the trigger.
* `feed` - (Optional, list) A nested block to describe the feed. Nested `feed` blocks have the following structure:
    * `name` - (Required, string) Trigger feed `ACTION_NAME`.
    * `parameters` - (Optional, string) Parameters definitions in key value format. Parameter bindings are included in the context and passed when the action is invoked.
* `user_defined_annotations` - (Optional, string) Annotation definitions in key value format.
* `user_defined_parameters` - (Optional, string) Parameters definitions in key value format. Parameter bindings are included in the context and passed to the trigger.

### Output parameters

The following attributes are exported:

* `id` - The ID of the new trigger.
* `publish` - Trigger visibility.
* `version` - Semantic version of the item.
* `annotations` - All annotations to describe the trigger, including those set by you or by IBM Cloud Functions.
* `parameters` - All parameters passed to the trigger, including those set by you or by IBM Cloud Functions.

### Import

`ibm_function_trigger` can be imported using the ID.

Example:

```
$ terraform import ibm_function_trigger.trigger alaram

```
