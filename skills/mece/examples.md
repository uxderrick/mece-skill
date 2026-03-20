# MECE Examples — Before & After

Real-world examples showing the full validate → ask → fix → confirm flow.

---

## Example 1: The Protein Problem

**Context:** You're ordering food with a one-protein-per-meal restriction.

**User's input:**
```
Order: Fish, Rice, Salad, Eggs
```

**MECE Report:**
```
MECE Score: 70/100

🔴 OVERLAP: "Fish" ↔ "Eggs"
   Both are proteins. You've ordered the same nutritional category twice
   under different items. Pick one.

Fix this now?
```

**User says yes:**
```
✓ Removed "Eggs" — duplicate protein category. Kept "Fish".

Order (by food group):
├── Protein: Fish
├── Carb: Rice
└── Vegetable: Salad

MECE Score: 100/100 — All things are MECE. ✓
```

---

## Example 2: The Packing List

**Context:** Packing for a trip.

**User's input:**
```
Packing list: Clothes, Shoes, Toiletries
```

**MECE Report:**
```
MECE Score: 40/100

🔴 CRITICAL GAP: Electronics (chargers, adapters, headphones)
   You'll arrive and have dead devices with no way to charge them.

🔴 CRITICAL GAP: Documents (passport, tickets, insurance)
   Can't travel without ID and travel docs.

🟡 GAP: Medications / health items
   Prescription meds, first aid — can't buy these easily at destination.

🟡 GAP: Accessories (sunglasses, watch, belt)
   Not critical but commonly forgotten items.

Fix these now?
```

**User says yes:**
```
✓ Added "Electronics" (was critical gap)
✓ Added "Documents" (was critical gap)
✓ Added "Health" (was moderate gap)
✓ Added "Accessories" (was moderate gap)

Packing list:
├── Clothing (tops, bottoms, outerwear, underwear)
├── Footwear (daily shoes, activity-specific, formal)
├── Toiletries (hygiene, skincare, haircare)
├── Electronics (devices, chargers, adapters)
├── Documents (ID, travel docs, insurance, itinerary)
├── Health (medications, first aid, supplements)
└── Accessories (eyewear, jewelry, bags)

MECE Score: 100/100 — All things are MECE. ✓
```

---

## Example 3: Product Feature Spec

**Context:** A PM writes a feature list for a dashboard app.

**User's input:**
```
Dashboard features:
- Analytics and reporting
- Data visualization (charts, graphs)
- Export to PDF and CSV
- User management
- Role-based access control
- Real-time updates
```

**MECE Report:**
```
MECE Score: 45/100

🔴 OVERLAP: "Analytics and reporting" ↔ "Data visualization"
   Charts and graphs ARE analytics. This is the same concern split
   into two line items. Two teams could build the same thing.

🔴 OVERLAP: "User management" ↔ "Role-based access control"
   RBAC is a subset of user management, not a separate feature.

🔴 CRITICAL GAP: No data source/input layer
   The dashboard consumes data but nothing covers where it comes from,
   how it connects, or how it syncs. This will block implementation.

🟡 GAP: No notifications or alerts
   Dashboards need threshold alerts — without this, users have to
   manually check for changes.

🟡 GAP: No settings or configuration
   No category for user preferences, integrations, or customization.

Fix these now?
```

**User says yes:**
```
✓ Merged "Analytics and reporting" + "Data visualization" → "Visualization"
✓ Merged "User management" + "Role-based access control" → "Access Control"
✓ Added "Data Layer" (was critical gap)
✓ Added "Notifications" (was moderate gap)
✓ Added "Configuration" (was moderate gap)

Dashboard features:
├── Data Layer (sources, connections, sync, import)
├── Visualization (charts, tables, maps, real-time displays)
├── Actions (export, share, schedule, print)
├── Access Control (users, roles, permissions, audit log)
├── Notifications (alerts, thresholds, digests)
└── Configuration (settings, preferences, integrations)

MECE Score: 100/100 — All things are MECE. ✓
```

---

## Example 4: API Endpoint Design

**Context:** Designing REST endpoints for a task management API.

**User's input:**
```
Endpoints:
- /tasks — CRUD operations
- /tasks/assign — Assign task to user
- /tasks/complete — Mark task complete
- /users — User management
- /teams — Team management
- /users/teams — Get user's teams
```

**MECE Report:**
```
MECE Score: 50/100

🔴 OVERLAP: "/tasks/assign" and "/tasks/complete" ↔ "/tasks CRUD"
   Assignment and completion are state mutations — they're PATCH /tasks/:id,
   not separate endpoints. This creates two ways to do the same thing.

🟡 OVERLAP: "/users/teams" ↔ "/teams"
   "Get user's teams" could live under /users/:id/teams OR /teams?userId=X.
   Having both creates ambiguity for API consumers.

🔴 CRITICAL GAP: No error/health endpoints
   No way to check if the API is running or diagnose issues.

🟡 GAP: No search or filtering
   Six resources but no way to search across them.

🟢 MINOR GAP: No batch operations
   Nice-to-have for bulk task updates.

Fix these now?
```

**User says yes:**
```
✓ Absorbed /tasks/assign and /tasks/complete into /tasks CRUD (state transitions via PATCH)
✓ Removed /users/teams — user's teams accessed via /users/:id/teams relationship
✓ Added /system (was critical gap)
✓ Added /search (was moderate gap)

Endpoints (by resource):
├── /tasks — CRUD + state transitions (assign, complete, archive via PATCH)
├── /users — CRUD + relationships (GET /users/:id/tasks, /users/:id/teams)
├── /teams — CRUD + membership (GET /teams/:id/members)
├── /search — Cross-resource search (tasks, users, teams)
└── /system — Health check, version, config

MECE Score: 100/100 — All things are MECE. ✓
```

---

## Example 5: State Management (React Reducer)

**Context:** Defining actions for a queue management reducer.

**User's input:**
```
Actions: ADD_TO_QUEUE, REMOVE_FROM_QUEUE, UPDATE_TICKET, MARK_AS_SERVING,
CALL_NEXT, HOLD_TICKET, COMPLETE_TICKET, TRANSFER_TICKET, CANCEL_TICKET, SET_QUEUE
```

**MECE Report:**
```
MECE Score: 50/100

🔴 OVERLAP: "MARK_AS_SERVING" ↔ "CALL_NEXT"
   "Call next" marks the next ticket as serving. These are the same state
   transition triggered differently. Two handlers for one mutation.

🔴 OVERLAP: "REMOVE_FROM_QUEUE" ↔ "CANCEL_TICKET" / "COMPLETE_TICKET"
   Completing and cancelling both remove from queue. Is REMOVE_FROM_QUEUE a
   separate action or the shared side effect? Ambiguous dispatch logic.

🟡 GAP: No action for reordering / priority changes
   Queue order can't be adjusted without a dedicated action.

🟡 GAP: No action for bulk operations (clear all, reset)
   No way to reset the queue state in one dispatch.

🟢 MINOR GAP: "UPDATE_TICKET" is too broad
   Covers metadata updates AND status changes — should be split by intent.

Fix these now?
```

**User says yes:**
```
✓ Removed MARK_AS_SERVING — absorbed into CALL_NEXT
✓ Removed REMOVE_FROM_QUEUE — COMPLETE/CANCEL/NO_SHOW each handle their own exit
✓ Split UPDATE_TICKET → UPDATE_PRIORITY + UPDATE_NOTES (by intent)
✓ Added CLEAR_QUEUE (was moderate gap)
✓ Added RESUME action (held → serving was missing)

Actions (by transition type):
├── Lifecycle (ticket enters/exits queue)
│   ├── ENQUEUE — new ticket added
│   └── DEQUEUE — ticket removed (completed, cancelled, or no-show)
├── Status transitions (ticket changes state)
│   ├── CALL_NEXT — waiting → serving
│   ├── HOLD — serving → held
│   ├── RESUME — held → serving
│   ├── TRANSFER — serving → transferred
│   └── COMPLETE — serving → completed
├── Mutations (data changes, no status change)
│   ├── UPDATE_PRIORITY — reorder within queue
│   └── UPDATE_NOTES — add/edit ticket notes
└── Bulk (whole-queue operations)
    ├── SET_QUEUE — replace entire queue (sync/init)
    └── CLEAR_QUEUE — remove all tickets

MECE Score: 100/100 — All things are MECE. ✓
```

---

## Example 6: Roadmap Phases

**Context:** A startup planning their product roadmap.

**User's input:**
```
Roadmap:
- MVP features
- Core platform
- Growth features
- Enterprise features
- Technical debt
- Nice-to-haves
```

**MECE Report:**
```
MECE Score: 25/100

🔴 OVERLAP: "MVP features" ↔ "Core platform"
   The MVP IS the core platform. These aren't distinct phases — they
   describe the same work with different names.

🔴 OVERLAP: "Growth features" ↔ "Enterprise features"
   Enterprise IS a growth segment. A feature could easily be both
   "growth" and "enterprise." Blurry ownership.

🔴 OVERLAP: "Technical debt" crosses all phases
   Tech debt isn't a phase — it's a concern that applies everywhere.
   Mixing concerns with phases guarantees overlap.

🔴 CRITICAL GAP: No infrastructure / DevOps / reliability work
   Nothing covers deployment, monitoring, or scaling.

🟡 GAP: No compliance / security phase
   Audit trails, encryption, certifications — not covered anywhere.

🟡 GAP: "Nice-to-haves" is an undefined catch-all
   Everything that doesn't fit goes here. That's a sign the other
   categories aren't exhaustive.

Fix these now?
```

**User says yes:**
```
✓ Merged "MVP features" + "Core platform" → "Phase 1: Foundation"
✓ Split "Growth" and "Enterprise" by phase, not segment
✓ Removed "Technical debt" as a phase — addressed within each phase
✓ Added "Phase 3: Scale" (was critical gap — infra/DevOps)
✓ Added "Phase 4: Compliance" (was moderate gap)
✓ Replaced "Nice-to-haves" → "Deferred" with explicit rationale required

Roadmap:
├── Phase 1: Foundation (auth, core data model, base UI, infra)
├── Phase 2: Core Workflows (primary user journeys, integrations)
├── Phase 3: Scale (performance, multi-tenancy, monitoring)
├── Phase 4: Compliance (audit trails, encryption, certifications)
├── Phase 5: Advanced (analytics, automation, marketplace)
└── Deferred (explicitly scoped out with rationale for each item)

MECE Score: 100/100 — All things are MECE. ✓
```

---

## Example 7: Already MECE

**Context:** User submits a well-structured breakdown.

**User's input:**
```
Customer segments by ARR:
- Enterprise (>$100K)
- Mid-Market ($10K–$100K)
- SMB ($1K–$10K)
- Free (<$1K)
```

**MECE Report:**
```
MECE Score: 100/100

Categories reviewed: 4
Overlaps found: 0
Gaps found: 0

All things are MECE. ✓
```

---

## Pattern: The MECE Validation Prompt

Use this after any structured output to validate it:

```
Review the structure above for MECE compliance:

1. Mutual Exclusivity — Can any item fit into more than one category?
   Are there concepts described with different names in different sections?

2. Collective Exhaustiveness — What real-world scenarios or items are NOT
   covered by any category? What would a domain expert immediately flag
   as missing?

Score it, show the findings, and ask me before fixing.
```
