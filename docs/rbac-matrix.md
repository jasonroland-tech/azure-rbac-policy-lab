# Lakeview Health RBAC matrix

Who gets what, where, and how. Standing access is the exception; elevation goes through PIM.

| Persona | Scope | Role | Assignment type | Approval / limits | Reviewed |
| --- | --- | --- | --- | --- | --- |
| Junior Developer (Jordan Lee) | `rg-lakeview-prod` | Reader | Active, permanent | None needed (read only) | Quarterly access review |
| Junior Developer (Jordan Lee) | `rg-lakeview-dev` | Contributor | **Eligible** (PIM) | Justification, MFA, 1 hour max | PIM audit monthly |
| Operations on call | Subscription | Lakeview VM Operator (custom) | Eligible (PIM) | Justification, 1 hour | Quarterly |
| IAM admin | Subscription | User Access Administrator | Eligible (PIM) | Approval by second admin | Quarterly |
| Break glass (bg-admin01/02) | Tenant | Global Administrator | Active, permanent | Excluded from CA, alerts on sign in | Monthly sign in test |

## Guardrails that apply to everyone

| Control | Scope | Effect |
| --- | --- | --- |
| Allowed VM size SKUs (B1s, B2s) | `rg-lakeview-dev` | Deny anything larger, e.g. Standard_D2as_v5 |
| CA001 require MFA | All users except break glass | MFA on every sign in |
| Access review on `Lakeview-Developers` | Group | Unreviewed members removed automatically |
