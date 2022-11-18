---

copyright:
  years: 2017, 2022
lastupdated: "2022-11-18"

keywords: schematics agents connect, connect agent, register agent

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

{{site.data.keyword.bpshort}} Agents is a [beta feature](/docs/schematics?topic=schematics-agent-beta-limitations) that is available for evaluation and testing purposes. It is not intended for production usage. Refer to the list of [limitations for Agents](/docs/schematics?topic=schematics-agent-beta-limitations) in the beta release.
{: beta}

# Using {{site.data.keyword.bpshort}} Agent
{: #using-agent}

You have successfully connected the Agent to {{site.data.keyword.bpshort}} service instance. The next step is to bind your {{site.data.keyword.bpshort}} Workspaces to the Agent.
{: shortdesc}

Once you bind the workspace to the Agent, then the corresponding workspace Jobs such as `terraform plan`, `terraform apply`, or `terraform destroy` is automated to route to the Agent. 

In other words, the Terraform automation runs in your provisioned Agent infrastructure (cluster). Then the {{site.data.keyword.bpshort}} Workspace bounds to the Agent in the following ways:
- Bind an new workspace to the Agent
   When you bind the new workspace to the Agent, the Terraform templates are downloaded from the Git repositories by using Sandbox jobs that run in your Agent infrastructure (cluster). Further, the Terraform jobs are also run in your cluster.

- Bind the existing workspace to the Agent
   When you bind an existing workspace to the Agent, the Terraform templates may be downloaded by the shared {{site.data.keyword.bpshort}} service instance. However, the subsequent template operations such as, update the terraform template from the Git repositories will be done by using the Sandbox jobs that run in your Agent infrastructure (cluster). Further, the Terraform jobs are also run in your Cluster.

## Steps to Bind an existing workspace to the Agent
{: #steps-bind-exist-wks}

   1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/){: external} account by using your credentials.
   2. Navigate to **{{site.data.keyword.bpshort}}** > **Agents**.
   3. Select your Agent from the list, and use the `...` dots to perform **Bind Agent** operation.
   4. In the side navigation pane, select **workspaces** to be bound statically to the Agent.
   5. Select the bounded Agent to view the list of related workspaces.

## Steps to Bind a new workspace to the Agent
{: #steps-bind-new-wks}

   1. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com/workspaces){: external} account by using your credentials.
   2. Click [**Create workspace +**](https://cloud.ibm.com/schematics/workspaces/create){: external}.
      - In **Specify Template** section:
         - **GitHub, GitLab or `Bitbucket` repository URL** - `https://github.com/Cloud-Schematics/cos-module`.
         - **Personal access token** - `<leave it blank>`.
         - Terraform Version - `terraform_v1.0`. You need to select Terraform version 1.0 or greater version.
         - Click **Bind Agent**. Choose your **Agent name** to bind to execute the Jobs.
         - Click `Next`.
      - In **Workspace details** section:
         - **Workspace name** as `Provisioning-wks-through-myagent`.
         - **Tags** as `myagent`.
         - **Resource group** as `default` or other resource group for this workspace. 
         - **Location** as `North America` or other [region](/docs/schematics?topic=schematics-multi-region-deployment) for this workspace.
               If the location used for Agent infrastructure and Agent service does not match, then the logs are not sent to LogDNA.
            {: note}

         - Click `Next`.
         - Check the information entered are correct to create a workspace.
      - Click `Create`.
   3. On successful creation of `Provisioning-wks-through-myagent` workspace. 
   4. Click **Settings** to edit the following input variables in the workspace. For more information about the input variable, see [Readme](https://github.com/Cloud-Schematics/cos-module/blob/main/README.md){: external} file.
      - `region` - `<your region where you want to deploy the VPC, for example, us-south>` and 
      - `prefix` - `<The prefix that you would like to append your resource, for example, mytest>` 
   5. Click **Apply plan** on the `Provisioning-wks-through-myagent` workspace to deploy the Agent service. Wait 5 - 15 minutes to complete the job execution.

## Validate the Job execution by the Agent
{: #validate-agent-job}

After the workspace is bound to an Agent, you can validate whether the Jobs are being run by the Agent. From your `Provisioning-wks-through-myagent` workspace view the Jobs log to see the following messages that signifies the Sandbox job or the Terraform jobs are run by the Agent.
```text
.....
Related workspace: name=<prefix>-003-cos-module, sourcerelease=(not specified), sourceurl=https://github.com/Cloud-Schematics/cos-module, folder=.
--- Ready to execute the command on Agent jobrunner-89b7f499-slhf9 ---
.....
```
{: screen}

- Now, you are ready to use the provisioned COS service to store your objects or files. 
- You can also deploy your {{site.data.keyword.bpshort}} Workspaces template, and bind your Agent to provision, install the software, or a tool in your private network to the {{site.data.keyword.bplong_notm}} service.
- You can also configure, and operate on your cloud cluster resources without any time, network, or software restrictions.

## Next steps
{: #agent-using-nextsteps}

You have completed the entire {{site.data.keyword.bpshort}} Agent set up and working flow.
- Looking for Agents samples? Check out the [{{site.data.keyword.bplong_notm}} GitHub repository](https://github.com/Cloud-Schematics?q=Agent&type=all&language=&sort=){: external}.
- For any challenges in Agents set up, see [FAQ about Agent](/docs/schematics?topic=schematics-faqs-agent) and [Troubleshooting guide](/docs/schematics?topic=schematics-agent-crn-not-found).
