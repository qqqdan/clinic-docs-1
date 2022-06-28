---
sidebar_position: 2
title: PingCAP Clinic Release Note
---

## v0.8.2
Release Date: June 28, 2022

**Diag**
[Fixed] Fixed the problem that metrics collection lost data when the data collection time was long (more than 2 hours) in versions 0.8.1 and 0.8.0.
[Fixed] Fix the upload error "EOF"  in Diag after collecting data from some clusters

## v0.8.0
Release Date: June 24, 2022

**Clinic Service**
[Added] When a Cluster has multiple data packages uploaded, it supports directly "Jump to" historical packages
[Added] On the Org selection page, support starred and mine to help you quickly find your own or starred Org
[Added] Optimized the Rebuild process to make data reconstruction smoother.
**Diag**
[Added] Full support for TiFlash log collection
[Added] Log collection supports refined classification, for example： supports --exclude=log.slow to exclude slow log collection (sensitive data scenarios).
[Added] [Experimental feature] Support collection of SQL query plan related information