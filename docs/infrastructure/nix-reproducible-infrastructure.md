# ❄️ Nix: Reproducible Infrastructure & Sovereign NixOS

This document defines the **Deterministic Layer** of the platform—synthesizing **Nix's reproducible build system**, **NixOS's declarative configuration**, and the **Sovereign DevOps** philosophy. It is the blueprint for infrastructure that is "Defined once, Identical everywhere."

---

## 🏛️ 01. The Deterministic Mandate

In industrial SaaS, "It works on my machine" is a system failure. Nix eliminates this by treating software as a **pure function**.

- **Pure Builds**: Every package is built in an isolated sandbox with zero access to the host network or filesystem.
- **The Nix Store**: All dependencies are stored in `/nix/store` with a cryptographic hash of their entire build-time configuration. 
- **Atomic Operations**: Updates and rollbacks are atomic. If a system update fails, the previous `generation` is still available at boot.

---

## 📦 02. NixOS: Declarative Fleet Management

We standardize on **NixOS** for all production servers (T1–T20).

- **`configuration.nix`**: The entire server state (users, packages, services, kernel modules) is defined in a single file.
- **Rollbacks are First-Class**: We can switch back to any previous system state in < 1 second.
- **Reproducible Runtimes**: Every Go `app` and Rust `worker` is packaged as a Nix Flake, ensuring the exact same shared libraries are used in dev, CI, and production.

---

## ⚡ 03. Flakes: The Industrial Unit of Work

We use **Nix Flakes** to lock Every dependency.

- **`flake.lock`**: Similar to `package-lock.json` but for the entire system ecosystem (C libraries, compilers, language runtimes).
- **Hermeticity**: Flakes ensure that a project cloned today will build identically 10 years from now.

---

## 🛡️ 04. Zero-Trust with Nix

Security is a byproduct of determinism.

- **Immutable Runtimes**: Production containers are built as "Minimal Nix Closures," containing ONLY the target binary and its direct dependencies (zero shell, zero package manager).
- **Auditability**: Every change to the infrastructure is a commit to a Nix file. We don't "Configure" servers; we "Publish" them.

---

## 🚀 05. Sovereign Nix Tools

We utilize the following high-agency tools within the Nix ecosystem:

- **Deployment:** **[Colmena](https://github.com/zhaofengli/colmena)** — A simple, stateless deployment tool for NixOS.
- **Secrets:** **[sops-nix](https://github.com/Mic92/sops-nix)** — Atomic secret management that integrates directly into the declarative NixOS config.
- **CI/CD:** **[Hydra](https://github.com/NixOS/hydra)** — The Nix-based continuous integration system for building and testing flakes.

---
*Last Updated: April 2026 | Following the BlackLoverTech "Build to Learn" Mandate.*
