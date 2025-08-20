---
categories:
- guide
description: How to use the Tunlr server
title: Server
---

{{% alert title="License Required" color="warning" %}}
This requires a [Server License]({{< ref "/pricing" >}})
{{% /alert %}}

Tunlr server is a self-hosted version of Tunlr Cloud that Tunlr clients can use.  The server is designed to be vertically and horizontally scalable, and supports redundant node hierarchies with failover for clients.

## Planning Your Deployment

Tunlr requires a bit of planning to be successfully deployed.

### Access

It is recommended to have Tunlr can be exposed directly to clients without using a reverse proxy like Apache/NGINX/Traefik.  Tunlr is designed to use HTTP/2 (and HTTP/3, eventually), and reverse proxies can make this difficult.

### Domain Delegation

Tunlr servers are designed to have an entire subdomain pointed at them via wildcard DNS records (`*.sub.example.com`).  The servers can share a subdomain as well.  Tunlr will generate hostnames to give to clients (like `sad8bs32.sub.example.com`).  **Nothing else should use this subdomain except Tunlr**.

### Certificates

Tunlr can provide HTTPS access for tunnels if configured to use a wildcard certificate.  Lets Encrypt and other ACME clients can provision these certificates via DNS challenges.

### High Availability

Tunlr can be deployed in a highly-available manner by configuring RPC.  RPC allows Tunlr servers to forward requests between each other:

- ServerA has a tunnel to ClientA via the hostname `abc.example.com`
- ServerB receives a request from UserA to `abc.example.com`
- ServerB forwards the request to ServerA
- ClientA sends the response back to ServerB
- ServerB sends the response to UserA

The RPC design of Tunlr is highly flexible.  It supports full mesh, hierarchal, and star designs, allowing you to create RPC topologies that best suit your needs.

## Configuration

Tunlr can be configured using environment variables, commandline arguments, and configuration files.  See [Config]({{% ref "/docs/references/config" %}}) for more details.

This section describes configurations for various use cases.

### Ephemeral Clients

Tunlr can support ephemeral clients and provision dynamic hostnames for them.  In this scenario:

- Clients connect to Tunlr
- Tunlr generates a random hostname based on a hostname template
- Client tunnels traffic to local services
- Tunlr or the client eventually disconnects
- On subsequent connections, the client is assigned a new hostname.

These configurations should be set:

- {{% config httpServer_rateLimitPatterns %}} to control the number of attempts a Tunlr client can access a server via rate limiting.
- {{% config server_clientLifetimeSec %}} sets the maximum lifetime of a client before it is disconnected and a new hostname is generated.
- {{% config server_hostnameTemplate %}} is the Go template used to generate hostnames for clients.
- {{% config server_requireAuthClient %}} require authentication for clients or allow anonymous clients.

### High Availability

Tunlr can be ran in a highly available manner by configuring {{% config rpc %}}.  RPC uses a Mutual TLS (mTLS) to establish connections between Tunlr servers.  Using these connections, Tunlr can discover clients connected to other Tunlr servers and route requests accordingly.

This scenario is flexible and supports these design patterns:

- **Full Mesh** All Tunlr servers are connected to each other.  Any server can act as a Tunlr client endpoint or user endpoint for request proxying.  This works well for a small number of nodes.
- **Dominate/Subordinate** Dominate Tunlr servers act as coordinators for subordinate Tunlr servers.  The subordinate Tunlr servers may act as Tunlr client endpoints, user endpoints, or both, and are meshed with the Dominate Tunlr servers.  This works well for a large number of nodes.

### Authentication

Tunlr can require authentication in a few ways:

- JWTs, via {{% config server_jwtPublicKeys %}}
- Static usernames, via {{% config server_staticUsernames %}}
- TLS authentication at the HTTP level, via {{% config httpServer_tlsClientCABase64 %}}

{{% alert title="Candid Commentary" color="info" %}}
Tunlr servers will eventually become OIDC clients and provide OIDC authentication portals for downstream tunnels.
{{% /alert %}}

## Running the Server

After [Installing Tunlr]({{% ref "/docs/guides/install-tunlr" %}}), the server can be started using a container or directly via an executable:

```
docker run --rm ghcr.io/candiddev/tunlr server
tunlr server
```
