---
title: "Why I switched my entire server fleet to NixOS and will never go back"
date: "2025-01-20"
tags: ["NixOS", "Infra", "Sysadmin", "FOSS"]
author: "Janarthanan S"
---

I used to manage servers the normal way: Ansible playbooks, bash scripts, careful documentation of what I'd installed and why. It worked until it didn't — until a server broke in a way that didn't match the documentation, or an update changed behaviour subtly, or I had to reproduce an environment six months later on new hardware.

NixOS fixed this. The entire system configuration — every package, every service, every config file — is described in a single declarative Nix expression. The system is exactly and only what the expression describes. Nothing more, nothing less.

## What "reproducible" actually means in practice

On a traditional Linux system, if you ask "why is nginx configured this way?", the answer is somewhere in your git history, your Ansible plays, three README files, and one engineer's memory. On NixOS, the answer is always: look at `configuration.nix`. That file is the complete truth of the system.

```nix
# This is the complete nginx configuration for this server.
# Nothing else exists.
services.nginx = {
  enable = true;
  virtualHosts."blacklovertech.in" = {
    forceSSL = true;
    enableACME = true;
    root = "/var/www/blt";
  };
};
```

If I deploy this on a new server, I get exactly the same result. If I roll back, I get exactly the previous state. Rollbacks are atomic and instant — NixOS keeps previous system generations and switching between them is a single command.

## The learning curve is real

Nix the language is unlike anything else. It's a lazy, purely functional language built for describing package builds and system configurations. The error messages are cryptic. The documentation is scattered. It took me about two weeks before things stopped feeling backwards.

The investment is worth it. After those two weeks, I stopped worrying about server state entirely. I stopped wondering if two servers were actually configured the same way. I stopped debugging environment drift.

## What I run on NixOS now

My entire client delivery infrastructure: CI/CD nodes, Forgejo instance, Woodpecker CI runners, PostgreSQL, Redis, monitoring stack (Prometheus + Grafana). All described in Nix, all reproducible, all version-controlled in a private git repo. New server provisioning takes 20 minutes including the OS install.

If you're a system administrator who manages more than two servers, learn NixOS. The upfront cost is real. The long-term payoff is complete confidence in your infrastructure state.
