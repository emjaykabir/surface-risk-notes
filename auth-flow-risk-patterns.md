# Authentication Flow Risk Patterns

## Scope

This note documents **common risk patterns** observed in authentication and access flows of modern web applications.
It focuses on **logic assumptions and surface-level design choices**, not on exploitation techniques or vulnerability discovery.

The observations below are **pattern-based and non-intrusive**.

---

## Common Assumptions That Fail

### 1. Authentication Equals Authorization

Many applications assume that a successful login automatically implies correct access rights.
This often leads to:

* Overly broad default permissions
* Insufficient checks when accessing secondary features
* Implicit trust in session state rather than explicit role validation

**Why it matters:**
Access control errors tend to have higher real-world impact than isolated input validation issues.

---

### 2. Password Reset Is Treated as a Secondary Flow

Password reset functionality is frequently designed as an exception rather than a core authentication path.

Common issues include:

* Weak rate limiting on reset attempts
* Overly verbose error responses
* Inconsistent validation between login and reset flows

**Why it matters:**
Attackers often target reset flows because they bypass primary login protections.

---

### 3. Role Boundaries Are Defined Implicitly

Role separation is often implemented through frontend logic or assumed URL structure rather than enforced server-side checks.

This results in:

* Reliance on hidden UI elements
* Trust in client-side role indicators
* Inconsistent access checks across endpoints

**Why it matters:**
Implicit boundaries tend to break as applications evolve.

---

## Misconfiguration Patterns

### 1. Inconsistent Session Handling

Examples include:

* Multiple session cookies with overlapping purposes
* Long-lived sessions without revalidation
* Missing invalidation on sensitive state changes

**Impact:**
Session confusion increases the likelihood of unintended access persistence.

---

### 2. Environment Leakage

Applications may expose:

* Debug modes in production
* Environment-specific banners or flags
* Configuration-dependent behavior visible to unauthenticated users

**Impact:**
Even without direct exploitation, this information reduces uncertainty for attackers.

---

## What Matters vs. What Does Not

**Higher priority:**

* Clear, server-side authorization checks
* Consistent handling of authentication state
* Predictable and minimal error messaging

**Lower priority:**

* Cosmetic UI protections
* Obscure URL paths without enforcement
* Complex client-side logic without backend validation

---

## Defensive Observations

* Treat authentication and authorization as separate decisions
* Design password reset flows with the same rigor as login
* Enforce role checks on every sensitive action
* Assume application structure will change over time

---

## Closing Note

Most authentication-related risk arises from **design assumptions**, not from complex technical flaws.
Surface-level clarity and consistency often reduce risk more effectively than adding new controls.
