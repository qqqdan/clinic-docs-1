---
title: Operator Diag 数据采集范围说明
summary: 详细说明 Clinic Diag 诊断客户端会在使用 TiDB Operator 部署的集群中采集哪些诊断数据。
---

# Clinic 数据采集说明 - Operator 环境

本文提供了 PingCAP Clinic 诊断服务（以下简称为 PingCAP Clinic）在使用 Operator 部署的 TiDB 集群中能够采集的诊断数据类型，并列出了各个采集项对应的采集参数。当执行 Clinic Diag 诊断工具（以下简称为 Diag）数据采集命令时，你可以依据需要采集的数据类型，在命令中添加所需的采集参数。

通过 Clinic 诊断服务对使用 TiDB Operator 部署的集群采集的数据会**仅**用于集群问题诊断与分析。

### Cluster 基础信息

|  诊断数据类型 | 输出文件 | Clinic 采集参数 |
| :------ | :------ |:-------- |
| 集群基础信息，包括集群 ID | `cluster.json` | 每次收集默认采集 |
| 集群详细信息 | `tidbcluster.json` | 每次收集默认采集 |

### TiDB 诊断数据

|诊断数据类型 | 输出文件 | Clinic 采集参数 |
| :------ | :------ |:-------- |
| 实时配置 | `config.json` | `--include=config` |
| 性能数据| `cpu_profile.proto`, `mem_heap.proto`,`goroutine.txt`,`mutex.txt` | `--include=perf` |

### TiKV 诊断数据

|诊断数据类型 | 输出文件 | Clinic 采集参数 |
| :------ | :------ |:-------- |
| 实时配置 | `config.json` | `--include=config` |
| 性能数据| `cpu_profile.proto` | `--include=perf` |

### PD 诊断数据

|诊断数据类型 | 输出文件 | Clinic 采集参数 |
| :------ | :------ |:-------- |
| 实时配置 | `config.json` |`--include=config` |
| `tiup ctl pd -u http://${pd IP}:${PORT} store` 的输出结果 | `store.json` | `--include=config` |
| `tiup ctl pd -u http://${pd IP}:${PORT} config placement-rules show` 的输出结果 | `placement-rule.json` | `--include=config` |
| 性能数据| `cpu_profile.proto`, `mem_heap.proto`,`goroutine.txt`,`mutex.txt` | `--include=perf` |

### TiFlash 诊断数据

|诊断数据类型 | 输出文件 | Clinic 采集参数 |
| :------ | :------ |:-------- |
| 实时配置 | `config.json` |`--include=config` |
| 性能数据| `cpu_profile.proto` | `--include=perf` |

### TiCDC 诊断数据

|诊断数据类型 | 输出文件 | Clinic 采集参数 |
| :------ | :------ |:-------- |
| 实时配置 | `config.json` |`--include=config` |
| Debug数据| `info.txt`,`status.txt`,`changefeeds.txt`,`captures.txt`,`processors.txt` | `--include=debug` |

### Prometheus 监控数据

|诊断数据类型 | 输出文件 | Clinic 采集参数 |
| :------ | :------ |:-------- |
| 所有的 Metrics 数据 | `{metric_name}.json` | `--include=monitor` |
| Alert 配置 | `alerts.json` | `--include=monitor` |
