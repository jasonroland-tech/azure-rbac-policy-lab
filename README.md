# Azure RBAC and Identity Governance Lab

> **Loom walkthrough:** _link coming soon_

Lab 6 of the Lakeview Health series. I locked down who can do what in Azure: least privilege role assignments, a custom role, a policy guardrail, just in time admin with Privileged Identity Management, and an access review that removes people automatically.

## What this proves

| Skill | Evidence |
| --- | --- |
| Least privilege RBAC | Reader scoped to one resource group, verified with Check access and a failed write |
| Custom roles | [`roles/lakeview-vm-operator.json`](roles/lakeview-vm-operator.json): operate VMs, never create or delete |
| Policy guardrails | Allowed VM sizes denied a Standard_D2as_v5 build ([`policy/`](policy)) |
| Just in time access | Contributor made eligible in PIM with MFA, justification and a 1 hour limit |
| Access reviews | Group review with auto apply; denied user removed |
| Audit evidence | Review results and PIM audit exported to [`evidence/`](evidence) |
| Documentation | [RBAC matrix](docs/rbac-matrix.md) |

## Results

| Test | Expected | Result |
| --- | --- | --- |
| Jordan writes to prod | Authorization failed | _pending_ |
| D2as_v5 VM in dev | Denied by policy | _pending_ |
| Jordan activates Contributor | Needs MFA and reason, expires in 1 hour | _pending_ |
| Access review deny | User removed from group automatically | _pending_ |

## Lessons learned

_Fill in after the lab._

## Run it

See [LAB-GUIDE.md](LAB-GUIDE.md).
