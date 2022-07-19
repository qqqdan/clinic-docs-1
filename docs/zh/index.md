---
sidebar_position: 1
title: PingCAP Clinic 诊断服务简介
summary: 介绍 PingCAP Clinic 诊断服务，包括工具组件、使用场景和工作原理。
---

# PingCAP Clinic 诊断服务文档目录

PingCAP Clinic 诊断服务（以下简称为 PingCAP Clinic）是 PingCAP 为 TiDB 集群提供的诊断服务，支持对使用 TiUP 或 TiDB Operator 部署的集群进行远程定位集群问题和本地快速检查集群状态，用于从全生命周期确保 TiDB 集群稳定运行、预测可出现的集群问题、降低问题出现概率、快速定位并修复问题。

根据数据存储的位置不同，Clinic Server 分为以下两个独立的服务：

- [Clinic Server 中国区](https://clinic.pingcap.com.cn)，数据存储在 AWS 中国区（北京）。
- [Clinic Server 美国区](https://clinic.pingcap.com)，数据存储在 AWS 美国区。

## 文档链接

Clinic 介绍文档已经在官方文档库上线，可以通过以下链接访问：

- 简介

    - [PingCAP Clinic 诊断服务简介](https://docs.pingcap.com/zh/tidb/stable/clinic-introduction)

- 在 TiUP 部署环境使用 PingCAP Clinic

    - [快速上手 PingCAP Clinic](https://docs.pingcap.com/zh/tidb/stable/quick-start-with-clinic)
    - [使用 PingCAP Clinic 诊断 TiDB 集群](https://docs.pingcap.com/zh/tidb/stable/clinic-user-guide-for-tiup)
    - [PingCAP Clinic 数据采集说明](https://docs.pingcap.com/zh/tidb/stable/clinic-data-instruction-for-tiup)

- 在 TiDB Operator 部署环境使用 PingCAP Clinic

    - [使用 PingCAP Clinic](https://docs.pingcap.com/zh/tidb-in-kubernetes/stable/clinic-user-guide)
    - [PingCAP Clinic 数据采集说明](https://docs.pingcap.com/zh/tidb-in-kubernetes/stable/clinic-data-collection)
