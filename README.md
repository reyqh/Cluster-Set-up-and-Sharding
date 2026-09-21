# Cluster-Set-up-and-Sharding
Self-deployed MongoDB sharded cluster implementing four replicated regional shards, config servers, mongos routing, automated data distribution, and aggregation queries across an employee burnout dataset.

# MongoDB Cluster Setup & Sharding

This project implements a self-deployed MongoDB sharded cluster on a local machine, demonstrating distributed data storage, replication, sharding, cluster configuration, and query routing.

## Project Overview

### 1. Self-Deployed MongoDB Cluster

The cluster was built locally using scripts and multiple MongoDB processes to simulate a distributed environment.

The architecture consists of:

- 4 regional shards: North, South, East and West
- 3 MongoDB nodes per shard
- 3 config server nodes
- Replica sets for each shard
- A `mongos` router for client access
- Separate data and log directories for each MongoDB process

Each regional shard represents a separate data centre and stores part of the application data. The config server replica set stores cluster metadata, while `mongos` acts as the interface between clients and the underlying shards. 

### 2. Replica Set Configuration

Four shard replica sets were created:

- `northRS`
- `southRS`
- `eastRS`
- `westRS`

Each replica set contains three MongoDB nodes, providing replication within each shard. This means that if one node fails, the other replica set members can continue operating with copies of the data. 

A separate three-node config server replica set called `cfgRS` was also created to manage metadata for the sharded cluster. 

### 3. Mongos Router & Cluster Configuration

A `mongos` router was configured as the main entry point for client requests. Rather than connecting directly to individual shards, queries and writes are sent through `mongos`, which uses metadata from the config servers to route requests to the appropriate shard. 

The four shard replica sets were then added to the cluster using `sh.addShard()` and verified using `sh.status()`. 

### 4. Sharding & Shard Key

Sharding was enabled on the `burnoutAnalysisDB` database and an employee burnout collection was created.

The shard key used was:

```javascript
{ department: 1, employee_id: 1 }
