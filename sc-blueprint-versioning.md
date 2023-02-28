---

copyright:
  years: 2017, 2023
lastupdated: "2023-02-28"

keywords: schematics blueprints, operate blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Versioning blueprint environments 
{: #blueprint-versioning}

Blueprints supports two approaches to managing change to blueprint environments and their constituent templates, modules and inputs. Either a relaxed 'development' mode where the current version of a template or module is always pulled on an update operation, or a 'production' mode where an explicit version must be specified. 

## Relaxed versioning
{: #update-blueprint-relaxed} 

In some development environments, or where IaC code and blueprint templates are being developed, versioning of IaC definitions is not needed. 

Blueprints defaults to always pull the current (latest) template, inputs, or modules on an update operation if no version information is specified in the blueprint config at create time. 


### Creating environments using relaxed versioning
{: #bp-relaxed-version}

Relaxed versioning is in effect if a blueprint configuration is created without Git version or branch information. To create an environment that does not use versioning, the create command omits the `bp-git-release`, `bp-git-branch` and the corresponding input file definitions:

```sh
ibmcloud schematics blueprint create -name Blueprint_Basic -resource-group Default \
-bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-file basic-blueprint.yaml \
-input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-file basic-input.yaml 
```
{: pre}

Sample template module definition without versioning:
```yaml
source:
  source_type: github
  git: 
    git_repo_url: "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-ResourceGroup"
```
{: pre}

### Updating an un-versioned environment
{: #bp-un-versioned-env}

Blueprints defaults to always pull the current (latest) template, inputs, or modules on an update operation if no version information is specified in the blueprint config at create time. When `latest` is in effect, {{site.data.keyword.bpshort}} identifies if the blueprint template, or module Git repositories, or input files are updated, and runs a `pull latest` to update the blueprint configuration.
{: shortdesc}

```sh
ibmcloud schematics blueprint update -id <blueprint_ID> 
```
{: pre}

### Migrating to a versioned configuration
{: #bp-versioned-env}

An un-versioned environment can be updated to versioned. 

1. Add [Git release tags](https://docs.github.com/en/repositories/releasing-projects-on-github/managing-releases-in-a-repository){: external} to the Git repos to create a versioned release.   
2. Update the blueprint configuration or template with version and branch information. The flags `bp-git-release`, `bp-git-branch` and the corresponding input file definitions: 
    ```sh
    ibmcloud schematics blueprint update -id <blueprint_ID> -bp-git-release <x.y.z> -input-git-release <x.y.z>
    ```
3. Similarly version information can be added to the blueprint template to control module version selection. 

    Sample template module definition updated with module version information:
    ```yaml
    source:
      source_type: github
      git: 
        git_repo_url: "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-ResourceGroup"
        git_release: "1.2.4"
    ```
    {: pre}

## Explicit versioning
{: #update-blueprint-strict} 

### Specifying versioning at create time
{: #bp-version-create-time}

Template and input file versions are specified at create time using the flags `bp-git-release`, `bp-git-branch` and the corresponding input file flags: 

```sh
ibmcloud schematics blueprint create -name Blueprint_Basic -resource-group Default \
-bp-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -bp-git-file basic-blueprint.yaml --bp-git-release <x.y.z>\
-input-git-url https://github.com/Cloud-Schematics/blueprint-basic-example -input-git-file basic-input.yaml --input-git-release <x.y.z>
```
{: pre}

Sample initial template module definition with versioning:
```yaml
source:
  source_type: github
  git: 
    git_repo_url: "https://github.com/Cloud-Schematics/blueprint-example-modules/tree/main/IBM-ResourceGroup"
    git_release: "1.2.4"
```
{: pre}


### Specifying versions at update time
{: #bp-version-update-time}

If explicit blueprint versioning is used with Git release tags or branches for each blueprint template release, the blueprint configuration is only updated if a new release tag or branch is specified on the Update operation. The following examples illustrate using Git release or branch to update a template. 

```sh
ibmcloud schematics blueprint update --id <blueprint_ID> --bp-git-release <x.y.z>
```
{: pre}

Or 
```sh
ibmcloud schematics blueprint update --id <blueprint_ID> --bp-git-branch <production_test>
```
{: pre}


Update the input file source and push a new release to its Git source repository. 

Similarly, if explicit input file version is used with release tags for each input file release, the blueprint configuration must be updated in {{site.data.keyword.bpshort}} with the new input file release tag.  

```sh
ibmcloud schematics blueprint update --id <blueprint_ID> --input-git-release x.y.z  
```
{: pre}
