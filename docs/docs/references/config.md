---
categories:
- reference
description: Reference documentation for Tunlr's configuration
title: Config
---

{{% snippet config_format Tunlr tunlr %}}

## Configuration Values

{{% snippet "config_cli" tunlr %}}

{{% snippet config_key "client" %}}

Configurations for Tunlr in client mode.

{{% snippet config_key "client_allowedNetworks" %}}

List of strings representing IPv4 and IPv6 networks that requests must originate from.  This check is performed on the Tunlr server, clients will never see requests that do not match these networks.

**Default:** `[]`

{{% snippet config_key "client_httpHeaderFilters" %}}

A string representing HTTP header filters that must be present for a client request to be tunneled.  This check is performed on the Tunlr server, clients will never see requests that do not match these headers.

The header format is a string containing boolean logic:

```json
{
  "client": {
    "httpHeaderFilters": [
      "(Authorization='^Bearer .*$' && MySecretHeader) || Authorization=^Basic",
    ],
  }
}
```

In this example, the request must match one of these conditions:

- Contain the header `Authorization` with a value that starts with `Bearer `, and contain the header `MySecretHeader` with any value.
- Contain the header `Authorization` with a value that starts with `Basic`.

**Default:** `[]`

{{% snippet config_key "client_hostname" %}}

String, a hostname to request from the server.  Server must be configured to allow client hostname requests.  If the server does not allow requests, or the hostname is taken, the server will return a randomized hostname.

**Default:** `""`

{{% snippet config_key "client_jwt" %}}

String, a JWT used for authenticating to a Tunlr server.

**Default:** `""`

{{% snippet config_key "client_password" %}}

String, a password used for authenticating to a Tunlr server.

**Default:** `""`

{{% snippet config_key "client_serverAddresses" %}}

List of server addresses to use.  Tunlr will randomize the list and attempt to connect to each one.

**Default:** `[]`

{{% snippet config_key "client_serverList" %}}

String, a URL to download a server list from.  The server list file should list the server addresses separated by a newline, i.e.:

```
https://123.tunlr.link
https://456.tunlr.link:3000
```

Tunlr will copy these addresses into [client_serverAddreses](#client_serverAddresses).

**Default:** `"https://tunlr.dev/servers.txt"`

{{% snippet config_key "client_targets" %}}

Targets is a map of target names to target configurations.  See [Guides > Client]({{< ref "/docs/guides/client" >}}) for more information.

**Default:** `{}`

### `client_targets_[target]_path` {#client_targets_path}

String, the path to the target.  Targets support these path types:

- `http://` for HTTP targets.
- `https://` for HTTPS targets.
- Other path types default to local directories

**Default:** `""`

{{% snippet config_key "client_username" %}}

String, a username used for authenticating to a Tunlr server.

**Default:** `""`

{{% snippet "config_httpClient" Tunlr %}}

{{% snippet "config_httpServer" ":5000" tunlr %}}

{{% snippet "config_jsonnet" true %}}

{{% snippet config_licenseKey Tunlr %}}

{{% snippet config_key "rpc" %}}

{{% alert title="License Required" color="warning" %}}
This requires a [Server License]({{< ref "/pricing" >}})
{{% /alert %}}

Configure the RPC connections between Tunlr servers.

{{% snippet config_key "rpc_cacheRequestsExpiration" %}}

String, the duration to cache RPC request IDs.  Setting this too low may cause duplicate request replies. {{% config_duration %}}

**Default:** `"1m"`

{{% snippet config_key "rpc_cacheServicesExpiration" %}}

String, the duration to cache RPC service names.  Setting this too low may cause excessive RPC broadcasts, and too high may cause requests to be routed to offline nodes. {{% config_duration %}}

**Default:** `"1m"`

{{% snippet config_key "rpc_findRequestTick" %}}

String, the duration to wait before checking for replies to find requests.  This should be set to the minimum end-to-end latency between RPC nodes. {{% config_duration %}}

**Default:** `"100ms"`

{{% snippet config_key "rpc_findRequestTimeout" %}}

String, the duration to wait for a find request reply.  This should be set to the maximum end-to-end latency between RPC nodes. {{% config_duration %}}

**Default:** `"10s"`

{{% snippet config_key "rpc_id" %}}

String, the unique ID of this node.  An ID will be generated if one is not provided.

**Default:** `""`

{{% snippet config_key "rpc_listenAddress" %}}

String, the listen address for TCP/TLS RPC traffic.

**Default:** `":2000"`

{{% snippet config_key "rpc_nodeAddresses" %}}

List of node addresses in the form of `host:port` to connect as a client.  Nodes can fully mesh without duplication--they will disconnect from nodes they are already connected to.

**Default:** `[]`

{{% snippet config_key "rpc_nodeKeepaliveMiss" %}}

Number, maximum number of keepalive misses from a node before it is considered dead.

**Default:** `3`

{{% snippet config_key "rpc_nodeKeepaliveTick" %}}

String, the duration to wait before issuing a keepalive. {{% config_duration %}}

**Default:** `"5s"`

{{% snippet config_key "rpc_nodeRetryMax" %}}

String, the maximum amount of time to wait before retrying a connection to a node listed in [rpc_nodeAddresses](#rpc_nodeAddresses). {{% config_duration %}}

**Default:** `"10s"`

{{% snippet config_key "rpc_nodeRetryMin" %}}

String, the minimum amount of time to wait before retrying a connection to a node listed in [rpc_nodeAddresses](#rpc_nodeAddresses). {{% config_duration %}}

**Default:** `"100ms"`

{{% snippet config_key "rpc_tlsCABase64" %}}

String, a base64 encoded string of the certificate authority used to establish mutal TLS (mTLS).  Required.

**Default:** `""`

{{% snippet config_key "rpc_tlsCABase64" %}}

String, a base64 encoded string of the certificate authority used to establish mutal TLS (mTLS).

**Default:** `""`

{{% snippet config_key "server" %}}

{{% alert title="License Required" color="warning" %}}
This requires a [Server License]({{< ref "/pricing" >}})
{{% /alert %}}

Configurations for Tunlr in server mode.

{{% snippet config_key "server_allowClientHostnames" %}}

Boolean, allows clients to request hostnames.

**Default:** `false`

{{% snippet config_key "server_clientKeepaliveSec" %}}

Number, maximum number of seconds to send periodic keepalives to clients.  The exact keepalive interval is randomized to limit timing attacks.

**Default:** `60` (1 minute)

{{% snippet config_key "server_clientLifetime" %}}

String, duration a client can be connected to a Tunlr server.  Clients can always reconnect, but they may receive a new hostname if [server_allowClientHostnames](#server_allowClientHostnames) is false.  Combine this with [httpServer_rateLimitPatterns](#httpServer_rateLimitPatterns) to limit how many times a client can connect a Tunlr server. {{% config_duration %}}

**Default:** `"20m"`

{{% snippet config_key "server_domain" %}}

String, the domain to generate certificates from.  If not specified, will use the domain of the server's hostname.

**Default:** `""`

{{% snippet config_key "server_hostnameTemplate" %}}

String, the hostname template to use for generating hostnames for clients.  Use Go templating.  The default template will use the `sub` JWT claim if provided or generate a 14 character Nanoid.  All names generated will be converted to lowercase and stripped of any non-alphaneumeric character, including `-`.

**Default:** `"{{ if eq (index . "sub") nil }}{{ nanoid 14 }}{{ else }}{{ .sub }}{{ end }}"`

{{% snippet config_key "server_jwtExpires" %}}

String, duration until JWTs signed by Tunlr server expire.  {{% config_duration %}}

**Default:** `"1d"`

{{% snippet config_key "server_jwtPrivateKey" %}}

String, a JWT private key used to sign JWTs.  Can be generated using {{% cli keys %}}.  If one is not provided, a random one will be generated, however JWTs will be invalidated everytime Tunlr restarts.

**Default:** `""`

{{% snippet config_key "server_jwtPublicKeys" %}}

List of strings containing JWT public keys to use for JWT authentication.

**Default:** `[]`

{{% snippet config_key "server_maxClients" %}}

Number, maximum number of clients a server can support.  When new clients try to connect after this limit is reached, they will receive an error and will connect to other servers if configured.

Setting `0` means the Tunlr server can support an unlimited number of clients.

**Default:** `0`

{{% snippet config_key "server_maxRequestSizeKB" %}}

Number, the maximum request size that can be sent to this server in kilobytes.  Requests sent to this server over this limit will receive an error response and not be proxied.

Setting `0` means there is no limit.

**Default:** `0`

{{% snippet config_key "server_maxResponseSizeKB" %}}

Number, the maximum response size that can be received by this server in kilobytes.  Responses sent to this server over this limit will receive an error response and not be proxied.

Setting `0` means there is no limit.

**Default:** `0`

{{% snippet config_key "server_requireAuthClient" %}}

Boolean, require clients to be authenticated via [JWT](#client_jwt) or [staticUsernames](#client_username)

**Default:** `true`

{{% snippet config_key "server_requestTimeout" %}}

String, duration to wait for a request to complete (from client request to tunlr client response).  {{% config_duration %}}

**Default:** `"1m"`

{{% snippet config_key "server_staticUsernames" %}}

Map of string representing a username to user details:

```json
{
  "userA": {
    "claims": {
      "admin": true
    },
    "password": "passwordA"
  },
  "userB": {
    "password": "passwordB"
  }
}
```

**Default:** `{}`

{{% snippet config_key "server_staticUsernames_claims" %}}

A map of strings to any value.  Used when generating JWTs, and can be referenced using [hostnameTemplate](#server_hostnameTemplate).

**Default:** `{}`

{{% snippet config_key "server_staticUsernames_password" %}}

String, the plain text password for the user.

**Default:** `""`

{{% snippet config_key "server_staticUsernames_passwordHash" %}}
String, the password hash for the user.  Can be generated using {{% cli password %}}.

**Default:** `""`

{{% snippet config_key "server_systemClientKey" %}}

String, a secret key to access the `/api/v1/system/client?key=<server_systemClientKey>` endpoint.  This endpoint returns details about the client, such as headers and the remote address.  The endpoint is not enabled if this key is unset.  Useful for debugging.

**Default:** `""`

{{% snippet config_key "server_systemMetricsKey" %}}

String, a secret key to access the `/api/v1/system/metrics?key=<server_systemMetricsKey>` endpoint.  This endpoint returns Prometheus-style scrape metrics.  The endpoint is not enabled if this key is unset

**Default:** `""`

{{% snippet config_key "server_tunnelHTTPS" %}}

Boolean, returns `https://` hostname schemes to clients when true.  If false, clients will see `http://` hostname schemes.

**Default:** `false`

{{% snippet config_key "server_tunnelPort" %}}

Number, the port number appended to client hostnames.  Defaults to [httpServer_listenAddress](#httpServer_listenAddress).

**Default:** `5000`
