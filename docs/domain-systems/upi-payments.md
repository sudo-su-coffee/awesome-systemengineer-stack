---
title: "UPI on a blockchain: what I researched, what NPCI said, and why it matters"
date: "2025-03-15"
tags: ["Security", "Blockchain", "India", "Payments"]
author: "Janarthanan S"
---

UPI processes over 12 billion transactions a month. It's the most successful retail payment rail in the world. It's also built on a centralised architecture where NPCI sits at the centre of every transaction. I spent several months researching what a blockchain overlay on UPI would look like — not as a gimmick, but as a genuine technical proposal for improving auditability and reducing reconciliation fraud.

## The actual problem I was trying to solve

UPI reconciliation failures are a real and underreported problem. When a transaction fails mid-flight — after the debit but before the credit — the resolution process is opaque and slow. Banks, payment service providers, and NPCI all hold partial state with no shared ledger. The dispute resolution system works, but it works slowly and with significant operational overhead.

The question I was researching: could a permissioned blockchain — specifically a distributed ledger shared between NPCI, banks, and PSPs — improve the auditability of in-flight transactions without increasing latency?

## The architecture I proposed

Not a public blockchain. Not Ethereum. A permissioned Hyperledger Fabric network where each UPI transaction generates an immutable event log entry accessible to all parties in the transaction chain. The key properties:

- **Settlement finality**: once a transaction is confirmed on the ledger, it cannot be disputed without a quorum of participants signing off on the reversal.
- **Transparent reconciliation**: any party can verify the state of a transaction at any point without calling NPCI's API.
- **Latency impact**: near-zero, because the ledger write happens async to the payment flow, not in the critical path.

## What NPCI contacts said

Through connections at NPCI, the proposal was reviewed informally. The feedback was substantive: the reconciliation problem is real and acknowledged. The blockchain approach to auditability is technically sound. The practical barriers are organisational — getting 30+ banks and 100+ PSPs to run nodes is a coordination problem, not a technical one.

I'm publishing this because the technical work deserves to be documented, even if the organisational path is long.

## What this taught me about payments infrastructure

Payments infrastructure is a coordination problem dressed up as a technical problem. The technology is almost always the easy part. The hard part is getting parties with competing interests and different legacy systems to agree on a shared protocol. UPI itself succeeded because NPCI could mandate adoption through regulatory power. A blockchain overlay would require the same.
