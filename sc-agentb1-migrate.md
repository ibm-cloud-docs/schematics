---

copyright:
  years: 2017, 2023
lastupdated: "2023-11-20"

keywords: schematics agent version migrate, migrate agent version, agent migrate, cli, api, ui

subcollection: schematics

---

{{site.data.keyword.attribute-definition-list}}

# Migrating agent version
{: #migrate-agent-version}

If you have already setup an agent on a {{site.data.keyword.cloud_notm}} cluster and you want to migrate the existing agent version to other supported version. For example, migrate to `1.0.0-beta1`
or `1.0.0-beta2`.

Currently, The {{site.data.keyword.bpshort}} Agent does not support agent version upgrade through CLI or API interface.
{: note}

## Migrating to `1.0.0-beta1` version
{: #migrate-v1beta1}

You can follow the steps to migrate to `1.0.0-beta1` version.

1. Browser to the target [{{site.data.keyword.cloud_notm}} cluster](https://cloud.ibm.com/kubernetes/clusters/){: external} page. Enter your `<target_iks_cluster_ID>` as part of the URL.
2. Click **Kubernetes Clusters** page.
3. Click your cluster hyper-link.
4. Click **Kubernetes dashboard**.
5. Select namespace - `schematics-job-runtime` and in `jobrunner` deployment, update the `jobrunner` container image with the tag `icr.io/schematics-remote/schematics-job-runner:3000de68-234`. Wait for the pods to be in a running state.
6. Select namespace - `schematics-runtime` and in `runtime-job` deployment, update the `runtime-job` container image with the tag `icr.io/schematics-remote/schematics-agent-ws-job:1c063cb7-453`. Wait for the pods to be in a running state.
7. Select namespace - `schematics-runtime` and in `runtime-ansible-job` deployment, update the `runtime-ansible-job` container image with the tag `icr.io/schematics-remote/schematics-ansible-job:d90d08fb-238`. Wait for the pods to be in a running state.
8. Select namespace - `schematics-sandbox` and in `sandbox deployment`, update the `sandbox` container image with the tag `icr.io/schematics-remote/schematics-sandbox:9db50fac-388`. Wait for the pods to be in a running state.
9. Select namespace - `schematics-agents-observe` and in `schematics-agents-controller-manager` deployment, update the manager container image with the tag `icr.io/schematics-remote/schematics-agent-operator:cc33f3a3-21`. Wait for the pods to be in a running state.
10. Select namespace - `schematics-agents-observe` and in `schematics-agents-log-collector` daemon set, update the `log-collector` container image with the tag `icr.io/schematics-remote/schematics-agents-log-collector:425167c7-28`. Wait for the pods to be in a running state.

## Migrating to `1.0.0-beta2` version 
{: #migrate-v1beta2}

You can follow the steps to migrate to `1.0.0-beta2` version.

1. Browser to the target [{{site.data.keyword.cloud_notm}} cluster](https://cloud.ibm.com/kubernetes/clusters/){: external} page. Enter your `<target_iks_cluster_ID>` as part of the URL.
2. Click **Kubernetes Clusters** page.
3. Click your cluster hyper-link.
4. Click **Kubernetes dashboard**.
5. Select namespace - `schematics-job-runtime` and in `jobrunner` deployment, update the `jobrunner` container image with the tag `icr.io/schematics-remote/schematics-job-runner:4be5ed21-270`. Wait for the pods to be in a running state.
6. Select namespace - `schematics-runtime` and in `runtime-job` deployment, update the `runtime-job` container image with the tag `icr.io/schematics-remote/schematics-agent-ws-job:47a850a7-481`. Wait for the pods to be in a running state.
7. Select namespace - `schematics-runtime` and in `runtime-ansible-job` deployment, update the `runtime-ansible-job` container image with the tag `icr.io/schematics-remote/schematics-ansible-job:4c01c88a-285`. Wait for the pods to be in a running state.
8. Select namespace - `schematics-sandbox` and in `sandbox deployment`, update the `sandbox` container image with the tag `icr.io/schematics-remote/schematics-sandbox:95f09e68-428`. Wait for the pods to be in a running state.
9. Select namespace - `schematics-agents-observe` and in `schematics-agents-controller-manager` deployment, update the manager container image with the tag `icr.io/schematics-remote/schematics-agent-operator:67477d11-26`. Wait for the pods to be in a running state.
10. Select namespace - `schematics-agents-observe` and in `schematics-agents-log-collector` daemon set, update the `log-collector` container image with the tag `icr.io/schematics-remote/schematics-agents-log-collector:ec6c7a42-31`. Wait for the pods to be in a running state.

Now, you are ready with the migrated version of the agent setup.

## Next steps
{: #agent-migrate-nextsteps}

- You can explore to [display an agent](/docs/schematics?topic=schematics-display-agentb1-overview&interface=cli).
- You can check out the [agent FAQ](/docs/schematics?topic=schematics-faqs-agent) for any common questions that are related to agent migration.
