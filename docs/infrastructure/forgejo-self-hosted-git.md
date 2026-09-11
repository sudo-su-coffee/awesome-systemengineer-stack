---
title: "Why I run Forgejo instead of GitHub for every client project"
date: "2024-12-12"
tags: ["FOSS", "DevOps", "Self-hosted", "Git"]
author: "Janarthanan S"
---

GitHub is a great product. It's also a trap. When your code, your CI/CD pipeline, your issue tracker, and your deployment secrets all live on infrastructure you don't control, you've made a choice that will be difficult to undo. I made that choice, ran into its limits, and switched everything to Forgejo.

## What Forgejo is

Forgejo is a self-hosted Git forge — it's a fork of Gitea, itself a fork of Gogs. You get repositories, pull requests, issues, wikis, package registries, and a full CI/CD system (via Woodpecker CI) running on your own hardware. The GitHub interface you're used to, but sovereign.

## Why I switched

Three reasons. First: my GitHub account was suspended due to a legacy Student Developer Pack application from 2021 — at the worst possible time, during active client deliveries. When your entire pipeline depends on a platform you don't control, that platform's policies become your operational risk. Second: GitHub Actions pricing and rate limits started mattering for heavier CI workloads. Third: client data and source code shouldn't route through Microsoft's servers unless the client explicitly wants that.

> The moment your tooling suspension affects a client delivery is the moment you realise sovereignty is not a philosophical position — it's an operational requirement.

## The setup

Forgejo runs in Docker on a dedicated VPS. Woodpecker CI runs on the same host, connected to Forgejo via webhook. Secrets are managed via Woodpecker's secret store. Backups run nightly to an S3-compatible object store (Backblaze B2). Total cost: under ₹2000/month for everything including the server.

```yaml
version: "3"
services:
  forgejo:
    image: codeberg.org/forgejo/forgejo:7
    volumes:
      - forgejo_data:/data
    environment:
      - FORGEJO__database__DB_TYPE=postgres
  woodpecker-server:
    image: woodpeckerci/woodpecker-server:latest
    environment:
      - WOODPECKER_GITEA=true
      - WOODPECKER_GITEA_URL=http://forgejo:3000
```

## What I gave up

GitHub's network effect: discoverability, the social graph, Actions marketplace. For public OSS work I still maintain a GitHub mirror. For client work and private projects, everything is on Forgejo. The tradeoff is worth it.
