# Adjustment Workflow

Use this workflow only when a project was estimated at 30 minutes or longer and the user asks to adjust model or reasoning settings.

## Evaluate the baseline

Compare the original recommendation with observed evidence:

- acceptance criteria, tests, review findings, and concrete errors;
- rework count or repeated failure patterns;
- elapsed time versus the original range;
- API or surface usage data when available; otherwise a labeled estimate; and
- remaining complexity, failure cost, and independent workstreams.

## Choose one adjustment

- Quality, correctness, or rework issue: raise effort first; then move Luna to Terra or Terra to Sol only when capability is the likely constraint.
- Time or token issue with acceptable quality: lower effort first; then consider Sol to Terra or Terra to Luna.
- Independent schedule bottleneck: propose parallel workers, while stating that aggregate tokens and cost can rise.

## Report and apply

Report the old and proposed model/effort, evidence, reason, predicted quality/time/token/cost effect, uncertainty, and the boundary of application. Apply only after the user requests the adjustment, and only to the next work unit or a new pinned subagent supported by the current surface. Do not switch a running root agent. Do not create recurring monitoring without separate approval.
