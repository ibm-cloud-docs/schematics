---

copyright:
  years: 2017, 2022
lastupdated: "2022-12-19"

keywords: schematics blueprints, operate blueprint, managed environments

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Blueprints is a [beta feature](/docs/schematics?topic=schematics-bp-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations](/docs/schematics?topic=schematics-bp-beta-limitations#sc-bp-beta-limitation) for the beta release.
{: beta}

# Blueprint versioning
{: #blueprint-versioning}

Blueprints supports two update approaches to change to templates, modules and input. Either a relaxed 'development' mode where the current version of a template or module is always pulled, or a 'production' mode where an explicit version is specified. 

## Relaxed versioning
{: #update-blueprint-relaxed} 

In some development environments, or where IaC code and blueprint templates are being developed, versioning of IaC definitions is not needed. 

Blueprints defaults to always pull the current template, inputs, or modules on an update operation if no versioning is specified. Alternatively template and module statements can use the `git_release` option `latest`. When `latest` is in effect, {{site.data.keyword.bpshort}} identifies if the blueprint template, or module Git repositories are updated, and runs a `pull latest` to update the blueprint configuration. 

```sh
ibmcloud schematics blueprint update -id <blueprint_ID> 
```
{: pre}

## Explicit versioning
{: #update-blueprint-strict} 

If explicit blueprint versioning is used with Git release tags for each blueprint template release, the blueprint configuration is only updated if a new release tag is specified on the Update operation. 

```sh
ibmcloud schematics blueprint update --id <blueprint_ID> --bp-git-release x.y.z 
```
{: pre}

Update the input file source and push a new release to its Git source repository. 

If explicit input file version is used with release tags for each input file release, the blueprint configuration must be updated in {{site.data.keyword.bpshort}} with the new input file release tag.  

```sh
ibmcloud schematics blueprint update --id <blueprint_ID> --input-git-release x.y.z  
```
{: pre} 