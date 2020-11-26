# Elasticache 

## Overview

In memory database with very high performance (sub-milliseconds), it a managed 
* Redis
  * Advanced data structure: dict, list, etc
  * Replication in AZ
  * Scale reads
  * Backup and restore
  * Transaction
* Memchached
  * Simple data structure: str, etc
  * No replication
  * Sharding 
  * No backup
  * multi-threaded

it can be used as a data cache with low latency requirements and it helps reducing database workload. It can be used to store **session data**.