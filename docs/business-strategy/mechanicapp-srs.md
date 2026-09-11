# MechOnDemand — Software Requirements Specification
**Version:** 1.0.0  
**Author:** BLT / Retrotech Pvt Ltd  
**Status:** Draft  
**Date:** June 2026

---

## Table of Contents

1. [Introduction](#1-introduction)
2. [System Overview](#2-system-overview)
3. [Stakeholders & User Roles](#3-stakeholders--user-roles)
4. [Functional Requirements](#4-functional-requirements)
5. [Screen-by-Screen Blueprint — Customer App](#5-screen-by-screen-blueprint--customer-app)
6. [Screen-by-Screen Blueprint — Mechanic / Workshop App](#6-screen-by-screen-blueprint--mechanic--workshop-app)
7. [Core Workflows & State Machines](#7-core-workflows--state-machines)
8. [API Integrations](#8-api-integrations)
9. [Non-Functional Requirements](#9-non-functional-requirements)
10. [Data Models](#10-data-models)
11. [Subscription & Billing Engine](#11-subscription--billing-engine)
12. [Security Requirements](#12-security-requirements)
13. [Tech Stack Recommendation](#13-tech-stack-recommendation)
14. [Out of Scope](#14-out-of-scope)

---

## 1. Introduction

### 1.1 Purpose
This SRS defines the complete functional, non-functional, and interface requirements for **MechOnDemand** — an on-demand vehicle service platform connecting vehicle owners with GST-verified, branded mechanic workshops. The model is analogous to Swiggy/Zomato for food, but applied to automotive repair and maintenance, with a Namma Yatri-style flat subscription engine instead of per-transaction commissions.

### 1.2 Scope
The platform comprises:
- **Customer Mobile App** (Android + iOS)
- **Mechanic / Workshop Mobile App** (Android + iOS)
- **Backend API Server**
- **Admin Dashboard** (Web)

### 1.3 Business Logic Pillars
| Pillar | Rule |
|--------|------|
| **Quality Gate** | Only GST-registered, branded workshops onboard |
| **No Catalog Bloat** | Parts via open text + procure checkbox — no SKU catalog |
| **Mandatory Call** | Mechanic cannot dispatch without logging a pre-arrival call |
| **User Authorization** | Mechanic cannot move until user approves total bill in-app |
| **Zero Per-Transaction Cut** | Platform earns via monthly tiered subscription from mechanics |

---

## 2. System Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                        MechOnDemand Platform                        │
│                                                                     │
│  ┌──────────────┐    REST/WS     ┌──────────────────────────────┐  │
│  │ Customer App │ ◄────────────► │                              │  │
│  └──────────────┘                │       Backend API Server     │  │
│                                  │  (Go / Node — REST + WS)     │  │
│  ┌──────────────┐    REST/WS     │                              │  │
│  │ Mechanic App │ ◄────────────► │  PostgreSQL  │  Redis        │  │
│  └──────────────┘                │  FCM Push   │  S3/R2        │  │
│                                  └──────────────────────────────┘  │
│  ┌──────────────┐    REST                                           │
│  │ Admin Dashboard◄───────────────────────────────────────────┐    │
│  └──────────────┘                                              │    │
│                                                                │    │
│  External:  Vahan/RTO API │ GST Verify API │ Google Maps │ UPI │    │
└─────────────────────────────────────────────────────────────────────┘
```

### 2.1 Two Booking Tracks

```
User Opens App
     │
     ├──► [ URGENT / SOS Toggle ON ]
     │         └─► GPS ping → nearest verified shop within 5km
     │             60s countdown match → flash alert on mechanic app
     │
     └──► [ Scheduled Booking ]
               └─► Select tier → describe issue → pick slot (6hr–7day)
                   → shop confirms → mandatory call → bill → dispatch
```

---

## 3. Stakeholders & User Roles

| Role | Description |
|------|-------------|
| **Vehicle Owner (Customer)** | Books mechanic service, approves bill, tracks job |
| **Mechanic / Workshop Owner** | Accepts jobs, logs diagnosis, inputs parts+cost, dispatches |
| **Platform Admin** | Manages onboarding, subscription billing, dispute resolution |
| **Shop Manager** (optional) | Sub-account under workshop; manages multiple mechanics |

---

## 4. Functional Requirements

### 4.1 Customer App

| ID | Requirement |
|----|-------------|
| C-FR-01 | OTP-based mobile login (+91 preset) |
| C-FR-02 | Vehicle onboarding via RTO/Vahan API (registration number → auto-fill model, fuel type, year) |
| C-FR-03 | Manual vehicle entry fallback if API fails |
| C-FR-04 | Toggle between Urgent SOS and Scheduled booking modes |
| C-FR-05 | Service tier selection: Basic / Medium / Comprehensive |
| C-FR-06 | Free-text issue description with brand preference input |
| C-FR-07 | "Authorize mechanic to procure parts" checkbox |
| C-FR-08 | Calendar booking: minimum 6 hours, maximum 7 days in advance |
| C-FR-09 | Time slot selection: Morning / Afternoon / Evening |
| C-FR-10 | Real-time mechanic location tracking after dispatch |
| C-FR-11 | In-app push notification for mechanic's itemized quote |
| C-FR-12 | Bill approval / rejection screen with itemized breakdown |
| C-FR-13 | "Approve & Authorize Dispatch" CTA that unlocks mechanic navigation |
| C-FR-14 | Post-job rating and review submission |
| C-FR-15 | Job history with invoice records |

### 4.2 Mechanic / Workshop App

| ID | Requirement |
|----|-------------|
| M-FR-01 | GST-verified onboarding (GSTIN validated against government DB) |
| M-FR-02 | Location pinning + service radius slider (3/5/10 km) |
| M-FR-03 | Vehicle category capability declaration |
| M-FR-04 | Full-screen flash alert for Urgent bookings with 60s countdown |
| M-FR-05 | Scheduled job calendar/list view |
| M-FR-06 | Job detail view showing vehicle info + user text notes |
| M-FR-07 | Mandatory "Call Customer" interlock — dispatch locked until call logged |
| M-FR-08 | In-app voice call trigger (or call log confirmation) |
| M-FR-09 | Digital job sheet: service checklist toggles (labor) |
| M-FR-10 | Dynamic spare parts logger: part name + cost per row, add/remove rows |
| M-FR-11 | Live total calculator: labor + parts = grand total |
| M-FR-12 | "Send for User Approval" CTA — freezes sheet, pushes notification to customer |
| M-FR-13 | Dispatch screen unlocks only on user approval event |
| M-FR-14 | Turn-by-turn Google Maps navigation to customer location |
| M-FR-15 | Parts reminder banner on dispatch screen |
| M-FR-16 | Real-time earnings and job ledger dashboard |
| M-FR-17 | Monthly subscription tier visualizer (progress bar) |
| M-FR-18 | Monthly renewal screen with UPI QR / UPI intent for flat fee payment |

---

## 5. Screen-by-Screen Blueprint — Customer App

---

### MODULE C-1: Onboarding & Vehicle Setup

---

#### Screen C-1.1 — Splash & OTP Login

```
┌─────────────────────────────────┐
│                                 │
│         🔧 MechOnDemand         │
│     Your Trusted Workshop       │
│                                 │
│   ┌─────────────────────────┐   │
│   │  🇮🇳 +91  │ 9876543210  │   │
│   └─────────────────────────┘   │
│                                 │
│      [ GET OTP → ]              │
│                                 │
│   ─────── or ───────            │
│   Continue with Google          │
│                                 │
└─────────────────────────────────┘
```

**Behavior:**
- Auto-reads OTP via SMS Retriever API (Android) / PassKit (iOS)
- No password, no email — phone-first auth
- Deep link to booking if referral code present in URL

---

#### Screen C-1.2 — Vehicle Registration (RTO API)

```
┌─────────────────────────────────┐
│  ← Add Your Vehicle             │
├─────────────────────────────────┤
│                                 │
│  Vehicle Registration Number    │
│  ┌─────────────────────────┐   │
│  │  TN  07  CX  1234       │   │
│  └─────────────────────────┘   │
│                                 │
│      [ VERIFY VEHICLE ]         │
│                                 │
│  ── or enter manually ──        │
│                                 │
└─────────────────────────────────┘
```

**API Call:** `GET /rto/vehicle?reg=TN07CX1234` → Vahan API  
**States:** Loading spinner → Success card → Error fallback

---

#### Screen C-1.3 — Vehicle Confirmation Card

```
┌─────────────────────────────────┐
│  ✓ Vehicle Found                │
├─────────────────────────────────┤
│                                 │
│  🚗  Hyundai i20 Asta (2022)    │
│  Fuel:   Petrol                 │
│  Owner:  J***NATH S             │
│  Engine: KxxxxxxX (last 3 shown)│
│                                 │
│  [ ✓ THIS IS MY VEHICLE ]       │
│                                 │
│     Edit / Enter Manually       │
│                                 │
└─────────────────────────────────┘
```

**Note:** Never display full chassis/engine number — show masked partial for security.

---

### MODULE C-2: Home & Discovery

---

#### Screen C-2.1 — Service Hub (Home)

```
┌─────────────────────────────────┐
│  👋 Hello, Janarthanan          │
│  📍 Madurai, TN                 │
├─────────────────────────────────┤
│                                 │
│  ┌──────────────────────────┐   │
│  │  🚨  URGENT / SOS MODE   │   │
│  │  Breakdown? Get help now │   │
│  └──────────────────────────┘   │
│                                 │
│  Schedule a Service             │
│  ┌────────┐ ┌────────┐ ┌──────┐ │
│  │ Basic  │ │ Medium │ │ Full │ │
│  │ ₹299+  │ │ ₹499+  │ │₹799+ │ │
│  └────────┘ └────────┘ └──────┘ │
│                                 │
│  Nearby Workshops (3)           │
│  ┌──────────────────────────┐   │
│  │ Express Auto Care  1.2km │   │
│  │ ✓ GST Verified    ⭐4.8  │   │
│  └──────────────────────────┘   │
│  ┌──────────────────────────┐   │
│  │ Sree Maruti Service 2.1km│   │
│  │ ✓ GST Verified    ⭐4.6  │   │
│  └──────────────────────────┘   │
│                                 │
└─────────────────────────────────┘
```

---

#### Screen C-2.2A — Urgent / SOS Track

```
┌─────────────────────────────────┐
│  🚨 URGENT MODE ACTIVE          │
│  UI: Amber accent theme         │
├─────────────────────────────────┤
│  📍 Your Location Pinned        │
│  [          MAP VIEW          ] │
│                                 │
│  What's the emergency?          │
│  ┌────────┐ ┌────────────────┐  │
│  │🛞 Flat │ │🌡 Overheating  │  │
│  │ Tyre   │ │                │  │
│  └────────┘ └────────────────┘  │
│  ┌────────┐ ┌────────────────┐  │
│  │🔋 Dead │ │🔧 Other Issue  │  │
│  │Battery │ │(type below)    │  │
│  └────────┘ └────────────────┘  │
│                                 │
│  ┌──────────────────────────┐   │
│  │ Describe issue...        │   │
│  └──────────────────────────┘   │
│                                 │
│  [ FIND NEAREST MECHANIC NOW ]  │
│  Scanning 5km radius...         │
│                                 │
└─────────────────────────────────┘
```

---

#### Screen C-2.2B — Scheduled Booking Track

**Step 1: Tier Selection**

```
┌─────────────────────────────────┐
│  ← Select Service Type          │
├─────────────────────────────────┤
│                                 │
│  ┌──────────────────────────┐   │
│  │ 🔵 BASIC                 │   │
│  │ Fluid top-ups, inspection│   │
│  │ minor adjustments        │   │
│  │ Starting ₹299            │   │
│  └──────────────────────────┘   │
│                                 │
│  ┌──────────────────────────┐   │
│  │ 🟡 MEDIUM                │   │
│  │ Oil filter, brake clean  │   │
│  │ periodic servicing       │   │
│  │ Starting ₹499            │   │
│  └──────────────────────────┘   │
│                                 │
│  ┌──────────────────────────┐   │
│  │ 🔴 COMPREHENSIVE         │   │
│  │ Deep mechanical repairs  │   │
│  │ engine diagnostics       │   │
│  │ Starting ₹799            │   │
│  └──────────────────────────┘   │
│                                 │
└─────────────────────────────────┘
```

---

**Step 2: Smart Customizer (C-2.3B)**

```
┌─────────────────────────────────┐
│  ← Describe Your Service        │
│  Vehicle: Hyundai i20 | Petrol  │
├─────────────────────────────────┤
│                                 │
│  Quick Select (Optional)        │
│  [x] Oil Change                 │
│  [x] Brake Inspection           │
│  [ ] Battery Check              │
│  [ ] AC Service                 │
│  [ ] Wheel Alignment            │
│                                 │
│  Describe in detail:            │
│  ┌──────────────────────────┐   │
│  │ e.g., "Need synthetic    │   │
│  │ oil change, use Castrol  │   │
│  │ 5W-30. Front wipers also │   │
│  │ uneven."                 │   │
│  └──────────────────────────┘   │
│                                 │
│  [x] Authorize mechanic to      │
│      procure parts on my behalf │
│  ⓘ Parts cost discussed on call │
│                                 │
│  [ NEXT: PICK DATE & TIME → ]   │
│                                 │
└─────────────────────────────────┘
```

---

**Step 3: Calendar Booking (C-2.4B)**

```
┌─────────────────────────────────┐
│  ← Schedule Your Service        │
├─────────────────────────────────┤
│                                 │
│  < Jun 2026  >                  │
│  Mo Tu We Th Fr Sa Su           │
│     10 11 12 13 14 15           │
│  16 17 18 19 20 21 22           │
│                                 │
│  (Greyed: today & next 6 hours) │
│  (Greyed: beyond 7 days)        │
│                                 │
│  Select Time Slot               │
│  ┌──────────┐ ┌──────────┐      │
│  │ Morning  │ │Afternoon │      │
│  │ 9AM-12PM │ │12PM-4PM  │      │
│  └──────────┘ └──────────┘      │
│  ┌──────────┐                   │
│  │ Evening  │                   │
│  │ 4PM-8PM  │                   │
│  └──────────┘                   │
│                                 │
│  [ CONFIRM BOOKING → ]          │
│                                 │
└─────────────────────────────────┘
```

---

### MODULE C-3: Job Tracking & Communication

---

#### Screen C-3.1 — Matching / Awaiting Confirmation

**Urgent Track:**
```
┌─────────────────────────────────┐
│  🔍 Finding Nearest Mechanic    │
├─────────────────────────────────┤
│                                 │
│     [ RADAR ANIMATION ]         │
│   Pinging mechanics nearby...   │
│                                 │
│  ● Express Auto Care   1.2km    │
│  ● Sree Maruti         2.1km    │
│  ● KM Auto Works       3.8km    │
│                                 │
│  Waiting for acceptance...      │
│  Estimated response: ~30 sec    │
│                                 │
│       [ CANCEL REQUEST ]        │
│                                 │
└─────────────────────────────────┘
```

**Scheduled Track:**
```
┌─────────────────────────────────┐
│  ✓ Booking Submitted            │
│  Awaiting workshop confirmation │
├─────────────────────────────────┤
│  Express Auto Care              │
│  Jun 14, 2026 | Morning Slot    │
│  Hyundai i20 | Oil Change       │
│                                 │
│  Status: ⏳ Pending Confirm     │
│  (You'll get a push notification)│
└─────────────────────────────────┘
```

---

#### Screen C-3.2 — Pre-Arrival Alignment & Tracking

```
┌─────────────────────────────────┐
│  🔧 Mechanic On The Way         │
├─────────────────────────────────┤
│                                 │
│  [    LIVE MAP — mechanic dot   │
│       moving toward user pin  ] │
│                                 │
├─────────────────────────────────┤
│  Rajan K. — Express Auto Care  │
│  ✅ GST Verified Partner        │
│  ⭐ 4.8  (214 jobs)             │
│                                 │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│  ⚠️ Mechanic is logging your    │
│  parts list. Tap to discuss.    │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                 │
│         [ 📞 CALL ]             │
│                                 │
└─────────────────────────────────┘
```

---

#### Screen C-3.3 — Bill Approval (Critical Flow)

```
┌─────────────────────────────────┐
│  📋 Review & Approve Quote      │
│  Rajan K. | Express Auto Care   │
├─────────────────────────────────┤
│  Vehicle: Hyundai i20           │
│  Reg: TN-07-CX-1234             │
│                                 │
│  ✅ Issues Confirmed on Call:   │
│   • Oil Change (Synthetic)      │
│   • Oil Filter Replacement      │
│                                 │
│  Parts Procurement List:        │
│  ┌──────────────────────────┐   │
│  │ Castrol 5W-30 (4L) ₹2,800│   │
│  │ Oil Filter         ₹ 450 │   │
│  │ ─────────────────────────│   │
│  │ Total Parts        ₹3,250│   │
│  └──────────────────────────┘   │
│  Service Fee:          ₹  499   │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│  GRAND TOTAL:         ₹3,749    │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                 │
│  ⓘ Payment collected after job  │
│  completion. Parts authorized.  │
│                                 │
│  [ ✓ APPROVE & AUTHORIZE ]      │
│  [ ✗ Reject / Cancel ]          │
│                                 │
└─────────────────────────────────┘
```

**Backend event on approval:** `job.status → DISPATCHED`, push to mechanic app, unlock navigation.

---

#### Screen C-3.4 — Job In Progress

```
┌─────────────────────────────────┐
│  🔧 Service In Progress         │
│  Rajan K. has arrived           │
├─────────────────────────────────┤
│  Hyundai i20 | Oil Change       │
│  Started: 10:23 AM              │
│                                 │
│  ┌──────────────────────────┐   │
│  │ [ OTP: 8842 ]            │   │
│  │ Share this with mechanic │   │
│  │ to start the job         │   │
│  └──────────────────────────┘   │
│                                 │
│  Authorized Total: ₹3,749       │
│                                 │
│  [ 📞 Call Mechanic ]           │
│  [ ⚠️ Report Issue ]            │
│                                 │
└─────────────────────────────────┘
```

---

#### Screen C-3.5 — Job Complete & Payment

```
┌─────────────────────────────────┐
│  ✅ Service Completed!          │
├─────────────────────────────────┤
│  Express Auto Care              │
│  Jun 14, 2026 | 10:23–11:45 AM  │
│                                 │
│  Final Bill:          ₹3,749    │
│                                 │
│  Pay via UPI:                   │
│  [ PhonePe ] [ GPay ] [ BHIM ]  │
│                                 │
│  ─────────────────────────────  │
│  Rate your experience:          │
│  ★ ★ ★ ★ ☆                     │
│  [ Write a review... ]          │
│                                 │
│  [ SUBMIT RATING ]              │
└─────────────────────────────────┘
```

---

### MODULE C-4: Job History

#### Screen C-4.1 — My Bookings

```
┌─────────────────────────────────┐
│  ← My Bookings                  │
├─────────────────────────────────┤
│  UPCOMING                       │
│  ┌──────────────────────────┐   │
│  │ Jun 14 | Morning         │   │
│  │ Express Auto Care        │   │
│  │ Hyundai i20 | Oil Change │   │
│  │ ₹3,749  [ Approved ]     │   │
│  └──────────────────────────┘   │
│                                 │
│  PAST                           │
│  ┌──────────────────────────┐   │
│  │ May 22 | ✅ Completed    │   │
│  │ KM Auto Works            │   │
│  │ i20 | Brake Service      │   │
│  │ ₹1,299  ⭐4/5            │   │
│  └──────────────────────────┘   │
└─────────────────────────────────┘
```

---

## 6. Screen-by-Screen Blueprint — Mechanic / Workshop App

---

### MODULE M-1: Workshop Onboarding & Verification

---

#### Screen M-1.1 — Business Profile & GST Validation

```
┌─────────────────────────────────┐
│  Register Your Workshop         │
├─────────────────────────────────┤
│                                 │
│  Workshop / Brand Name          │
│  [ Express Auto Care          ] │
│                                 │
│  Owner Full Name                │
│  [ Rajan Kumar                ] │
│                                 │
│  Phone (OTP verified)           │
│  [ +91 9876543210             ] │
│                                 │
│  GSTIN                          │
│  [ 33AABCE1234F1Z5  🔄 ]       │
│  ✅ GST Verified: Registered     │
│     Business Entity             │
│                                 │
│  [ NEXT → ]                     │
│                                 │
└─────────────────────────────────┘
```

**Validation:** Live GSTIN lookup → `api.gst.gov.in` → verify status = ACTIVE  
**Reject:** Cancelled / suspended GST registrations

---

#### Screen M-1.2 — Location & Operational Capacity

```
┌─────────────────────────────────┐
│  ← Set Service Area             │
├─────────────────────────────────┤
│                                 │
│  [      LIVE MAP — pin drop   ] │
│  [ Drop pin on your workshop  ] │
│                                 │
│  Service Radius                 │
│  3km ●──────────────── 10km    │
│       Current: 5km              │
│                                 │
│  Vehicle Types You Service:     │
│  [x] Hatchbacks & Sedans        │
│  [x] SUVs & Luxury              │
│  [ ] Two-Wheelers               │
│  [ ] Commercial Vehicles        │
│                                 │
│  [ COMPLETE ONBOARDING → ]      │
│                                 │
└─────────────────────────────────┘
```

---

#### Screen M-1.3 — Subscription Plan Briefing

```
┌─────────────────────────────────┐
│  How Billing Works              │
├─────────────────────────────────┤
│                                 │
│  We don't cut your earnings.    │
│  You pay a flat monthly fee     │
│  based on jobs completed.       │
│                                 │
│  Tier 1:  1–20 jobs  → ₹499/mo  │
│  Tier 2: 21–50 jobs  → ₹999/mo  │
│  Tier 3: 51–100 jobs → ₹1,499/mo│
│  Tier 4: 100+ jobs   → ₹1,999/mo│
│                                 │
│  ✅ No per-transaction cut       │
│  ✅ Billed on 1st of every month │
│  ✅ Pay via UPI in one transfer  │
│                                 │
│  [ I UNDERSTAND, LET'S GO → ]   │
│                                 │
└─────────────────────────────────┘
```

---

### MODULE M-2: Booking Alert & Diagnostic Call Flow

---

#### Screen M-2.1 — Full-Screen Flash Alert (Urgent SOS)

```
┌─────────────────────────────────┐
│  ███ 🚨 URGENT BREAKDOWN ███    │
│  [  PULSING AMBER BACKGROUND  ] │
├─────────────────────────────────┤
│                                 │
│  📍 1.8 km away                 │
│     Est. Travel: 6 mins         │
│                                 │
│  🚗 Maruti Swift | Petrol       │
│                                 │
│  📝 "Car stopped suddenly,      │
│      smoke from bonnet"         │
│                                 │
│  ┌──────────────────────────┐   │
│  │    ( 45s countdown ring) │   │
│  │                          │   │
│  │     [ ACCEPT JOB ✓ ]     │   │
│  │                          │   │
│  └──────────────────────────┘   │
│                                 │
│          Skip / Pass Job        │
│                                 │
└─────────────────────────────────┘
```

**Behavior:**
- Overrides screen lock (foreground service / notification channel: IMPORTANCE_HIGH)
- Loud distinct ringtone (not system notification)
- 60s auto-decline if no action
- Pass = soft decline; system offers to next nearest shop

---

#### Screen M-2.2 — Job Accepted — Mandatory Call Interlock

```
┌─────────────────────────────────┐
│  ⚠️  MANDATORY STEP             │
├─────────────────────────────────┤
│  You must call the customer     │
│  to diagnose the problem and    │
│  discuss required parts         │
│  BEFORE leaving the shop.       │
│                                 │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│  Customer: Janarthanan S        │
│  Vehicle:  Maruti Swift Petrol  │
│  Issue:    Smoke from bonnet    │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                 │
│       [ 📞 CALL CUSTOMER ]      │
│                                 │
│  [ MAP / DISPATCH ] — LOCKED 🔒 │
│                                 │
│  Navigation unlocks after:      │
│  ✅ Call completed               │
│  ✅ Parts & costs logged         │
│  ✅ Customer approves bill       │
│                                 │
└─────────────────────────────────┘
```

**State machine note:** Button `[MAP / DISPATCH]` is disabled until `call_logged=true AND quote_approved=true`

---

### MODULE M-3: Digital Job Sheet & Parts Cost Logger

---

#### Screen M-3.1 — Live Invoice & Checklist Builder

*(Unlocks after call is placed via app)*

```
┌─────────────────────────────────┐
│  📋 Job Sheet                   │
│  Maruti Swift | Rajan Kumar     │
├─────────────────────────────────┤
│  A: SERVICE CHECKLIST (Labor)   │
│                                 │
│  [x] Basic Checkup    ₹  299   │
│  [x] Brake Cleaning   ₹  499   │
│  [ ] Battery Jumpstart ₹  399  │
│  [ ] Oil Change        ₹  349  │
│  [ ] AC Inspection     ₹  449  │
│                                 │
│  ─────────────────────────────  │
│  B: SPARE PARTS (Dynamic)       │
│                                 │
│  [ Castrol 5W30 Oil    ] [2500] │
│  [ Oil Filter          ] [ 450] │
│  [ + Add Another Part  ]        │
│                                 │
│  ─────────────────────────────  │
│  C: LIVE TOTAL                  │
│                                 │
│  Labor:             ₹   499     │
│  Parts:             ₹ 2,950     │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│  TOTAL TO BILL:     ₹ 3,449     │
│                                 │
│  [ SEND FOR USER APPROVAL → ]   │
│                                 │
└─────────────────────────────────┘
```

**After Send:**
```
⏳ Waiting for customer approval...
   Bill sent: ₹3,449
   [Customer notified via push]
```

---

### MODULE M-4: Dispatch & Execution

---

#### Screen M-4.1 — Dispatch Unlocked

```
┌─────────────────────────────────┐
│  ✅ CUSTOMER APPROVED!          │
│  Bill Authorized: ₹3,449        │
├─────────────────────────────────┤
│                                 │
│  [  GOOGLE MAPS NAVIGATION    ] │
│  [  Turn-by-turn to customer  ] │
│                                 │
│  📋 Parts Reminder:             │
│  ┌──────────────────────────┐   │
│  │ ☐ Castrol 5W30 Oil (4L)  │   │
│  │ ☐ Oil Filter             │   │
│  └──────────────────────────┘   │
│                                 │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│  [ ▶▶▶ SLIDE TO START JOURNEY]  │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                 │
└─────────────────────────────────┘
```

---

#### Screen M-4.2 — At Customer Location / Job Execution

```
┌─────────────────────────────────┐
│  🔧 Job Active                  │
│  Maruti Swift — Rajan Kumar     │
├─────────────────────────────────┤
│  Arrived: 10:45 AM              │
│  OTP from customer: [ 8842 ]    │
│  [ VERIFY OTP & START JOB ]     │
│                                 │
│  Authorized Bill: ₹3,449        │
│                                 │
│  [ VIEW JOB SHEET ]             │
│  [ 📞 CALL CUSTOMER ]           │
│                                 │
│  ─────────────────────────────  │
│  [ MARK JOB COMPLETE ✓ ]        │
└─────────────────────────────────┘
```

---

### MODULE M-5: Earnings & Subscription Dashboard

---

#### Screen M-5.1 — Real-Time Business Ledger

```
┌─────────────────────────────────┐
│  📊 My Dashboard — June 2026    │
├─────────────────────────────────┤
│                                 │
│  ┌────────┐ ┌────────┐ ┌──────┐ │
│  │  42    │ │₹76,400 │ │ Tier │ │
│  │ Jobs   │ │Revenue │ │  2   │ │
│  └────────┘ └────────┘ └──────┘ │
│                                 │
│  Revenue Breakdown:             │
│  Labor:  ₹18,200                │
│  Parts:  ₹58,200                │
│                                 │
│  Subscription Tier Progress:    │
│  Tier 1     Tier 2     Tier 3   │
│  [████████████████░░░░░░░░░░░]  │
│  42 / 50 jobs | Tier 2 Active   │
│                                 │
│  Next renewal: Jul 1 — ₹999     │
│                                 │
│  [ VIEW ALL JOBS ]              │
│                                 │
└─────────────────────────────────┘
```

---

#### Screen M-5.2 — Monthly Renewal Overlay (1st of Every Month)

```
┌─────────────────────────────────┐
│  🗓️  Monthly Renewal — Jul 2026  │
├─────────────────────────────────┤
│                                 │
│  LAST MONTH SUMMARY             │
│  Total Jobs Logged:    42       │
│  Total Business:  ₹76,400       │
│  Your Tier:       Tier 2        │
│                                 │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│  AMOUNT DUE:           ₹999     │
│  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━  │
│                                 │
│  Pay to activate next 30 days:  │
│                                 │
│  [ QR CODE — UPI ]              │
│                                 │
│  mechondemand@upi               │
│                                 │
│  [ Open PhonePe ] [ Open GPay ] │
│                                 │
│  ⚠️ Account paused until payment │
│                                 │
└─────────────────────────────────┘
```

---

## 7. Core Workflows & State Machines

### 7.1 Job Lifecycle State Machine

```
CREATED
   │
   ├──[Urgent]──► MATCHING ──► ACCEPTED ──► CALL_PENDING
   │                                              │
   └──[Scheduled]──► CONFIRMED ──────────► CALL_PENDING
                                                  │
                                           QUOTE_SENT
                                                  │
                              ┌───────────────────┤
                              │                   │
                         REJECTED            QUOTE_APPROVED
                              │                   │
                           CANCELLED          DISPATCHED
                                                  │
                                             IN_PROGRESS
                                                  │
                                             COMPLETED
                                                  │
                                             PAID / RATED
```

### 7.2 Mechanic Dispatch Interlock

```
Job Accepted
     │
     ▼
[CALL_PENDING state]
     │ Mechanic places call via app
     ▼
[SHEET_FILLING state]
     │ Mechanic logs checklist + parts + costs
     ▼
[QUOTE_SENT state]
     │ Push notification → customer app
     ▼
Customer decision:
     ├── APPROVE → job.status = QUOTE_APPROVED → dispatch unlocks
     └── REJECT  → job.status = CANCELLED → mechanic notified
```

### 7.3 Urgent SOS Match Flow

```
Customer taps SOS
     │
     ▼
GPS captured → query workshops within 5km radius
     │
     ▼
Sort by: distance ASC + rating DESC + online_status = ACTIVE
     │
     ▼
Push flash alert to Shop #1 (60s window)
     │
     ├── Accept → job assigned
     └── Timeout / Decline → Push to Shop #2 (60s window)
                                   └── ... repeat up to 5 shops
                                   └── No match → "No mechanics available, try again"
```

---

## 8. API Integrations

| Integration | Purpose | Endpoint / Service |
|------------|---------|-------------------|
| **Vahan API** | Vehicle details from registration number | `https://vahan.nic.in` or MoRTH API |
| **GST Verify API** | Validate GSTIN of workshops | `https://api.gst.gov.in/commonapi/v1.1/search` |
| **Google Maps SDK** | Map display, turn-by-turn nav, distance matrix | Google Maps Platform |
| **Google Places API** | Address autocomplete, geocoding | Google Maps Platform |
| **Firebase FCM** | Push notifications (booking alerts, approvals) | Firebase Cloud Messaging |
| **UPI Intent** | Deep-link to PhonePe/GPay for subscription payment | `upi://pay?pa=...` intent |
| **Twilio / AWS SNS** | OTP SMS delivery | Twilio Verify or AWS SNS |
| **In-App Call** | Voice call between customer and mechanic via app | Twilio Voice / Agora / Daily.co |

### 8.1 Vahan API Request/Response

```
GET /rto/vehicle?reg=TN07CX1234

Response:
{
  "reg_no": "TN07CX1234",
  "make": "Hyundai",
  "model": "i20 Asta",
  "year": 2022,
  "fuel_type": "Petrol",
  "owner_masked": "J***NATH S",
  "engine_masked": "KxxxxxX"
}
```

### 8.2 GST Verify Request

```
GET https://api.gst.gov.in/commonapi/v1.1/search?gstin=33AABCE1234F1Z5

Response:
{
  "gstin": "33AABCE1234F1Z5",
  "tradeNam": "Express Auto Care",
  "sts": "Active",
  "ctb": "Tamil Nadu"
}
```

---

## 9. Non-Functional Requirements

### 9.1 Performance

| Metric | Target |
|--------|--------|
| API response time (P95) | < 300ms |
| Urgent SOS match latency | < 5s for first ping |
| Push notification delivery | < 2s |
| Map render time | < 1.5s |
| App cold start | < 2s (Android), < 1.5s (iOS) |

### 9.2 Availability

| Component | SLA |
|-----------|-----|
| Core API | 99.9% uptime |
| Push service | 99.5% |
| RTO/GST API (3rd party) | Degrade gracefully — manual fallback |

### 9.3 Scalability

- Stateless API server — horizontal scaling via container orchestration
- WebSocket connections for live job tracking — Redis pub/sub for fan-out
- Geo queries via PostGIS `ST_DWithin` — indexed on workshop location

### 9.4 Offline Handling

- Customer app: cache nearby workshops, vehicle card, booking history
- Mechanic app: job sheet fillable offline; syncs when connection restored
- No silent data loss — queue offline writes in local SQLite, flush on reconnect

---

## 10. Data Models

### 10.1 Core Tables

```sql
-- Users (customers)
CREATE TABLE users (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  phone         VARCHAR(15) UNIQUE NOT NULL,
  name          VARCHAR(100),
  created_at    TIMESTAMPTZ DEFAULT NOW()
);

-- Vehicles
CREATE TABLE vehicles (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id       UUID REFERENCES users(id),
  reg_number    VARCHAR(20) UNIQUE NOT NULL,
  make          VARCHAR(50),
  model         VARCHAR(100),
  year          SMALLINT,
  fuel_type     VARCHAR(20),
  created_at    TIMESTAMPTZ DEFAULT NOW()
);

-- Workshops
CREATE TABLE workshops (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name          VARCHAR(150) NOT NULL,
  owner_name    VARCHAR(100),
  phone         VARCHAR(15),
  gstin         VARCHAR(20) UNIQUE NOT NULL,
  gst_status    VARCHAR(20) DEFAULT 'PENDING',
  location      GEOGRAPHY(POINT, 4326),
  service_radius_km  SMALLINT DEFAULT 5,
  vehicle_types TEXT[],
  is_active     BOOLEAN DEFAULT FALSE,
  created_at    TIMESTAMPTZ DEFAULT NOW()
);
CREATE INDEX idx_workshops_location ON workshops USING GIST(location);

-- Jobs
CREATE TABLE jobs (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  customer_id     UUID REFERENCES users(id),
  vehicle_id      UUID REFERENCES vehicles(id),
  workshop_id     UUID REFERENCES workshops(id),
  service_tier    VARCHAR(20),         -- BASIC | MEDIUM | COMPREHENSIVE
  booking_type    VARCHAR(20),         -- URGENT | SCHEDULED
  issue_text      TEXT,
  parts_authorized BOOLEAN DEFAULT FALSE,
  scheduled_slot  TIMESTAMPTZ,
  status          VARCHAR(30) DEFAULT 'CREATED',
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);

-- Job Quote (parts + labor logged by mechanic)
CREATE TABLE job_quotes (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  job_id          UUID REFERENCES jobs(id),
  labor_items     JSONB,   -- [{"name": "Oil Change", "cost": 499}]
  parts_items     JSONB,   -- [{"name": "Castrol 5W30", "cost": 2500}]
  labor_total     NUMERIC(10,2),
  parts_total     NUMERIC(10,2),
  grand_total     NUMERIC(10,2),
  call_logged_at  TIMESTAMPTZ,
  sent_at         TIMESTAMPTZ,
  approved_at     TIMESTAMPTZ,
  rejected_at     TIMESTAMPTZ
);

-- Monthly Subscription Ledger
CREATE TABLE subscription_ledger (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  workshop_id     UUID REFERENCES workshops(id),
  month           DATE,               -- first day of month
  jobs_completed  INTEGER DEFAULT 0,
  gross_revenue   NUMERIC(12,2) DEFAULT 0,
  tier            SMALLINT,           -- 1 | 2 | 3 | 4
  fee_due         NUMERIC(8,2),
  paid_at         TIMESTAMPTZ,
  upi_txn_id      VARCHAR(100)
);
```

---

## 11. Subscription & Billing Engine

### 11.1 Tier Calculation Logic

```
Monthly jobs completed → determine tier → fee due on 1st of next month

Tier 1:   1–20  jobs → ₹499
Tier 2:  21–50  jobs → ₹999
Tier 3:  51–100 jobs → ₹1,499
Tier 4:  100+   jobs → ₹1,999
```

```go
// Go pseudocode
func calculateTier(jobsCompleted int) (tier int, fee float64) {
    switch {
    case jobsCompleted <= 20:
        return 1, 499
    case jobsCompleted <= 50:
        return 2, 999
    case jobsCompleted <= 100:
        return 3, 1499
    default:
        return 4, 1999
    }
}
```

### 11.2 Month-End Cron Job

```
0 0 1 * * → Run month-end settlement
  1. Aggregate jobs_completed per workshop for last month
  2. Calculate tier + fee
  3. Write to subscription_ledger
  4. Push renewal notification to mechanic app
  5. Set workshop.is_active = FALSE (reactivates on payment confirmation)
```

### 11.3 Payment Confirmation

- Mechanic pays via UPI to platform VPA
- Webhook from payment processor OR manual UPI UTR verification
- On confirmation: `workshop.is_active = TRUE`, push "Account Activated" notification
- **No Razorpay per-transaction fee** — single monthly lump sum

---

## 12. Security Requirements

| Area | Requirement |
|------|-------------|
| Auth | JWT with 15-min access token + 30-day refresh token, stored in secure storage |
| OTP | 6-digit, 5-min expiry, max 3 attempts before 30-min lockout |
| GSTIN | Server-side validation only — never trust client assertion |
| Vehicle data | Mask sensitive fields (engine/chassis) — show partial only |
| Call privacy | In-app call via masked number relay (Twilio Proxy) — never expose direct phone |
| API | Rate limiting: 100 req/min per IP, 10 req/min on OTP endpoint |
| Transport | TLS 1.3 mandatory; HSTS on all web endpoints |
| Payments | UPI intent only — no card data stored; PCI-DSS not applicable |
| Audit log | All job status transitions logged with timestamp + actor |

---

## 13. Tech Stack Recommendation

| Layer | Technology | Rationale |
|-------|-----------|-----------|
| Mobile | Flutter (single codebase) | Android + iOS, good maps/push support |
| Backend API | Go (chi router) | Low latency, concurrency for WS + geo queries |
| Database | PostgreSQL 16 + PostGIS | Geo queries, JSONB for flexible parts data |
| Cache / Pub-Sub | Redis 7 | WS fan-out, session store, rate limiting |
| Push | Firebase FCM | Reliable delivery on Android + iOS |
| Maps | Google Maps Platform | Maps SDK + Navigation SDK + Distance Matrix |
| OTP / SMS | Twilio Verify | Reliable Indian delivery |
| Voice Relay | Twilio Proxy | Masked number calling |
| Hosting | Hetzner + Coolify | Cost-effective, self-hosted, sovereign |
| File Storage | Cloudflare R2 | GST docs, job photos |
| Auth | JWT + custom OTP service | No Firebase Auth dependency |

---

## 14. Out of Scope (v1.0)

| Feature | Status | Notes |
|---------|--------|-------|
| Parts catalog / SKU inventory | ❌ Out of scope | Replaced by open text + mechanic procurement |
| In-app payment gateway for job billing | ❌ Out of scope | Customer pays mechanic directly (cash / UPI) |
| Multi-mechanic dispatch per job | ❌ v2 | Single mechanic per job in v1 |
| Insurance / warranty module | ❌ v2 | — |
| Fleet / B2B accounts | ❌ v2 | — |
| Web customer portal | ❌ v2 | Mobile-first v1 |
| Analytics dashboard for admin | ❌ v2 | Basic admin panel only in v1 |
| Referral / loyalty program | ❌ v2 | — |

---

*Document owned by Retrotech Pvt Ltd / Black Lover Tech. Internal use only.*
