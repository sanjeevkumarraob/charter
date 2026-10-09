# Review instructions

## Passes
Run three passes and tag each finding with its pass:
- Bugs: logic errors, broken edge cases, regressions
- Security: injection, auth gaps, secrets, PII in logs
- Compliance: the change matches spec.md, plan.md and repo conventions

## Traceability
Flag any change in the diff that cannot be traced to plan.md. Either the plan is
updated in the same PR, or the change is removed.

## What Important means here
Important = breaks behaviour, leaks data, or breaches a policy. Style and naming are nits.

## Cap the nits
At most five nits per review; summarise the rest as a count.

## Do not report
Generated files and anything CI already enforces.
