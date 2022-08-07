---
sidebar_position: 2
title: PingCAP Clinic Release Note
---
## August 4, 2022 - GA

**Clinic Service**

- [Added] Provide Comparison Report which can compare the key metrics between two data packages for a cluster. 

## July 15, 2022 - GA

**Clinic Service**

- [Added] Provide the Health Report feature for each diagnosis data package. It can quickly check metrics and logs with 58 rules. For more details about rules, see [Health rule release note](https://clinic-docs-en.vercel.app/docs/health-rule-release-note).

**Diag v1.0.0**

- [Added] Add cluster name into the default name of the collected data folder.
- [Fixed] Fix the display issue of the Diag config result.

## July 11, 2022

**Clinic Service**

- [Added] Support SSO login to the [Clinic.us](https://clinic.pingcap.com/) with TiDB Cloud account.
- [Fixed] Fix the issue that the rebuild process shows 0% in some special cases.

**Diag v0.9.0**

- [Added] Support setting region for [Clinic.us](https://clinic.pingcap.com/) and [Clinic.cn](https://clinic.pingcap.com.cn/).
- [Fixed] Fix the data collection issue for the old versions of the Prometheus server.

## June 28, 2022

**Diag v0.8.2**

- [Fixed] Fix the issue that metrics collection lost data when the data collection time is long (more than 2 hours) in  v0.8.1 and v0.8.0.
- [Fixed] Fix the upload error "EOF" in Diag after collecting data from some clusters.

## June 24, 2022

**Clinic Service**

- [Added] Support a "Jump to" button to historical packages when multiple uploaded data packages in a cluster.
- [Added] Support **Mine** and **Starred** buttons on the Organization page, which helps quickly find your own or starred organizations.
- [Added] Optimize the rebuild process to make data reconstruction smoother.

**Diag v0.8.0**

- [Added] Full support for TiFlash log collection.
- [Added] Log collection supports refined classification, for example, support `--exclude=log.slow` to exclude slow log collection (sensitive data scenarios).
- [Added] [Experimental feature] Support collection of SQL query plan related information.
