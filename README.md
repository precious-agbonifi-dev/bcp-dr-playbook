# Business Continuity & Disaster Recovery Playbook

A practical BCP/DR playbook designed for cloud-hosted and hybrid environments. Based on real-world implementation experience across public sector organisations including the Greater London Authority, The British Library, and MOPAC.

---

## 📐 Framework Overview

This playbook follows a four-phase lifecycle:

```
┌─────────────────────────────────────────────────────┐
│                  BCP/DR LIFECYCLE                   │
│                                                     │
│  1. ASSESS → 2. DESIGN → 3. IMPLEMENT → 4. TEST    │
│                                                     │
│  Business       RTO/RPO       Run Books      DR     │
│  Impact         Targets       & Playbooks    Drills │
│  Analysis                                           │
└─────────────────────────────────────────────────────┘
```

---

## Phase 1: Business Impact Analysis (BIA)

Before designing any recovery solution, identify what matters most.

### BIA Template

| System / Process | Business Function | Max Tolerable Downtime | RTO Target | RPO Target | Priority |
|---|---|---|---|---|---|
| Core ERP | Financial reporting | 4 hours | 2 hours | 1 hour | P1 |
| Email / Comms | Organisation-wide | 8 hours | 4 hours | 4 hours | P2 |
| HR System | Payroll & records | 24 hours | 8 hours | 24 hours | P3 |
| Intranet | Internal comms | 72 hours | 24 hours | 24 hours | P4 |

**Key definitions:**
- **RTO (Recovery Time Objective)** — how fast you must restore service
- **RPO (Recovery Point Objective)** — how much data loss is acceptable
- **MTD (Maximum Tolerable Downtime)** — absolute limit before business harm is critical

---

## Phase 2: Recovery Strategy Design

### Cloud DR Patterns by Tier

```
P1 Systems — Active/Active Multi-Region
┌──────────────┐     ┌──────────────┐
│  Region A    │◄───►│  Region B    │  RTO: <15 min
│  (Primary)   │     │  (Secondary) │  RPO: Near-zero
└──────────────┘     └──────────────┘

P2 Systems — Warm Standby
┌──────────────┐     ┌──────────────┐
│  Region A    │────►│  Region B    │  RTO: 1-4 hours
│  (Primary)   │     │  (Scaled     │  RPO: 1 hour
└──────────────┘     │   down)      │
                     └──────────────┘

P3/P4 Systems — Backup & Restore
┌──────────────┐     ┌──────────────┐
│  Production  │────►│  S3/GCS      │  RTO: 8-24 hours
│              │     │  Backups     │  RPO: 24 hours
└──────────────┘     └──────────────┘
```

---

## Phase 3: Incident Response Runbook

### Severity Levels

| Level | Description | Response Time | Escalation |
|---|---|---|---|
| SEV-1 | Full outage — P1 system down | 15 minutes | CISO + CTO immediately |
| SEV-2 | Partial outage — degraded service | 1 hour | IT Director |
| SEV-3 | Non-critical system impacted | 4 hours | IT Manager |
| SEV-4 | Minor issue, workaround available | Next business day | Team lead |

### SEV-1 Response Steps

```
00:00 — Incident declared. Incident Commander assigned.
00:15 — Initial triage. Confirm scope and affected systems.
00:30 — DR bridge call opened. Key stakeholders notified.
01:00 — Recovery decision: failover vs restore vs workaround.
01:00 — Comms drafted. Status page updated.
[ongoing] — 30-minute update cadence to stakeholders.
[recovery] — Systems restored. Validation tests run.
[close]   — Post-incident review scheduled within 5 days.
```

---

## Phase 4: DR Testing Schedule

| Test Type | Frequency | Duration | Participants |
|---|---|---|---|
| Tabletop exercise | Quarterly | 2 hours | Leadership + IT |
| Component failover test | Bi-annual | 4 hours | IT Operations |
| Full DR simulation | Annual | 1–2 days | All teams |
| Backup restoration test | Monthly | 1 hour | Infrastructure |

### Test Report Template

```markdown
## DR Test Report — [Date]

**Test Type:** [Tabletop / Failover / Full Simulation]
**Systems in Scope:** 
**RTO Target:** [X hours]  |  **RTO Achieved:** [X hours]
**RPO Target:** [X hours]  |  **RPO Achieved:** [X hours]

### What Went Well
-

### Issues Identified
-

### Action Items
| Action | Owner | Due Date |
|---|---|---|

### Sign-off
IT Director: _____________  Date: _____________
```

---

## 📄 License

MIT — adapt freely with attribution.
