---
title: "The Case for Sovereign Infrastructure — Why I Self-Host Everything"
date: "2026-03-15"
tags: ["Philosophy", "Infrastructure", "FOSS", "Engineering"]
author: "Janarthanan S"
---

# The Case for Sovereign Infrastructure

Every time you use a managed service, you're renting a piece of your own stack back from someone else. That's not inherently wrong — but it's a tax. A compound, long-term tax that gets heavier the more you depend on it.

## What Sovereign Infrastructure Actually Means

Sovereign infrastructure isn't about being anti-cloud. It's about **intentional ownership**. It means:

- You can read and audit every line of software running your data
- You can migrate without a vendor holding your data hostage
- Your uptime is your responsibility, not a black-box SLA

I run PostgreSQL, Redis, Nginx, and Minio on my own machines for Whatomate. Not because I can't afford Supabase or Upstash — but because I can read `pg_stat_activity`. I know exactly what's happening. That's not possible with a managed service.

## The Cost of Not Owning Your Stack

Let me give you a concrete example. A client I worked with had their entire email marketing stack on a single SaaS. When that SaaS changed its pricing model, their costs went from ₹3,000/month to ₹18,000/month — overnight. They had 90 days to migrate.

They couldn't migrate in 90 days. They paid the new rate.

That's the tax. Not the new rate — the inability to move.

## How I Think About Stack Decisions

For every tool in my stack, I ask:

1. **Can I self-host this?** If yes, is the operational overhead worth the cost savings?
2. **Is the data mine?** Even if I use a managed service, can I export everything cleanly?
3. **What's the migration cost?** If this service disappears tomorrow, how long does recovery take?

For Whatomate, the full answer is: PostgreSQL (self-hosted, full control), Redis Streams (self-hosted, replicated), S3-compatible storage (Minio on-prem), and Nginx (open source, config in Git).

The only external dependency I've accepted is Meta's WhatsApp Cloud API — and that's the product itself, not the infrastructure.

## The FOSS Philosophy Behind This

Zerodha is the clearest example of this philosophy in Indian tech. They built [knadh/listmonk](https://github.com/listmonk/listmonk) for email — a single Go binary that handles millions of sends. They open-sourced it. They built [dungbeetle](https://github.com/zerodha/dungbeetle) for async SQL jobs. They open-sourced that too.

The pattern: **build what you need, own it, publish it**. If it's good enough for your production load, it's good enough for the community.

That's the ethos I'm trying to build with BlackLoverTech.

## What This Looks Like in Practice

For every new project, I default to:

- **Compute**: My own VPS (not managed Kubernetes for small projects)
- **Database**: PostgreSQL — not PlanetScale, not Neon
- **Cache**: Redis — not Upstash (unless startup budget forces it temporarily)
- **Storage**: Minio — not S3 unless the project requires CDN-edge delivery
- **CI/CD**: Forgejo + Woodpecker — not GitHub Actions for private repos

Is this more work? Yes. Do I understand my stack better because of it? Absolutely.

## The Trade-off

I'm not naive about this. There are real trade-offs:

- **Operational load**: I maintain servers. I've debugged Redis OOM issues at 2am.
- **No support contract**: When something breaks, it's my problem.
- **Setup time**: A new project takes longer to get running.

But the compounding benefit is: **I know how everything works**. When Whatomate has a message delivery delay, I know exactly where to look — the Redis Streams consumer group, the sql-jobber workers, the WhatsApp webhook ingestion path. That's only possible because I own the entire stack.

## The Conclusion

Sovereign infrastructure is a bet on your own competence. It's saying: I'd rather understand this system than pay someone to hide its complexity from me.

That philosophy shapes every architectural decision I make. And so far, it's been worth it.

---

*Janarthanan S is the founder of Retro Tech Pvt. Ltd. and the BlackLoverTech brand. He builds sovereign SaaS from Madurai, Tamil Nadu.*
