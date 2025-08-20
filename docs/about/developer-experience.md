---
categories:
- feature
description: Tunlr is designed for ease of access and usage.
title: Developer Experience
type: docs
---

Tunlr is a single binary that can be ran directly or via a container.

Tunlr can be started with a single CLI command to proxy access to services or local paths:

```bash
$ tunlr client http://localhost:3000 hugo/public
Tunneling https://itj1kj4o0oks2e-1.wurr.tunlr.link to http://localhost:3000
Tunneling https://itj1kj4o0oks2e-2.wurr.tunlr.link to hugo/public
```

Or Tunlr can use a configuration file and environment variables to proxy access to multiple services:

```bash
$ tunlr_client_targets_homechart_path=http://localhost:3000 tunlr_client_targets_etcha_path=http://localhost:4000 tunlr client
Tunneling https://esrfhibgqcsx9a-etcha.wurr.tunlr.link to http://localhost:4000
Tunneling https://esrfhibgqcsx9a-homechart.wurr.tunlr.link to http://localhost:3000
```

The configuration file uses Jsonnet and lives nicely in a Git repository:

```
{
  client: {
    targets: {
      etcha: {
        path: http://localhost:4000,
      },
      homechart: {
        path: http://localhost:3000,
      },
    },
  },
}
```
