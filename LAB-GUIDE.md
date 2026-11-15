# Lab guide: Azure RBAC, Policy, PIM and access reviews

**Time:** about 2 hours (do Parts 1 to 3 one day, 4 to 5 another). **Cost:** free. No VMs are created.
**Why this lab:** least privilege on cloud resources, just in time admin and access reviews are what "cloud IAM" and "governance" mean in job posts. It also covers about half of SC-300 area 4.

Uses your Lakeview Health tenant and Azure subscription. 📸 = screenshot. Redact the subscription ID and tenant domain.

## Part 1: Least privilege RBAC (25 min)

**1. Create the test user and resource groups.** Entra admin > Users > New user: **Jordan Lee**, job title **Junior Developer**. Azure portal > Resource groups > create `rg-lakeview-dev` and `rg-lakeview-prod` (same region as DC01).
*Note:* CA001 requires MFA, so Jordan registers Microsoft Authenticator on first sign in. That's expected, and it proves the policy covers new hires.

**2. Give Jordan read only on prod.** `rg-lakeview-prod` > Access control (IAM) > Add role assignment > **Reader** > Jordan.
*Why:* scope the role to the smallest place that works. A resource group, not the whole subscription.

**3. Prove it.** Access control (IAM) > **Check access** > Jordan. 📸
Then sign in as Jordan in a private window and try to create a storage account in `rg-lakeview-prod`: it fails authorization. 📸

## Part 2: Custom role (15 min)

**4. Create "Lakeview VM Operator".** Open Cloud Shell (Bash), upload [`roles/lakeview-vm-operator.json`](roles/lakeview-vm-operator.json), then:
```bash
SUB=$(az account show --query id -o tsv)
sed -i "s/<subscription-id>/$SUB/" lakeview-vm-operator.json
az role definition create --role-definition @lakeview-vm-operator.json
```
*Why:* built in roles are often too broad. This one can start, stop and restart VMs but never create or delete them. 📸 the role in Access control > Roles.

## Part 3: Azure Policy guardrail (15 min)

**5. Allow only small VM sizes in dev.** Policy > Assignments > Assign policy > Scope `rg-lakeview-dev` > definition **Allowed virtual machine size SKUs** > Parameters: `Standard_B1s`, `Standard_B2s` > Review + create.

**6. Test the deny.** Wait 10 minutes for the policy to apply. Virtual machines > Create > resource group `rg-lakeview-dev` > size **Standard_D2as_v5** > Review + create. Validation fails and names the policy. 📸 Then cancel. Don't create the VM.
*Why:* RBAC controls who can act. Policy controls what they're allowed to build.

## Part 4: Just in time access with PIM (30 min)

**7. License.** Entra admin > Identity Governance > Privileged Identity Management. If it asks, start the free **Microsoft Entra ID P2** trial, then assign a P2 license to yourself and Jordan.

**8. Make Jordan eligible, not permanent.** PIM > Azure resources > `rg-lakeview-dev` > Assignments > Add assignments > role **Contributor** > Jordan > type **Eligible** > 30 days. 📸
Then Settings > Contributor > Edit: max activation **1 hour**, **require justification**, **require MFA on activation**.

**9. Activate as Jordan.** Private window > portal > PIM > My roles > Azure resources > Activate > justification "Deploy test app" > Activate. 📸 the active assignment.

**10. Check the audit trail.** As yourself: PIM > Azure resources > `rg-lakeview-dev` > Resource audit. 📸 the activation record.
*Why:* nobody holds standing admin rights, and every elevation has a reason, a time limit and a log.

## Part 5: Access review (20 min, plus a short wait)

**11. Create a review.** Make a group `Lakeview-Developers` with Jordan and one other test user. Identity Governance > Access reviews > New access review > Teams + Groups > `Lakeview-Developers` > reviewers: **Selected users** (you) > duration 1 day > Upon completion settings: **Auto apply results** on, **If reviewers don't respond: Remove access** > Create.

**12. Review and apply.** Go to myaccess.microsoft.com > Access reviews > approve Jordan with a reason, deny the other user. Back in the review: Stop > Apply. The denied user is removed from the group. 📸 the results page.

**13. Save evidence.** Download the review results and the PIM audit (redact names) into `evidence/`. Auditors ask for exactly this.

Fill in the README and [`docs/rbac-matrix.md`](docs/rbac-matrix.md). The P2 trial ends after 30 days, and nothing here bills.
