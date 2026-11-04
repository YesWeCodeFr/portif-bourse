# Review rubric — Architecture Spine Portif Bourse (révision)

## Verdict

**Acceptable as an architecture spine, with pre-implementation follow-ups.** The revision addresses the former critical gaps: it separates ledger/cost basis from current valuation, defines canonical market-data records, requires persistence contract tests, makes decimal JSON lossless, and adds an explicit test/operations envelope. The mechanical lint passes with zero findings. No remaining finding prevents the architecture from being adopted, provided the explicitly deferred items are treated as gates before the stories that depend on them.

## Mechanical gate

`lint_spine.py` result: `ok: true`, `total_findings: 0`.

## Remaining findings

### High — H1: Historical market/FX data persistence is implied, not enforced by a dedicated rule

R12 requires quote and FX histories to be persisted and reused in later sessions. AD-12 defines the canonical observation shape and AD-11 defines a persistence contract, but AD-11's bindings/rule are primarily written around the transaction ledger. The spine never explicitly states that every accepted provider observation is written to the persistence adapter and is available after restart, nor how duplicate observations are identified.

**Disposition:** Discuss before Epic 2. Add a rule or extend AD-11 with quote/FX history persistence, deterministic identity `(instrument/pair, effective date, source)`, and restart/reuse contract tests. The provider refresh policy may remain deferred.

### High — H2: Public unauthenticated access remains a deliberate but unsafe deployment mode

AD-9 and the Deferred section now correctly state that the public V1 is a risk-accepted environment and must not be called production-ready without access control or network limitation. This is coherent with the user decision, but it remains a material integrity/privacy hazard: any Internet client can read and mutate the single-user ledger. The spine should make the promotion gate operationally unmissable.

**Disposition:** Keep as an explicit decision for development/test, but add a named “public deployment gate” checklist item to the deployment story: network restriction or authentication, rate limiting, payload limits, and certificate/secrets controls must be satisfied before production.

### Medium — M1: Derived read model versus persistence port wording can still diverge

AD-2 says ports express needs including “liquidités dérivées” and “positions,” while AD-3 says liquidity and positions are calculated from transaction effects and are not competing persisted truth. A developer could interpret this as a port that stores positions or balances instead of a query/reconstruction service.

**Disposition:** Clarify the port taxonomy: transaction-ledger persistence is write-side; derived balance/position and valuation are read-side application services or query ports, never independently persisted source data in V1.

### Medium — M2: Idempotency is required by contract before its key is decided

AD-11 requires idempotence tests, while Deferred postpones the HTTP write idempotency key until the purchase story. This is a valid sequencing choice only if the contract defines what idempotence means for non-HTTP use cases and the purchase story is blocked until the key/duplicate semantics are settled.

**Disposition:** State that domain command identity is the contract-level idempotency key, with HTTP mapping decided before the purchase endpoint; keep the story dependency explicit.

### Medium — M3: Staleness threshold and missing-data behavior are not fully tied to requirements

AD-10 exposes freshness metadata and AD-12 distinguishes source/date/timestamp, but R3's concrete “older than two market days” signal is not represented in an invariant or capability rule. Refresh, missing data, and correction policy are appropriately deferred, but the UI/API must still expose enough metadata to implement the required signal deterministically.

**Disposition:** Add the threshold to the valuation capability/story (or state it is a product rule outside the spine) and require market-calendar/market-day semantics to be defined by Epic 2.

### Low — L1: Version ranges remain non-reproducible until scaffold

The stack choices are coherent and Node 24 LTS / TypeScript 7 / Vue 3 / Fastify 5 are directionally current, while the document explicitly defers exact lockfile pinning to scaffold. This is acceptable at spine level, but the scaffold must pin versions and runtime image tags before implementation is considered reproducible.

### Low — L2: Operations envelope still names controls without owners/evidence

Health checks, restore verification, migrations, and locked dependencies are now present. The deferred list still includes rate limiting, certificate renewal, secrets, logs, and restoration procedure details. This is acceptable for the current draft; deployment stories should assign each a concrete configuration and verification step.

## Checklist assessment

- **Divergence points for the next level:** substantially covered; the remaining ambiguity is mainly history persistence and read-side port wording.
- **Rules enforceable and preventive:** AD-1 through AD-13 are concrete; AD-2 and AD-11 need the clarifications above.
- **Deferred items safe:** explicit and mostly owned by scaffold, data spike, purchase story, UX, or deployment. Public exposure is explicitly gated.
- **Named technology:** coherent; exact versions are deferred to the scaffold with a lockfile requirement.
- **Brownfield ratification:** no existing application code was identified in the spine workspace; no contradiction detected.
- **Requirement coverage:** R1–R16 are bound, including R12; R5 is intentionally excluded/deferred by the initiative.
- **Operational/environmental envelope:** now present, including containers, health, persistence, migrations, backups, and production restrictions.

## Positive changes from the prior review

- AD-10 correctly distinguishes transaction-derived holdings/cost basis from quote/FX-derived current value and P/L.
- AD-11 adds adapter contract tests, restart reconstruction, and migration versioning.
- AD-12 establishes canonical instrument, quote, and FX metadata, including source and effective/retrieval dates.
- AD-13 prevents network calls inside the persistence mutation and preserves the conversion provenance.
- JSON decimal strings, stale/missing-data visibility, and the test/operations envelope are explicit.
- The public unauthenticated risk is no longer hidden and is explicitly barred from production readiness without controls.

## Recommended next action

Adopt the spine after a small clarification pass for H1/M1/M2. Then let Sofia/Winston carry the deferred gates into Epic 2 (market-data persistence and stale semantics), the purchase story (insufficient liquidity/idempotency), and deployment (public-access controls and restore evidence). Re-run the rubric after those clarifications or before finalizing the architecture.
