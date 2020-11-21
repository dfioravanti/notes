# DynamoDB

## Overview

* NoSQL DB as a service
* Serverless
* Provision
    * Manual
    * Autoscale
    * On demand (this should be price competitive if you set autoscale at ~40% utilization)
* Highly resilient across AZ and possibly globally
* Fast
* Backups, PIT recovery, encrypted at rest
* Event driven integration

## Basic concepts

A **table** is a group of items with the same primary key, i.e. a row in a relational db.
A table can have an infinite number of items. 
A primary key can either be 

* Simple (Partition Key)
* Composite (Partition + Sort keys)
 
other than the key items can have really any mixture of attributes.
 
**Important**: an item can be max 400kb. 

## Capacity

Two models:

1. On demand: you get billed based on usage
   * Great for: unknown or unpredictable access pattern
   * Low admin overhead 
   * Price per **million** read and write
   * It can be up to 5x provisioned
2. Manual/Autoscale: you need to set write read speed for each table
    * 1 WCU (write capacity unit) = 1KB/s read
    * 1 RCU (read capacity unit) = 4KB/s write
    * each operation uses at least 1 W/RCU
    * Each table has a W/RCU burst pool (300 seconds). Use with care

## Backups

Two possible options:

1. On demand:
    * Full copy
    * Retained until deletion
    * Restore 
        * Same/different region (used for migrations)
        * With or without indexes
        * Encryption can be adjusted

2. Point-in-time recovery
    * Continuos stream of backup in a 35 window
    * Disable by default
    * Allow recovery down to 1s

## Operations

### Query

A query accepts a **single** PK and optionally a sort key or a range of sort keys. Capacity is used based on the returned object. Further filtering still uses capacity.

### Scan

Scan scans the whole table, element by element. It consumes capacity for each element. So it is flexible but super expensive.

## Consistency

It can either be strong or eventually consistent. For each AZ we have a storage node, one of these nodes is elected leader and all the write are routed to the leader. The leader than replicates all the changes to the other nodes. Usually this takes milliseconds but it might take longer. Reads can be set to be eventually consistent where they check only 1/3 of the nodes. Eventually consistent reads cost 50% of the cost.

## Streams and triggers

* Time ordered list of changes in a table with a24h rolling window (it is a kinetis stream).
* Per table base
* Records insert, update and delete
* Different possible view of a change
  * KEYS_ONLY, just changes on keys
  * NEW_IMAGE, stores the new item
  * OLD_IMAGE, stores the old item
  * NEW_AND_OLD_IMAGE, stores both

they can be used together with triggers. Trigger create an event that contains the data that changed. An action can be taken using that data with Lambda. Triggers are great for

* Reporting and analytics
* Aggregations, messaging and notifications.

## Indexes

Query is the fastest operation in dynamo. Indexes create alternative view of the data. A local secondary indexes (sort key) and global secondary indexes (sort + partition key).

LSI have the following properties
* Must be created with the table 
* Max of 5
* They are an alternative SK on the table
* Share the W/RCU of the table

GSI have the following properties
* Can be created at any time
* Max of 20
* Alternative PK and SK
* Have their own W/RCU
* Only eventual consistency

with both LSI/GSI you can choose the attributes to project in, i.e. to make it available via the index. If an attribute is not present it will be fetched but it is super expensive.

## Global tables

They provide a multi-master cross-region replication. Tables are created in multiple regions and added to the global table. Last write wins in case of conflicts. Read and write replicate in any region in matter of seconds. Strongly consistent only in one region.

## DynamoDB accelerator

It is an in memory cache included in Dynamodb. It is transparent, there is not overhead in handling the cashing. It supports write-thoughts and it is deployed inside a VPC. As with Dynamo there is a primary and replicas.
It can cache:

1. Items
2. Whole queries/scans

times are extremely fast:

* milliseconds for misses
* microseconds for hits


## Exam

* NoSQL or Key/value = DynamoDB
* Relational data/SQL != DynamoDB
* Billing based on WCU/RCU, storage and features
* Reservation can be used to lower the cost

## Resources

[The DynamoDB book](https://www.dynamodbbook.com/)
