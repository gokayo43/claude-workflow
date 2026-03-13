Run a read-only deep review of uncommitted changes. Do NOT auto-fix anything — report findings only.

Launch all 3 review agents in parallel (single message, all with run_in_background: true):
- **Ripple Tracer**: What existing dependents still assume the old contract?
- **Correctness Auditor**: Can the changed code produce wrong behavior on realistic inputs and states?
- **Architecture & Data Flow Auditor**: Do end-to-end invariants still hold across boundaries?

Wait for all 3 to complete, then synthesize:
1. Deduplicate findings across agents (report duplicates only if root cause differs)
2. Verify each finding against actual code before reporting
3. Report findings organized by severity and agent lens
4. Do NOT implement fixes — present findings for the user to decide
