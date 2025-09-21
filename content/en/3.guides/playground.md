---
title: GNAP Playground
description: Experiment with GNAP grants, device codes, and access flows in a safe demo environment. Perfect for learning how authorization works without setting up a full deployment.
navigation:
  icon: i-lucide-terminal
---
# TwigBush Playground Demo

Modern products need to let software act on behalf of people.  With current solutions, it's challenging to scope the correct permissions, for the right time, and with a clear audit trail.

This playground demonstrates how a **GNAP Authorization Server (AS)** handles that safely.

---

## Core Principles

* **Human in the loop**: A user explicitly approves what an app/agent is allowed to do.
* **Just-in-time access**: Short-lived permissions are granted only when needed.
* **Delegation you can trust**: Tokens are bound to a client key (proof-of-possession), reducing theft risks.
* **Clear lifecycle**: Each authorization (“grant”) moves through states: `processing → pending → approved/denied → finalized`.

---

## Who Should Care

* **Product & Platform leaders**: Reduce risk and improve UX with explicit approvals and time-boxed access.
* **Security & Compliance**: See how consent, short TTLs, and auditable state changes align with least-privilege and zero-trust.
* **Developers**: Learn a realistic GNAP flow you can integrate with UI, backends, and policy engines (e.g., OpenFGA).

---

## Typical Scenarios

### 1. Customer Checkout (Human Present, Device Code UX)

* Customer initiates purchase (e.g., *“buy 100 GPU hours for \$19.99”*).
* Shopping app requests a GNAP grant.
* User enters a code and approves on a secure consent screen.
* AS issues a short-lived, key-bound token.
* RS validates the token and fulfills the purchase.

**Value**: explicit consent, reduced fraud, no standing permissions.

---

### 2. Agent Action (Human Not Present, Intent-Bound)

* User tells agent: *“When X happens, purchase Y (up to \$Z, before <date>)”*.
* Agent requests a constrained grant (amount, merchant, expiry).
* User approves once; later, the agent executes within those limits.

**Value**: automation without open-ended permissions, fully auditable.

---

### 3. Operational Approvals

* Privileged actions (e.g., *“rotate API key”*) require just-in-time approval.
* Grant lifecycle gives a clean audit trail per action.

**Value**: makes least-privilege enforceable and reportable.

---

## Playground Components

* **Authorization Server (AS)**: GNAP grant lifecycle & token issuance.
* **Demo Web UI**:

    * Craft grant requests (JSON).
    * View grant state diagram in real time.
    * Simulate Device Code UX for approval.
    * Call `/continue` to observe final token issuance.
* **SSE Event Stream**: Live updates to visualize lifecycle changes.

---

## Key Endpoints

* `POST /grant` — Start a new grant request.
* `GET /device` — User enters the code.
* `POST /device/verify` — Verify code and show consent.
* `POST /device/consent` — Approve or deny.
* `POST /continue/{grantId}` — Poll status; retrieve token if approved.
* `GET /events` — Server-Sent Events for UI updates.

*(Follows [RFC 9635](https://www.rfc-editor.org/rfc/rfc9635.html) semantics: pending returns `continue`; tokens only after approval.)*

---

Here’s a rewritten version of your documentation with the **image embedded for clarity** and the step-by-step instructions tightly connected to the visual.

---

## Running the Demo

From the repo root, start the playground server:

```bash
go run ./cmd/playground
# Authorization Server available at http://localhost:8089
```

Then open the playground in your browser:
[http://localhost:8089/playground](http://localhost:8089/playground)

---

## Using the Playground (Step-by-Step)

The playground demonstrates a complete GNAP device flow. Use the diagram below as a visual reference. Each lettered label is explained in the legend.

![Playground diagram](./playground-ui.png)

---

### 1. Create a Grant

* Submit a JSON request (for example, a “payment” request with constraints).
* Lifecycle: `create → processing → pending`.
* You’ll receive a **user code** and a **continuation token**.
* **Diagram references:**

    * **A** – Create grant request
    * **B** – JSON payload details
    * **C** – “POST /grant” button

---

### 2. Verify (Human Step)

* Enter the issued code at `/device`.
* The server marks the grant as verified and shows consent details: type, resource, actions, constraints, locations.
* **Diagram reference:**

    * **D** – Input code + “Simulate Verify” button

---

### 3. Approve or Deny

* Approve → state becomes `approved`.
* Deny → state becomes `denied`.
* No action before TTL → state becomes `expired`.
* **Diagram reference:**

    * **E** – Approve/Deny controls

---

### 4. Continue (Client Polls)

* The client calls `/continue`.
* If pending → returns a continuation object.
* If approved → returns a usable access token (for example, JWT).
* **Diagram reference:**

    * **F** – “Call /continue” and polling actions

---

## Playground Legend

This diagram shows how UI elements map to the GNAP grant lifecycle:

* **A. Create Grant Request** – Initial request from client, includes proof key and requested access.
* **B. Grant Request JSON** – The request body sent to the authorization server.
* **C. POST /grant Button** – Submits the request, moving lifecycle to `processing`.
* **D. Input Device Code (Simulated Verify)** – Mimics user entering issued code to verify.
* **E. Approve / Deny Controls** – User’s choice, which moves the grant to `approved`, `denied`, or `expired`.
* **F. Continue Call (/continue)** – Client fetches grant status or tokens.
* **G. Event Log** – Displays state changes (codes issued, grants created, approvals/denials).
* **H. Lifecycle Diagram** – State machine overview:

    * `create → processing → pending`
    * From pending → `approved`, `denied`, or `expired`
    * All end in `finalized`, where tokens may be issued if approved

---

## Why This Matters

* **Fraud reduction**: Constraints + short TTLs reduce chargebacks.
* **Smooth customer flows**: Device Code UX keeps security in flow.
* **Agent safety**: Agents only get purpose-limited rights.
* **Compliance**: Every step is stateful, logged, and auditable.
* **Future-proof**: Designed for modern clients and agents, unlike legacy OAuth.

---

## Security Notes

**Production hardening**:

* Rate limits on `/grant` and `/device/*`.
* CSRF protection for consent.
* Structured logging + metrics.
* Real signing keys and rotation (JWKS).
