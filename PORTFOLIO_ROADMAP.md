# StockFlow portfolio roadmap

Baseline reviewed on November 17, 2026. Repository roadmap published on November 18, 2026.

This roadmap applies to this repository and follows the shared Null Catchers portfolio plan. Verify the documented state against the current code and runs before starting work.

**Estimated initial demo effort** 70–120 hours after approval of the scope for a developer familiar with the project. The estimate excludes external reviews, store accounts, target environment setup, and later milestones.

## Project priorities

**Documented state** The repository has a README and portfolio plan. The proposed product scope requires approval before implementation.

**P0 — Demo** Review the requirements below and approve the scope and stack before building the first inventory movement flow.

**P1 — Validation and depth** Use an append-only stock ledger and reversing movements for correction. Derive balances from the ledger or a reconciled projection. Use transactional concurrency and idempotency for issues. Build balance and movement screens, CSV export, and synthetic data.

**P2 — Later** Consider barcode, multiple stores, or purchase orders after validating the ledger. Accounting, invoicing, and taxes are outside the proposed MVP.

**Acceptance criteria** Opening balance plus receipts minus issues reconciles. Retry produces one movement, concurrency preserves the balance policy, and rollback leaves no partial entry.

**Demo scenario** Receive 10 units → issue 7 → attempt concurrent issues → see the low-stock alert → count and correct inventory. [README](README.md).

## Next milestones

### Draft product requirements

A stock clerk at a single store needs to see how much is on hand and why it changed. Start with one warehouse and whole units per item. Defer inter-warehouse transfers.

| Proposed decision | Acceptance criteria |
|---|---|
| A clerk receives and issues stock. A supervisor corrects stock. | Corrections record who, when, and why. A clerk cannot submit one. |
| Use a stock ledger and reversing movements. | Keep the original movement, and show the reversal in the movement list. |
| Reject issues that would make a balance negative. | With a balance of 3 and two concurrent issues of 2, one succeeds and the balance is 1. |
| Require an idempotency key for submissions. | A replay adds no movement. Reusing the key with a different quantity is rejected. |
| Support low-stock thresholds and item history. | Receive 10 and issue 7 to reach 3 and trigger a threshold of 3. The balance matches the movement totals. |

Use a web application and PostgreSQL, with one transaction to record a movement and protect the balance. Choose the application framework after reviewing team experience. The first slice covers item setup, receiving, issuing, and current balance. Defer external alerts and barcode integration until the synthetic walkthrough passes.

## Portfolio acceptance checklist

- [ ] Another person can reproduce the primary workflow from a clean checkout using the README and test credentials or fixtures.
- [ ] The demo tells one complete story with a defined user, problem, and saved result.
- [ ] Tests cover the main flow, an important failure, and access boundaries when multiple accounts are involved.
- [ ] Demo data is labeled synthetic and reproducible. Review screenshots and recordings for secrets and customer data.
- [ ] Include four to six real screenshots where appropriate, a 60–120 second recording, and working links. Identify the revision used for capture.
- [ ] The README describes implemented capabilities, setup, architecture, limits, tests, and the demo.
- [ ] A short case study explains two engineering decisions, a considered alternative, and how the system handles an actual failure.
- [ ] Release evidence records the commit SHA, CI, prerequisites, and limits. Performance data includes workload, hardware, and method.
- [ ] Licensing and contributor attribution are settled before distribution. Choose the license for each repository individually.

A hosted demo is optional. Code execution, server administration, and secret management can be demonstrated through a recording and a reproducible isolated environment. Keep public demo access scoped to the activities it needs.

## Implementation and follow-up

- Break P0 into tasks with expected behavior, acceptance criteria, and a suitable test.
- Verify existing functionality first. Update this plan when evidence tied to a commit or PR closes a milestone.
- Record the fixture source, the revision used for evidence, and the tested limits.
- Review the final diff and verify required checks for its exact SHA before merging. Follow branch protection.
- For documentation-only changes with no applicable workflow, verify content and links and record the checks actually performed.
- Tie performance, security, and readiness claims to supporting evidence.
