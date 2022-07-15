---
sidebar_position: 3
title: PingCAP Clinic Health Rule Release Note
---

## v1.0.0

Release Date: July 15, 2022

**Metrics Checking Rule**

- [Added] Region size is too large
- [Added] GC process is abnormal 
- [Added] The CPU of TiDB nodes is not balanced
- [Added] The memory of TiKV nodes is not balanced
- [Added] Too many failed QPS
- [Added] StmtPrepare usage is low
- [Added] Raft-engine is not enable
- [Added] Scatter scheduler should be disabled
- [Added] TiDB CPU is high
- [Added] TiKV disk usage is high
- [Added] QPS is imbalance from applications
- [Added] QPS downward trend follows active connection counts 
- [Added] Plan cache hit rate is low
- [Added] Latency bottleneck outside TiDB 
- [Added] JDBC set recommended
- [Added] Active connection counts is not balanced
- [Added] TiKV CPU is high
- [Added] TiKV AsyncIO is not enabled
- [Added] Network latency high between TiDB and TiKV
- [Added] The CPU of TiKV nodes is not balanced
- [Added] Active connection increased while QPS not
- [Added] The latency of TiDB nodes is not balanced
- [Added] The memory of TiDB nodes is not balanced
- [Added] PD schedulers are abnormal
- [Added] Too many heartbeat messages
- [Added] Get-token is slow in TiDB
- [Added] Parsing query string is slow in TiDB
- [Added] Compiling query statement is slow in TiDB
- [Added] TiDB takes a long time to wait for tso
- [Added] tso rpc duration is high
- [Added] Pessimistic lock wait time is too long
- [Added] The wait duration of raft propose is high
- [Added] Too many MVCC versions
- [Added] TiDB coprocessor duration is high

**Log Checking Rule**

- [Added] Writing conflict in optimistic transactions
- [Added] GC life time is shorter than transaction duration
- [Added] Transaction not commit for a long time
- [Added] Data index may be inconsistent
- [Added] Writing conflict in optimistic transactions
- [Added] RocksDB write stall
- [Added] TiKV can't bootstrap since region error 
- [Added] Tasks pile up in the scheduler
- [Added] Pump has insufficient disk space
- [Added] Pump fails to notify all living drainer
- [Added] DM sync error with incorrect utf8 value
- [Added] Lightning checksum error
- [Added] Timestamp error on tidb lightning
- [Added] Lightning can't guess encoding for input file
- [Added] Opened engine files exceed the limit of tikv-importer
- [Added] Metadata is written too frequently for TiCDC
- [Added] CDC sync interrupted since buffer limit
- [Added] Canal requires old value to be enabled
- [Added] Creating changefeed failed
- [Added] Requests to TiKV coprocessor queued more than 60s
- [Added] TiDB send request timeout
- [Added] TiKV response timeout
- [Added] TiKV peer is not leader
- [Added] DM sync abnormal
- [Added] DM sync interrupted
