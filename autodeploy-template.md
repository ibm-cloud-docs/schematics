---

copyright:
  years: 2017, 2022
lastupdated: "2022-02-08"

keywords: schematics action deployment, automation, schematics workspace,  schematics workspace creation, auto deploy

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}


# Creating an auto deploy button for {{site.data.keyword.bpshort}} actions
{: #auto-deploy-url}

Use the instructions on this page to create a button that opens the {{site.data.keyword.bpshort}} action create page and pre-populates an action name and the GitHub repository URL that stores your Ansible playbook. You can use this button to create {{site.data.keyword.bpshort}} actions more quickly. 
{: shortdesc}

For a sample button, see the `Deploy to IBM Cloud` button on the [Sample Ansible playbook for {{site.data.keyword.cloud_notm}}](/docs/schematics?topic=schematics-sample_actiontemplates) page.
{: tip}

1. Create an Ansible playbook and publish the playbook in a GitHub repository. If you do not have a playbook, you can use one of the [IBM-provided Ansible playbooks](https://github.com/Cloud-Schematics/?q=Ansible&type=&language=&sort=){: external}.
2. Copy the Git repository URL, such as `https://github.com/Cloud-Schematics/ansible-app-deploy`. 
3. Use the following syntax to create the URL to automatically pre-populate an action name and the Git repository URL on the {{site.data.keyword.bpshort}} action create page. If you do not provide the name and Git repository URL, the `Deploy to {{site.data.keyword.cloud_notm}}` link defaults to the **Create an action** page without pre-populating an action name or the Git repository URL.

    **Syntax**
    ```text
    https://cloud.ibm.com/schematics/actions/create?name=<action_name>&url=<git_repository_url>
    ```
    {: codeblock}

    **Example**
    ```text
    https://cloud.ibm.com/schematics/actions/create?name=ansible-app-deploy&url=https://github.com/Cloud-Schematics/ansible-app-deploy
    ```
    {: codeblock}

4. Open your web browser and enter the URL.
5. Verify that the {{site.data.keyword.bplong_notm}} action create page opens and that the **Action name** and **Repository URL** are pre-populated.

## Adding an image to your URL to create the auto deploy button
{: #add_an_image}

You can add an image to your URL to create your `Deploy to {{site.data.keyword.cloud_notm}}` button.

1. Use `draw.io` or any other tool to create an image for your button. Save the image in `.png` extension.
2. Create an image map. 

    **Syntax**: 
    ```html
    <img usemap="#<image_map_ID>" src="<path_to_image>"><map name="<image_map_name>" alt="<alt_text>"><area alt="<alt_text>" title="<button_title>" href="<schematics_action_url>" target="_blank" coords="<image_coordinates>" shape="rect"></map>
    ```
    {: codeblock}

    **Example**: 
    ```html
    <img usemap="#deploybutton_map" alt= "Auto deployment button"  src="images/autodeploy_button.png"><map name="deploybutton_map" alt="This image creates a Schematics action."><area alt="Deploy to IBM Cloud" title="Deploy to IBM Cloud" href="https://cloud.ibm.com/schematics/actions/create?name=ansible-is-instance-actions&url=https://github.com/Cloud-Schematics/ansible-is-instance-actions" target="_blank" coords="1,3,139,20" shape="rect"></map>
    ```
    {: codeblock}

    For a sample button, see the `Deploy to IBM Cloud` button on the [Sample Ansible playbook for {{site.data.keyword.cloud_notm}}](/docs/schematics?topic=schematics-sample_actiontemplates) page.
{: tip}


