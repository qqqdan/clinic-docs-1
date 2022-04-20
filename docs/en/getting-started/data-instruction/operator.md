---
title: Operator 
summary: Learn what diagnostic data can be collected by PingCAP Clinic Diagnostic Service from the TiDB clusters deployed using Operator.
---

# PingCAP Clinic Diagnostic Data - Operator

This document provides the types of diagnostic data that can be collected by PingCAP Clinic Diagnostic Service (PingCAP Clinic) from the TiDB and DM clusters deployed using Operator. Also, the document lists the parameters for data collection corresponding to each data type. When running a command to [collect data using Diag client (Diag)](https://clinic-docs.vercel.app/docs/getting-started/clinic-user-guide-for-operator), you can add the required parameters to the command according to the types of the data to be collected.

The diagnostic data collected by PingCAP Clinic is **only** used for troubleshooting cluster problems.

Clinic Server is a diagnostic service deployed in the cloud. Currently, you can upload the collected diagnostic data to [Clinic Server China](https://clinic.pingcap.com.cn) only. The uploaded data is stored in the AWS S3 China (Beijing) region server set up by PingCAP. Clinic Server Global will be provided soon with a new URL and data storage location. 

PingCAP strictly controls permissions for data access and only allows authorized in-house technical support staff to access the uploaded data.

### TiDB cluster information

| Data type | Exported file | Parameter for data collection by PingCAP Clinic |
| :------ | :------ |:-------- |
| Basic information of the cluster, including the cluster ID | `cluster.json` | The data is collected per run by default. |
| Detailed information of the cluster | `meta.yaml` | The data is collected per run by default. |

### TiDB diagnostic data

| Data type | Exported file | Parameter for data collection by PingCAP Clinic |
| :------ | :------ |:-------- |
| Real-time configuration | `config.json` | `collectors:config` |
| Performance data | `cpu_profile.proto`, `mem_heap.proto`, `goroutine.txt`, `mutex.txt` | `collectors:perf` |

### TiKV diagnostic data

| Data type | Exported file | Parameter for data collection by PingCAP Clinic |
| :------ | :------ |:-------- |
| Real-time configuration | `config.json` |  `collectors:config` |
| Performance data | `cpu_profile.proto` | `collectors:perf` |

### PD diagnostic data

| Data type | Exported file | Parameter for data collection by PingCAP Clinic |
| :------ | :------ |:-------- |
| Real-time configuration | `config.json` | `collectors:config`|
| Outputs of the command `tiup ctl pd -u http://${pd IP}:${PORT} store` | `store.json` | `collectors:config` |
| Outputs of the command `tiup ctl pd -u http://${pd IP}:${PORT} config placement-rules show` | `placement-rule.json` |`collectors:config` |
| Performance data | `cpu_profile.proto`, `mem_heap.proto`, `goroutine.txt`, `mutex.txt` | `collectors:perf`  |

### TiFlash diagnostic data

| Data type | Exported file | Parameter for data collection by PingCAP Clinic |
| :------ | :------ |:-------- |
| Real-time configuration | `config.json` | `collectors:config` |
| Performance data | `cpu_profile.proto` | `collectors:perf` |

### TiCDC diagnostic data

| Data type | Exported file | Parameter for data collection by PingCAP Clinic |
| :------ | :------ |:-------- |
| Performance data | `cpu_profile.proto`, `mem_heap.proto`, `goroutine.txt`, `mutex.txt` | `collectors:perf` |
| Debug data | `info.txt`, `status.txt`, `changefeeds.txt`, `captures.txt`, `processors.txt` | `collectors:debug` |

### Prometheus monitoring data

| Data type | Exported file | Parameter for data collection by PingCAP Clinic |
| :------ | :------ |:-------- |
| All metrics data | `{metric_name}.json` | `collectors:monitor` |
| All alerts data | `alerts.json` | `collectors:monitor` |
