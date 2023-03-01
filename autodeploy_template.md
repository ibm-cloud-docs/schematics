---

copyright:
  years: 2017, 2023
lastupdated: "2023-03-01"

keywords: schematics action deployment, automation, schematics workspace,  schematics workspace creation, auto deploy

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Creating an auto deploy button for {{site.data.keyword.bpshort}} Actions
{: #auto-deploy-url}

Use the instructions to create a button that opens the {{site.data.keyword.bpshort}} Actions create page and pre-populates an action name and the GitHub repository URL that stores your Ansible playbook. You can use the button to create {{site.data.keyword.bpshort}} Actions more quickly.
{: shortdesc}

For a sample button, see the `Deploy to {{site.data.keyword.cloud}}` button on the [Sample Ansible playbook for {{site.data.keyword.cloud_notm}}](/docs/schematics?topic=schematics-sample_actiontemplates) page.
{: tip}

## Creating the deployment URL
{: #create-url}

1. Create an Ansible playbook and publish the playbook in a GitHub repository. If you do not have a playbook, you can use one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics/?q=Ansible&type=&language=&sort=){: external}.
2. Copy the Git repository URL, such as `https://github.com/Cloud-Schematics/ansible-app-deploy`.
3. Use the following syntax to create the URL to automatically pre-populate an action name and the Git repository URL on the {{site.data.keyword.bpshort}} Actions create page. If you do not provide the name and Git repository URL, the `Deploy to {{site.data.keyword.cloud_notm}}` link defaults to the **Create an action** page without pre-populating an action name or the Git repository URL.

    Syntax
    
    ```text
    https://cloud.ibm.com/schematics/actions/create?name=<action_name>&repository=<git_repository_url>
    ```
    {: codeblock}

    Example

    ```text
    https://cloud.ibm.com/schematics/actions/create?name=ansible-app-deploy&repository=https://github.com/Cloud-Schematics/ansible-app-deploy
    ```
    {: codeblock}

4. Open your web browser and enter the URL.
5. Verify that the {{site.data.keyword.bpshort}} Actions create page opens and that the **Action name** and **Repository URL** are pre-populated.

## Adding the button to a website or page
{: #add_an_image}

You can add an image to your URL to create your `Deploy to {{site.data.keyword.cloud_notm}}` button.

### Adding the button in HTML
{: #add-button-html}

To add the button in an HTML page, such as a website, copy the following code into the page where you want to position the button. Replace `<deployment URL>` with the URL for the {{site.data.keyword.bpshort}} action.
{: shortdesc}

```html
<a href="<deployment URL>" target="_blank">
  <img src="https://cloud.ibm.com/media/docs/images/icons/Deploy_to_cloud.svg" alt="Deploy to {{site.data.keyword.cloud_notm}} button">
</a>
```
{: codeblock}

### Adding the button in Markdown
{: #add-button-markdown}

To add the button into a Markdown file, such as in a repositories readme file, use the following syntax. Replace `<deployment URL>` with the URL for the deployment link.

```markdown
[![Deploy to {{site.data.keyword.cloud_notm}} button](https://cloud.ibm.com/media/docs/images/icons/Deploy_to_cloud.svg)](<deployment URL>)
```
{: codeblock}

## Next steps
{: #sample-actions-nextsteps}

Explore the [{{site.data.keyword.bpshort}} samples templates](/docs/schematics?topic=schematics-sample_actiontemplates) to deploy in your {{site.data.keyword.cloud_notm}} account.
Looking for more code examples? Check out the [samples for {{site.data.keyword.bplong_notm}} GitHub repositories](https://github.com/Cloud-Schematics?q=Ansible&type=all&language=&sort=){: external}.
