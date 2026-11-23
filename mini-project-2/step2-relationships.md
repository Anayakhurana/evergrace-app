# Step 2 — Identify and Justify Every Relationship

## 1. Video — SafetyFlag

**Relationship Type:** Composition

**Multiplicity:** Video (1) — SafetyFlag (0..*)

**Roles:**
- Video: video
- SafetyFlag: safetyFlags

**Justification:**

A SafetyFlag belongs to a specific Video and has no meaningful purpose without that video. If a video is deleted, its associated safety flags should also be deleted. Therefore, this relationship is composition.

## 2. SafetyFlag — SafetyAuditLog

**Relationship Type:** Association

**Multiplicity:** SafetyFlag (1) — SafetyAuditLog (0..*)

**Roles:**
- SafetyFlag: safetyFlag
- SafetyAuditLog: auditHistory

**Justification:**

A SafetyAuditLog entry records changes made to a specific SafetyFlag. However, audit records must be preserved as permanent history, even if a safety flag is removed. Therefore, the audit log should exist independently of the SafetyFlag, making association the appropriate relationship.

## 3. User — SafetyFlag

**Relationship Type:** Association

**Multiplicity:** User (1) — SafetyFlag (0..*)

**Roles:**
- User: createdBy
- SafetyFlag: creator

**Justification:**

A User (administrator) creates safety flags but exists independently of them. The user does not own the lifecycle of the safety flags. Therefore, this relationship is association.

## 4. User — SafetyAuditLog

**Relationship Type:** Association

**Multiplicity:** User (1) — SafetyAuditLog (0..*)

**Roles:**
- User: admin
- SafetyAuditLog: auditEntries

**Justification:**

A User (administrator) performs changes to safety flags, and the SafetyAuditLog records which administrator performed each change. The administrator exists independently of the audit records. Therefore, this relationship is association.

## 5. Classic Pattern Parallel

The Video–SafetyFlag relationship is structurally similar to the Vehicle–VehiclePart composition pattern. A SafetyFlag is created for a specific video and has no meaningful purpose without that video. Similarly, a vehicle part that belongs to a specific vehicle may have no independent purpose outside that vehicle. In both cases, the whole controls the lifecycle of its parts, making composition the appropriate relationship type.
