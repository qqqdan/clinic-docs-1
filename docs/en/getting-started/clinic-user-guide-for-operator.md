---
sidebar_position: 4
title: Use PingCAP Clinic For Operator Environment
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Clinic Diagnostic Service Operation Manual for Operator Environment

For clusters deployed using TiDB Operator, Clinic Diagnostic Service can perform data collection and cluster rapid diagnosis on the cluster through Clinic Diag diagnostic client side and Clinic Server Cloud as a Service.

:::info Note
This document **only** applies to clusters deployed with TiDB Operator. To view clusters for TiUP deployments, see the [Clinic Operations Manual for TiUP Environments](/clinic-user-guide-for-tiup.md).
Clinic Diagnostics Service temporarily **does not support** data collection on clusters deployed by TiDB Ansible.
:::info

For clusters deployed using TiDB Operator, Clinic Diag needs to be deployed as a standalone Pod. This article describes how to use the kubectl command to create and deploy a Diag pod, then continue data collection and quick checks through API calls.

## Use scenario

Through Clinic Diag client, you can easily and quickly obtain diagnostic data from clusters:

- [Use Clinic Diag to collect diagnostic data] ( #use-clinical-diag-to-ollect-diagnostic-data)
- [Quickly diagnose clusters with Clinic Diag] ( #quickly-diagnose-clusters-with-clinical-diag)

## Use Clinic Diag to collect diagnostic data

This section details the steps to use the Clinic Diag client.

### Step 1: Prepare the environment

Before deploying Clinic Diag, please review the following software requirements:

* Kubernetes v1.12 or later
* [TiDB Operator](https://docs.pingcap.com/zh/tidb-in-kubernetes/stable/tidb-operator-overview)
* [PersistentVolume](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
* [RBAC](https://kubernetes.io/docs/admin/authorization/rbac)
* [Helm 3](https://helm.sh)

#### Install Helm

Refer to [Use Helm](https://helm.sh/docs/intro/install/) to install Helm and configure PingCAP official chart repository.

#### Check the permissions of the deployment user

During the Diag deployment process, you need to create *Role* and *Cluster Role* with the following permissions. The user who needs to deploy Diag should have permission to create this type of *Role* and *Cluster Role*.

```
PolicyRule:
  Resources                 Non-Resource URLs  Resource Names  Verbs
  ---------                 -----------------  --------------  -----
  pods                      []                 []              [get list]
  secrets                   []                 []              [get list]
  services                  []                 []              [get list]
  tidbclusters.pingcap.com  []                 []              [get list]
  tidbmonitors.pingcap.com  []                 []              [get list]
```
:::info Note
- Smaller permissions can be used if the cluster situation qualifies for least privilege deployment. See [Least Privilege Deployment](#step-3-:-deploy-the-clinic-diag-pod) for details.
:::info

### Step 2: Log in to Clinic Server to get Clinic Token

Clinic Token is used for user authentication when Diag client uploads data，it can ensure that the data is uploaded to the right organization. You need to sign-up and login to Clinic Server to get the Token.

#### Register and log in to Clinic Server

Enter the [Clinic login page] ( https://clinic.pingcap.com.cn/portal/#/login ), select "Sign in with AskTUG", and you can log in PingCAP Clinic services through your TiDB community account. If you don't have a TiDB community account yet, you can register at the login interface.

#### Create Organization

After the user successfully logs in for the first time, an organization needs to be created. Enter the organization name according to the page prompts to create it. After the creation is successful, enter the organization page. The user can upload diagnostic data through Diag client after obtaining the Token.

#### Get client side upload Token
Click the upload icon on the page, select "Get Access Token For Diag Tool", copy and save the Token information in the pop-up window.

 ![Get Token](/img/getting-started/get-token.png)

:::info Note
- Token content is only displayed when it is created. If the user loses the Token information, the old Token can be deleted and recreated.
:::info

### Step 3: Deploy the Clinic Diag Pod

Depending on the network connectivity of the cluster, you can choose to deploy the Clinic Diag Pod in the following ways:

- Online rapid deployment: If the network where the cluster is located can access the Internet and use the default configuration parameters, it is recommended to use this method.
- Online ordinary deployment: If the network where the cluster is located can access the Internet, you need to customize the configuration parameters of the Diag Pod. It is recommended to use this method.
- Offline deployment: If the network where the cluster is located cannot access the Internet, offline deployment can be used.
- Least privilege deployment: If all nodes of the target cluster are in the same namespace, Diag can be deployed to the namespace where the target cluster is located to achieve least privilege deployment.

<Tabs>
<TabItem value="Online rapid deployment" label="Online rapid deployment" default>

  1. Deploy Clinic Diag with the following helm command, the latest Diag image will be downloaded from Docker Hub

    ```shell
    helm install --namespace tidb-admin diag-collector pingcap/diag \
          --set diag.clinicToken=${clinic_token }
    ```

    :::info Note
    If accessing Docker Hub is slow, you can use the image on Alibaba Cloud:

    ```shell
    helm install --namespace tidb-admin diag-collector pingcap/diag --version v0.7.0 \
         --set image.diagImage=registry.cn-beijing.aliyuncs.com/tidb/diag \
         --set diag.clinicToken= ${clinic_token }
    ```
    :::info


  2. After deployment, return the following:

    ```
    NAME: diag-collector
    LAST DEPLOYED: Tue Mar 15 13:00:44 2022
    NAMESPACE: tidb-admin
    STATUS: deployed
    REVISION: 1
    NOTES:
    Make sure diag-collector components are running:

      kubectl get pods --namespace tidb-admin -l app.kubernetes.io/instance=diag-collector
      kubectl get svc --namespace tidb-admin -l app.kubernetes.io/name=diag-collector

    ```

</TabItem>
<TabItem value="Online ordinary deployment" label="Online ordinary deployment">
  1. Get the values-diag-collector.yaml file in the Clinic Diag chart you want to deploy:

    ```shell
    mkdir -p ${HOME}/diag-collector && \
    helm inspect values pingcap/diag --version=${chart_version} > ${HOME}/diag-collector/values-diag-collector.yaml
    ```

    :::info Note
    `${chart_version}` represents the chart version in subsequent docs, such as `v0.7.0`, you can check the currently supported version by
    `helm search repo -l diag`.
    :::info

  2. configuration values-diag-collector.yaml file

    Modify the `${HOME}/dialog-collector/values-diag-collector.yaml` file to set your Clinic Token.

    Other items such as: `limits`, `requests` and `volume `, please modify as needed.

    :::info Note
    - Please refer to the content of [Step 2: Log in to Clinic Server to obtain Clinic Token] (#Step-2-:-log-in-to-clinic-server-to-get-clinic-token) to obtain Token.
    - To deploy `diagonal-collector`, the`pingcap/diag` image will be used. If the image cannot be downloaded from docker hub, you can modify the `image.diag Image `in the` ${HOME}/diagonal-collector/values-diag-collector.yaml `file to registry.cn-beijing.aliyuncs.com/tidb/diag`.
    :::info

  3. Deploy Clinic Diag

    ```shell
    helm install diag-collector pingcap/diag --namespace=tidb-admin --version=${chart_version} -f ${HOME}/diag-collector/values-diag-collector.yaml && \
    kubectl get pods --namespace tidb-admin -l app.kubernetes.io/instance=diag-collector
    ```

    :::info Note
    - The namespace should be set to the same as the TiDB Operator. If the TiDB Operator is not deployed, please deploy the TiDB Operator before deploying the Clinic Diag.
    :::info

  4. [Optional Action] Set persistent data volume

    This operation mounts data volumes for Diag to provide the ability to persist data
    Modify the `${HOME}/dialog-collector/values-diag-collector.yaml` file, the configuration diag.volume field can select the required volume

    Example:

    ```
    # PVC type is used
      volume:
        persistent volumes claim:
          claimName: local-storage-diag
    ```
    ```
    # host type is used
      volume:
        hostPath:
          path: /data/diag
    ```
    :::info Note
    - Multi-disk mount is not supported
    - Support any type of StorageClass
    :::info

</TabItem>
<TabItem value="Offline deployment" label="Offline deployment">

  If the server does not have an extranet, follow the steps below to install Clinic Diag offline:

  1. Download the `Clinic diag `chart

    If there is no extranet on the server, the Clinic diag component and other applications cannot be installed through the configuration Helm repo. At this time, you need to download the chart file required for cluster installation on the machine with extranet, and then copy it to the server.

    Download the `Clinic diag `chart file with the following command:

    ```shell
    wget http://charts.pingcap.org/diag-v0.7.1.tgz
    ```

    Copy the diag-v0.7.1.tgz file to the server and extract it to the current directory:

    ```shell
    tar zxvf diag-v0.7.1.tgz
    ```
  2. Download the Docker image required for Clinic Diag to run

    You need to download the Docker image used by Clinic Diag on the machine with extranet and upload it to the server, and then use `docker load` to install the Docker image to the server.

    The Docker images used by TiDB Operator are:

    ```shell
    pingcap / diagram: v0.7.1
    ```

    Next download the image with the following command:

    ```shell
    docker pull pingcap/diag:v0.7.1
    docker save -o diag-v0.7.1.tar pingcap / diagram: v0.7.1
    ```

    Next upload these Docker images to the server and execute `docker load` to install these Docker images on the server:

    ```shell
    docker load -i diag-v0.7.1.tar
    ```
  3. Configuration Clinic Diag

    Modify the `${HOME}/dialog-collector/values-diag-collector.yaml` file to set your Clinic Token.

    Other items such as: `limits`, `requests` and`volume `, please modify as needed.

    :::info Note
    - Please refer to the content of [Step 2: Log in to Clinic Server to obtain Clinic Token] (#Step-2-:-log-in-to-clinic-server-to-get-clinic-token) to obtain Token.
    :::info

  4. Install Clinic Diag

  Install Clinic Diag with the following command:

    ```shell
    helm install diag-collector ./diag --namespace=tidb-admin
    ```

    :::info Note
    The namespace should be set to the same as the TiDB Operator. If the TiDB Operator is not deployed, please deploy the TiDB Operator first and then deploy Clinic Diag.
    :::info

  5. [Optional Action] Set persistent data volume

    This operation mounts data volumes for Diag to provide the ability to persist data
    Modify the `${HOME}/dialog-collector/values-diag-collector.yaml` file, the configuration diag.volume field can select the required volume

    Example:

    ```
    # PVC type is used
      volume:
        persistent volumes claim:
          claimName: local-storage-diag
    ```
    ```
    # host type is used
      volume:
        hostPath:
          path: /data/diag
    ```
    :::info Note
    - Multi-disk mount is not supported
    - Support any type of StorageClass
    :::info

</TabItem>

<TabItem value="Least privilege deployment" label="Least privilege deployment" default>
 :::info Note
  - This deployment method deploys Diag to the namespace where the target cluster is located. Diag can only collect data in the namespace, and cannot collect data across namespaces.
  :::info

  1. Confirm the permissions of the deployment user

    A least-privilege deployment will create a Role in the deployed namespace with the following permissions. The user used for the deployment needs to have permission to create this type of *Role* in the namespace.

    ```
    PolicyRule:
      Resources                 Non-Resource URLs  Resource Names  Verbs
      ---------                 -----------------  --------------  -----
      pods                      []                 []              [get list]
      secrets                   []                 []              [get list]
      services                  []                 []              [get list]
      tidbclusters.pingcap.com  []                 []              [get list]
      tidbmonitors.pingcap.com  []                 []              [get list]
    ```

 2. Deploy Clinic Diag with the following helm command, the latest Diag image will be downloaded from Docker Hub

    ```shell
    helm install --namespace tidb-cluster diag-collector pingcap/diag \
          --set diag.clinicToken=${clinic_token} \
          --set diag.clusterRoleEnabled=false
    ```
    :::info Note
    - If the cluster does not have TLS enabled, you can set `diag.tls Enabled = false`, and the Role created at this time will not have`get ` and `list` permissions for `secrets`.

      ```shell
      helm install --namespace tidb-cluster diag-collector pingcap/diag \
            --set diag.clinicToken=${clinic_token} \
            --set diag.tlsEnabled=false \
            --set diag.clusterRoleEnabled=false
      ```
    - If accessing Docker Hub is slow, you can use the image on Alibaba Cloud:

      ```shell
      helm install --namespace tidb-cluster diag-collector pingcap/diag --version v0.7.0 \
          --set image.diagImage=registry.cn-beijing.aliyuncs.com/tidb/diag \
          --set diag.clinicToken= ${clinic_token} \
          --set diag.clusterRoleEnabled=false
      ```
    :::info

  3. After deployment, return the following:

    ```
    NAME: diag-collector
    LAST DEPLOYED: Tue Mar 15 13:00:44 2022
    NAMESPACE: tidb-cluster
    STATUS: deployed
    REVISION: 1
    NOTES:
    Make sure diag-collector components are running:
      kubectl get pods --namespace tidb-cluster -l app.kubernetes.io/instance=diag-collector
      kubectl get svc --namespace tidb-cluster -l app.kubernetes.io/name=diag-collector
    ```

</TabItem>
</Tabs>

### Step 4: Check the running status of the Clinic Diag Pod:

Query the Diag status with the following command:

  ```shell
  kubectl get pods --namespace tidb-admin -l app.kubernetes.io/instance=diag-collector
  ```
The output of a functioning Pod is as follows:
  ```
  NAME                             READY   STATUS    RESTARTS   AGE
  diag-collector-5c9d8968c-clnfr   1/1     Running   0          89s
  ```

## Use Clinic Diag to collect diagnostic data

Clinic Diag can quickly capture diagnostic data of TiDB clusters, including monitoring data, configuration information, and more.

### Use scenario:

The following scenarios apply to the collection of diagnostic data using Clinic Diag:

- When there is a problem with the cluster and you want to consult PingCAP technical support, you need to provide cluster diagnostic data to assist technical support for locating the problem.
- Retain cluster diagnostic data for later analysis.

:::info Note
For clusters deployed using TiDB Operator, the collection of diagnostic data such as logs, configuration files, and system hardware information is not currently supported.
:::info

### Step 1: Determine what data needs to be collected

For a detailed list of data collected by Clinic Diag, see [Clinic data collection Instructions - Operator Environment](https://clinic-docs.vercel.app/docs/getting-started/clinic-data-instruction-for-operator). It is recommended to collect complete monitoring data in order to improve diagnostic efficiency.

### Step 2: Collect data

All operations of the Clinic Diag tool are done through the API.

- To view the full API definition doc, visit the node `http://${host}:${port}/api/v1`.

- To view the node IP, use the following command:

  ```bash
  kubectl get node | grep node
  ```

- To view the port number of the `diagonal-collector service`, use the following command:

  ```bash
  kubectl get service -n tidb-admin
  NAME                 TYPE           CLUSTER-IP           EXTERNAL-IP   PORT(S)              AGE
  diag-collector   NodePort   10.111.143.227   <none>            4917:31917/TCP   18m
  ```

  - The port to access the Service from outside the Kubernetes cluster is `31917`.
  - The Service type is NodePort. You can access the service by Kubernetes the IP address `${host}` and port number `${port}` of any host in the cluster.

#### 1. Initiate a data collection request

Initiate a data collection task via API request:

```bash
curl -s http://${host}:${port}/api/v1/collectors -X POST -d '{"clusterName": "${cluster-name}","namespace": "${cluster-namespace}","from": "2022-02-08 12:00 +0800","to": "2022-02-08 18:00 +0800"}'
```
API call parameter description:

- `clusterName`: TiDB cluster name
- `Namespace`: the`namespace name` where the TiDB cluster is located (not the`namespace` where the TiDB Operator is located)
- `Collector`: optional parameter, you can configure the data type to be collected, support [monitor, config, perf]. If this parameter is not configured, monitor and config data will be collected by default.
- `From` and`to `: The start and end times of collection, respectively. `+ 0800`represents the time zone, and the supported time formats are as follows:



  ```bash
  "2006-01-02T15:04:05Z07:00"
  "2006-01-02T15:04:05.999999999Z07:00"
  "2006-01-02 15:04:05 -0700",
  "2006-01-02 15:04 -0700",
  "2006-01-02 15 -0700",
  "2006-01-02 -0700",
  "2006-01-02 15:04:05",
  "2006-01-02 15:04",
  "2006-01-02 15",
  "2006-01-02",
  ```

  An example of the command output result is as follows:

  ```bash
      "clusterName": "${cluster-namespace}/${cluster-name}",
      "collectors"            "config",
          "monitor"
      ],
      "date": "2021-12-10T10:10:54Z",
      "from": "2021-12-08 12:00 +0800",
      "id": "fMcXDZ4hNzs",
      "status": "accepted",
      "to": "2021-12-08 18:00 +0800"

  ```
API return information description:
- `Date`: The time when the acquisition task was initiated.
- `Id`: The ID number of this task. In subsequent operations, this ID is the only information to locate this task.
- `Status` The current status of this task, `accepted` means the acquisition task is queued.

:::info Note
Returning the command result only means that the data collection task has started, not that the collection has been completed. To know if the acquisition is all complete, you need to check the status of the acquisition task through the next step.
:::info

#### 2. View the status of the collection data task

Get the status of the acquisition task through an API request:

```bash
curl -s http://${host}:${port}/api/v1/collectors/${id}
{
            "clusterName": "${cluster-namespace}/${cluster-name}",
        "collectors": [
            "config",
            "monitor"
        ],
        "date": "2021-12-10T10:10:54Z",
        "from": "2021-12-08 12:00 +0800",
        "id": "fMcXDZ4hNzs",
        "status": "finished",
        "to": "2021-12-08 18:00 +0800"
}
```
Where`id `is the ID number of the task, in the above example`fMcXDZ4hNzs`. The return format of the command in this step is the same as that in the previous step.

If the status of the task changes to`finished `, the data collection is complete.

#### 3. View collected dataset information

After completing the collection task, you can obtain the collection time and data size information of the dataset through the API request:

```bash
curl -s http://${host}:${port}/api/v1/data/${id}
{
        "clusterName": "${cluster-namespace}/${cluster-name}",
        "date": "2021-12-10T10:10:54Z",
        "id": "fMcXDZ4hNzs",
        "size": 1788980746
}
```
With this command, **can only** view the file package size of the dataset, not the specific data.

### Step 3: Upload the dataset

When providing diagnostic data to PingCAP technical support staff, the data needs to be uploaded to Clinic Server and then its data link sent to the technical support staff. Clinic Server is Cloud as a Service for Clinic Diagnostic Services, which provides more secure diagnostic data storage and sharing.

#### 1. Initiate an upload task

Package and upload the collected dataset via API request:

```bash
curl -s http://${host}:${port}/api/v1/data/${id}/upload -XPOST
{
        "date": "2021-12-10T11:26:39Z",
        "id": "fMcXDZ4hNzs",
        "status": "accepted"
}
```
Returning the command result only means that the upload task has started, and does not mean that the upload has been completed. To know if the upload task is complete, you need to go through the next step to check the task status.

#### 2. View upload task status

View the status of the uploaded task via API request:

```bash
curl -s http://${host}:${port}/api/v1/data/${id}/upload
{
        "date": "2021-12-10T10:23:36Z",
        "id": "fMcXDZ4hNzs",
        "result": "\"https://clinic.pingcap.com:4433/diag/files?uuid=ac6083f81cddf15f-34e3b09da42f74ec-ec4177dce5f3fc70\"",
        "status": "finished"
}
```

If the status changes to`finished `, both packaging and uploading are complete. At this time,` result `means that Clinic Server views the link to this dataset, that is, the data access link that needs to be sent to PingCAP technical support personnel.

### Optional action: View data locally

The collected data will be saved in the Pod's '/diag/collector/diag- ${id}' directory. You can enter the Pod to view this data by the following methods:

#### 1. Get `diag-collector-pod-name`

```bash
kubectl get pod --all-namespaces  | grep diag
tidb-admin      diag-collector-69bf78478c-nvt47               1/1     Running            0          19h
```

Among them, the name of the Diag Pod is `dialog-collector-69bf78478c-nvt47`, and the `namespace` is `tidb-admin `.

#### 2. Enter the Pod and view the data

```bash
kubectl exec -n ${namespace} ${diag-collector-pod-name}  -it -- sh
cd  /diag/collector/diag-${id}
```
Among them, `${namespace}` needs to be replaced with the name of the `namespace `where the TiDB Operator is located (usually`tidb-admin`).

## Quickly diagnose clusters with the Clinic Diag tool

Clinic diagnostic service supports rapid diagnosis of the health status of the cluster, mainly supports checking the content of configuration items, and quickly discovers unreasonable configuration items.

### Use steps

This section details how to quickly diagnose clusters deployed using TiDB Operator through the Clinic Diagnostic Service.

Step 1: Collect data

For specific methods of collecting data, please refer to previouse content.

Step 2: Quick Diagnosis

Quick diagnosis of the cluster locally via API requests:

```bash
curl -s http://${host}:${port}/api/v1/data/${id}/check -XPOST -d '{"types": ["config"]}'
```

Among them, `id` is the ID number of the data collection task, which is `fMcXDZ4hNzs` in the above example.

The found configuration risk content and the knowledge base link of the recommended configuration will be listed in the request result. The specific examples are as follows:

```bash
stdout:
# Check Result Report
basic 2022-02-07T12:00:00+08:00

## 1. Cluster Information
- Cluster ID: 7039963340562527412
- Cluster Name: basic
- Cluster Version: v5.4.0

## 2. Sample Information
- Sample ID: fPrz0RnDxRn
- Sampling Date: 2022-02-07T12:00:00+08:00
- Sample Content:: [monitor config]

## 3. Main results and abnormalities
In this inspection, 21 rules were executed.
The results of **3** rules were abnormal and needed to be further discussed with support team.
The following is the details of the abnormalities.

### Configuration Summary
The configuration rules are all derived from PingCAP’s OnCall Service.
If the results of the configuration rules are found to be abnormal, they may cause the cluster to fail.
There were **3** abnormal results.

#### Rule Name: tidb-max-days
- RuleID: 100
- Variation: TidbConfig.log.file.max-days
- For more information, please visit: https://s.tidb.io/msmo6awg
- Check Result:
  TidbConfig_172.20.21.213:4000   TidbConfig.log.file.max-days:0   warning

#### Rule Name: pdconfig-max-days
- RuleID: 209
- Variation: PdConfig.log.file.max-days
- For more information, please visit: https://s.tidb.io/jkdqxudq
- Check Result:
  PdConfig_172.20.22.100:2379   PdConfig.log.file.max-days:0   warning
  PdConfig_172.20.14.102:2379   PdConfig.log.file.max-days:0   warning
  PdConfig_172.20.15.222:2379   PdConfig.log.file.max-days:0   warning

#### Rule Name: pdconfig-max-backups
- RuleID: 210
- Variation: PdConfig.log.file.max-backups
- For more information, please visit: https://s.tidb.io/brd9zy53
- Check Result:
  PdConfig_172.20.22.100:2379   PdConfig.log.file.max-backups:0   warning
  PdConfig_172.20.14.102:2379   PdConfig.log.file.max-backups:0   warning
  PdConfig_172.20.15.222:2379   PdConfig.log.file.max-backups:0   warning

Result report and record are saved at /diag-fPrz0RnDxRn/report-220208030210
```
