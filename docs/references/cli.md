---
categories:
- reference
description: Reference documentation for Tunlr's CLI
title: CLI
---

{{% snippet cli_arguments %}}

{{% snippet cli_commands Tunlr %}}

{{% cli_autocomplete %}}

### `client`

Run Tunlr in client mode.  See [Guides > Client]({{< ref "/docs/guides/client" >}}) for more details.

{{% snippet cli_config %}}

{{% snippet cli_docs %}}

{{% snippet cli_eula Tunlr %}}

{{% snippet cli_jq %}}

### `keys`

Generate cryptographic keys for server JWT signing.

### `password`

Generate a password hash for {{% config server_staticUsernames_passwordHash %}}.

### `server`

{{% alert title="License Required" color="warning" %}}
This requires a [Server License]({{< ref "/pricing" >}})
{{% /alert %}}

Run Tunlr in server mode.  See [Guides > Server]({{< ref "/docs/guides/server" >}}) for more details.

{{% cli_version %}}
