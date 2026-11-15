# Lab guide: Azure RBAC, custom role and Policy

**Time:** 35 to 40 min. **Cost:** free, no VMs are created.
**Skills:** least privilege role assignments, custom roles, Azure Policy guardrails, access verification.

Uses your Lakeview tenant and Azure subscription. 📸 = screenshot. Redact the subscription ID and tenant domain.

## Before you start

**Accounts**
- [ ] **Your Lakeview Health Entra tenant and Azure subscription** (the ones running DC01). Sign in as your Global Admin.
- [ ] **Authenticator app** on your phone: Jordan Lee will register in Microsoft Authenticator because CA001 requires MFA. You can add Jordan as a second account in the same app.

**Software:** none. The portal and **Cloud Shell** run in the browser.
*First time in Cloud Shell:* choose **Bash**, then **No storage account required**, pick your subscription, and Apply.

## Least privilege (15 min)

**1. Test user and resource groups.** Entra admin > Users > New user **Jordan Lee**, job title **Junior Developer**. Azure portal > Resource groups > create `rg-lakeview-dev` and `rg-lakeview-prod`.
*Note:* CA001 makes Jordan register Microsoft Authenticator at first sign in. That's expected.

**2. Reader on prod only.** `rg-lakeview-prod` > Access control (IAM) > Add role assignment > **Reader** > Jordan.
*Why:* give the smallest role at the smallest scope that works.

**3. Prove it.** Access control (IAM) > **Check access** > Jordan. 📸 Then sign in as Jordan (private window) and try to create a storage account in `rg-lakeview-prod`: authorization fails. 📸

## Custom role (10 min)

**4. Create "Lakeview VM Operator".** Cloud Shell (Bash) > upload [`roles/lakeview-vm-operator.json`](roles/lakeview-vm-operator.json):
```bash
SUB=$(az account show --query id -o tsv)
sed -i "s/<subscription-id>/$SUB/" lakeview-vm-operator.json
az role definition create --role-definition @lakeview-vm-operator.json
```
*Why:* built in roles are often too broad. This one can start, stop and restart VMs but never create or delete them. 📸 the role under Access control > Roles.

## Policy guardrail (10 min, plus a wait)

**5. Allowed VM sizes.** Policy > Assignments > Assign policy > scope `rg-lakeview-dev` > **Allowed virtual machine size SKUs** > allow `Standard_B1s`, `Standard_B2s` > Create.

**6. Test the deny.** Wait about 10 minutes. Virtual machines > Create > `rg-lakeview-dev` > size **Standard_D2as_v5** > Review + create. Validation fails and names the policy. 📸 Cancel, don't create it.
*Why:* RBAC decides who can act. Policy decides what anyone is allowed to build.

**7. Write it up.** Update [`docs/rbac-matrix.md`](docs/rbac-matrix.md) and the README results. Keep Jordan for the PIM lab.
