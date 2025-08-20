---
categories:
- feature
description: Tunlr provides a scalable way to expose all of your dynamic environments.
title: Scalable Access
type: docs
---

## Scalable Single Node

Each Tunlr server is designed to operate statelessly and entirely in-memory.  Each server is delegated a DNS subdomain and generates random hostnames from the subdomain for each client:

```mermaid
graph BT
  tunlrClient1[Tunlr Client 1]
  tunlrClient2[Tunlr Client 2]
  tunlrServer[Tunlr Server - 123.tunlr.link]

  tunlrServer -- Assigns abc.123.tunlr.link --> tunlrClient1
  tunlrServer -- Assigns def.123.tunlr.link --> tunlrClient2
```

## Scale-Out Server List

Tunlr's architecture is designed to support an unlimited number of clients in a scale-out architecture:

```mermaid
flowchart TD
  tunlrClient1[Tunlr Client 1]
  tunlrClient2[Tunlr Client 2]
  tunlrServer1[Tunlr Server 1]
  tunlrServer2[Tunlr Server 2]
  tunlrServer3[Tunlr Server 3]

  tunlrClient1 --> tunlrServer1
  tunlrClient2 -- Attempts Connection, Server Full --> tunlrServer1
  tunlrClient2 -- Tries Next Server --> tunlrServer2
  tunlrClient2 -. Tries Next Server .-> tunlrServer3
```
