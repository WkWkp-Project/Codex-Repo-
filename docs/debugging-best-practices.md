# Debugging Best Practices

Use this checklist when investigating, fixing, and preventing defects. The goal is to make debugging repeatable, observable, and safe for production systems.

## 1. Reproduce Before Changing Code

- Capture the exact steps, inputs, environment, browser/runtime version, feature flags, and user role required to reproduce the issue.
- Reduce the scenario to the smallest failing case before editing implementation code.
- Record the expected behavior and actual behavior in the issue or pull request.
- If the bug is intermittent, collect timestamps, request IDs, correlation IDs, and affected build versions before assuming a root cause.

## 2. Preserve Evidence

- Save relevant logs, screenshots, stack traces, network traces, and payload samples before retrying or redeploying.
- Do not overwrite the failing state until the team has enough information to recreate it.
- Redact secrets, credentials, tokens, personal data, and customer-sensitive values before sharing debugging artifacts.

## 3. Use Structured, Actionable Logging

- Prefer structured logs with stable fields such as `request_id`, `user_id`, `operation`, `status`, `duration_ms`, and `error_code`.
- Log at the lowest useful level: `debug` for local investigation, `info` for meaningful state transitions, `warn` for recoverable problems, and `error` for failed operations requiring attention.
- Include context that helps diagnose the failure, but avoid logging secrets or full sensitive payloads.
- Keep log messages concise and searchable; avoid noisy logs that hide important signals.

## 4. Debug With the Right Tools

- Use an interactive debugger, breakpoints, watch expressions, and logpoints when they provide faster feedback than adding temporary print statements.
- For compiled or bundled frontend code, keep source maps available in the appropriate debugging environment so stack traces and breakpoints map back to original source files.
- Use profilers for performance defects instead of guessing from code inspection alone.
- Use network inspectors for API, caching, CORS, and payload-shape problems.

## 5. Isolate the Root Cause

- Change one variable at a time and document what each experiment proves or disproves.
- Verify assumptions with tests, logs, queries, or debugger state instead of relying on intuition.
- Distinguish symptoms from causes; fixing only the visible symptom often allows the defect to return.
- When multiple services are involved, trace the request across boundaries with a shared correlation ID.

## 6. Add Regression Protection

- Add or update an automated test that fails before the fix and passes after it.
- Prefer focused unit tests for pure logic defects and integration/end-to-end tests for cross-boundary behavior.
- Include edge cases found during debugging, especially null/empty values, boundary dates, invalid permissions, retry behavior, and timeout paths.
- Keep the regression test readable so future maintainers understand the bug it prevents.

## 7. Clean Up Debugging Artifacts

- Remove temporary console statements, ad hoc scripts, local-only configuration, and test data before merging.
- Keep useful instrumentation as intentional structured logging or metrics rather than one-off debug output.
- Confirm the final diff does not expose secrets, local paths, or customer data.

## 8. Document the Fix

- Summarize the root cause, user impact, and fix in the pull request.
- Link to the issue, incident, trace, or dashboard where appropriate.
- Note any follow-up work, monitoring checks, or operational actions needed after deployment.

## Pull Request Debugging Checklist

Before requesting review, confirm:

- [ ] The bug can be reproduced or is explained with enough evidence.
- [ ] The root cause is identified, not just the symptom.
- [ ] The fix is covered by an automated regression test when practical.
- [ ] Logs, metrics, or traces are sufficient to diagnose similar failures later.
- [ ] Temporary debug code and sensitive artifacts are removed.
- [ ] The PR description explains the debugging approach and verification steps.
