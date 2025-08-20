---
categories:
- guide
description: How to use the Tunlr client
title: Client
---

Tunlr client is used to rapidly tunnel access to your local environments via Tunlr server.

## Quickstart

### CLI

Tunlr client can be ran from the CLI to expose a local URL such as http://localhost:3000 like this:

```bash
curl -L https://tunlr.dev/releases/tunlr_linux_amd64.gz -O | gzip -d > tunlr
chmod +x tunlr
sha256sum tunlr | grep $(curl -L https://tunlr.dev/releases/tunlr_linux_amd64.sha256)
./tunlr client http://localhost:3000
```

Tunlr will connect to Tunlr Cloud servers by default and receive a hostname with a valid certificate:

```
Tunneling https://dhrdsjxmh7shir-1.wurr.tunlr.link to http://localhost:3000
```

### Container

Tunlr client can be ran from a container to expose a local URL such as http://container1:3000 like this:

```bash
docker run --name tunlr --rm ghcr.io/candiddev/tunlr client http://container1:3000
```

Tunlr will connect to Tunlr Cloud servers by default and receive a hostname with a valid certificate:

```
docker logs -f tunlr
Tunneling https://dhrdsjxmh7shir-1.wurr.tunlr.link to http://localhost:3000
```

{{% alert title="Candid Commentary" color="info" %}}
A single Tunlr container can be embedded in a docker-compose file and expose all services within it.
{{% /alert %}}

## Advanced Usage

Tunlr client can be configured to support other use cases.

### Expose Multiple Local Endpoints

One Tunlr client can tunnel to multiple local endpoints or paths by specifying them in the commandline:

```bash
tunlr client http://localhost:3000 http://localhost:3001 hugo
```

Each target will be associated with a specific tunnel suffix:

```
Tunneling https://cjqaxrdoztbbt5-1.kwbg.tunlr.link to http://localhost:3000
Tunneling https://cjqaxrdoztbbt5-2.kwbg.tunlr.link to http://localhost:3001
Tunneling https://cjqaxrdoztbbt5-3.kwbg.tunlr.link to hugo
```

More control over targets and naming can be done using {{% config client_targets %}}:

```bash
tunlr -x client_targets_hugo_path=hugo -x client_targets_homechart_path=http://localhost:3000 client
Tunneling https://oj4oirzms9pqb3-homechart.wurr.tunlr.link to http://localhost:3000
Tunneling https://oj4oirzms9pqb3-hugo.wurr.tunlr.link to hugo
```

In this example, the suffixes are mappings (`-homechart`, `-hugo`) instead of array indexes (`-1`, `-2`).  You can embed these Tunlr configurations into your source repository for easy provisioning.

### Request a Hostname

Tunlr can request a specific hostname **if the server is configured to allow it**:

```bash
tunlr -x client_hostname=helloworld client http://localhost:3000
```

### Restrict Tunnel Access

Tunlr can be configured to restrict tunnel access to only certain IP addresses (via {{% config client_allowedNetworks %}}) or for requests that have specific headers (via {{% config client_httpHeaderFilters %}}):

```bash
tunlr -x client_allowedNetworks=192.168.0.0/16 client http://localhost:3000
```
