# Azure RBAC, Custom Role and Policy Lab

> **Loom walkthrough:** _link coming soon_

Lab 6 of the Lakeview Health series. I controlled who can do what in Azure, and what anyone is allowed to build: least privilege role assignments, a custom role, and a policy guardrail that blocks oversized VMs.

## What this proves

| Skill | Evidence |
| --- | --- |
| Least privilege RBAC | Reader scoped to one resource group, verified with Check access and a failed write |
| Custom roles | [`roles/lakeview-vm-operator.json`](roles/lakeview-vm-operator.json): operate VMs, never create or delete |
| Policy guardrails | Allowed VM sizes denied a Standard_D2as_v5 build ([`policy/`](policy)) |
| Documentation | [RBAC matrix](docs/rbac-matrix.md) |

## Results

| Test | Expected | Result |
| --- | --- | --- |
| Jordan writes to prod | Authorization failed | _pending_ |
| Custom role created | Appears in subscription roles | _pending_ |
| D2as_v5 VM in dev | Denied by policy | _pending_ |

## Lessons learned

_Fill in after the lab._

Next: [PIM and access reviews](https://github.com/jasonroland-tech/entra-pim-access-reviews-lab). Run it: [LAB-GUIDE.md](LAB-GUIDE.md).
