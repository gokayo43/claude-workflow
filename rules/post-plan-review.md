## Code Review

### Automatic trigger
After completing any plan-mode implementation, no exceptions based on change size. Runs the full workflow (review + fix).

### Manual trigger
When the user asks to review changes, review a diff, or invokes /deep-review. Runs review-only (steps 1-4 below). Does NOT auto-fix — reports findings and lets the user decide.

### Full workflow (automatic trigger)

1. Run /simplify on the changed code
2. Implement any findings from the simplify results
3. Launch all 3 review agents in parallel (single message, all with run_in_background: true):
   - Ripple Tracer
   - Correctness Auditor
   - Architecture & Data Flow Auditor
4. Wait for all 3 to complete
5. Synthesize findings from all agents — deduplicate, verify each finding against actual code before acting
6. Implement confirmed fixes
7. Resume all 3 agents on the post-fix diff to verify fixes are clean and didn't introduce cross-lens regressions (resumed agents retain full context from step 3, making this efficient)
8. Report summary to user: what was found, what was fixed, what was debatable

### Review-only workflow (manual trigger)

1. Launch all 3 review agents in parallel (single message, all with run_in_background: true):
   - Ripple Tracer
   - Correctness Auditor
   - Architecture & Data Flow Auditor
2. Wait for all 3 to complete
3. Synthesize findings — deduplicate, verify each finding against actual code
4. Report findings to user organized by severity and agent lens
