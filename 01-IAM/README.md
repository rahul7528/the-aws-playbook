# 01 - IAM (Identity and Access Management)

> IAM is the bouncer, the key-maker, and the rule-book for your AWS chai shop.

## The Chai Shop Analogy (read this first)

Imagine you own **"Rahul's Cloud Chai"** in Faridabad.

- **Root User** = You, the owner. You have the master key to the safe, the shop deed, and you can sell the whole shop. You use this key ONCE to open the shop, then you lock it in a locker.
- **IAM User** = Your staff. `priya-cashier`, `aman-chai-maker`, `rohit-delivery`.
- **IAM Group** = Shift teams. `morning-shift` gets cashier + chai-maker permissions together.
- **Policy** = The job card taped to their apron. "Can: use cash register, cannot: open safe".
- **Role** = A temporary visitor badge. The milk supplier wears it for 1 hour to deliver milk to the fridge — he doesn't become staff.
- **MFA** = Fingerprint + key. Even if someone steals Priya's key, they can't open the register without her fingerprint.

**Why not give everyone the Root key?** Because one angry employee can burn the shop, delete the safe, and you can't undo it. Root can close the AWS account permanently. That's why we never use Root after Day 1.

---

## Why we need IAM

AWS starts with one email (Root). Without IAM:
- Everyone shares one password = no audit trail
- One mistake = delete everything
- You can't give limited access to developers, auditors, or CI/CD

IAM solves: **Who can do What on Which resource, and When.**

## What problem it solves

1. **Least Privilege** — give only chai-making access, not safe access
2. **Auditing** — know *who* deleted the S3 bucket at 2am
3. **Rotation** — change staff without changing the master key
4. **Federation** — let your Google Workspace staff login without AWS passwords

## Cost

**Free.** IAM users, groups, roles, policies cost $0. You pay for what those identities *use* (EC2, S3), not for IAM itself.

---

## Core Building Blocks

### 1. User
A permanent identity for a person or app. Has console password and/or access keys.
- Example: `dev-rahul`

### 2. User Group
A collection of users. Attach policies to the group, not to each user.
- Example: `Developers` group gets `AmazonS3ReadOnlyAccess`

### 3. Role
Temporary credentials for AWS services or external users. No long-term password.
- Use for: EC2 → S3 access, Lambda → DynamoDB, GitHub Actions → AWS
- Trust Policy = "who can wear this badge"

### 4. Policy
JSON document that says Allow or Deny.
```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::my-chai-menu/*"
  }]
}
```
Types:
- **AWS Managed** — pre-made by AWS (`ReadOnlyAccess`)
- **Customer Managed** — you create and reuse
- **Inline** — stuck to one user/role only (avoid)

### 5. Identity Providers (IdP)
Let staff login with company Google/Okta/AzureAD. No AWS passwords needed. Uses SAML or OIDC.

### 6. MFA, Password Policy, Access Keys
- MFA: must-have for Root and all humans
- Password policy: 14 chars, rotate 90 days
- Access Keys: for CLI — treat like ATM PIN, rotate every 90 days

---

## Why NEVER use Root (except 3 tasks)

Use Root ONLY for:
1. Creating first admin user
2. Changing account settings / support plan
3. Closing account

Everything else — use IAM admin user with MFA.

If Root keys leak, attacker owns billing, can delete *everything*, and you can't see who did it.

---

## Step-by-Step: Create your first IAM Admin (Console)

1. Login as Root → IAM → Users → Create user: `admin-rahul`
2. Check "Console access" → Autogenerate password → Require reset
3. Add to group: Create group `Admins` → Attach policy `AdministratorAccess`
4. Enable MFA: User → Security credentials → Assign MFA device → Authenticator app
5. Logout Root, login as `admin-rahul`, never use Root again

## Step-by-Step: Create a Developer with Least Privilege (CLI)

```bash
# 1. Create user
aws iam create-user --user-name dev-priya

# 2. Create group and attach read-only S3
aws iam create-group --group-name Developers
aws iam attach-group-policy --group-name Developers   --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess

# 3. Add user to group
aws iam add-user-to-group --user-name dev-priya --group-name Developers

# 4. Create login profile (one-time password)
aws iam create-login-profile --user-name dev-priya   --password 'TempPass123!' --password-reset-required

# 5. Force MFA (via policy - attach to group)
```

## Create a Role for EC2 to read S3

```bash
# trust policy file
cat > trust.json <<EOF
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": {"Service": "ec2.amazonaws.com"},
    "Action": "sts:AssumeRole"
  }]
}
EOF

aws iam create-role --role-name EC2-S3-Reader --assume-role-policy-document file://trust.json
aws iam attach-role-policy --role-name EC2-S3-Reader   --policy-arn arn:aws:iam::aws:policy/AmazonS3ReadOnlyAccess
```

---

## Best Practices (do these on Day 1)

1. **Lock Root**: Enable MFA, don't create access keys, store password in password manager
2. **No long-term keys for humans**: Use IAM Identity Center (SSO) or federate
3. **Groups over users**: Never attach policies directly to users
4. **Least privilege**: Start with ReadOnly, add as needed
5. **Rotate**: Access keys 90 days, passwords 90 days
6. **Use Roles for everything**: EC2, Lambda, CodeBuild, GitHub Actions
7. **Enable Access Analyzer**: finds public or over-permissive policies
8. **Policy Simulator**: test before attaching

---

## Common Mistakes

- Giving `AdministratorAccess` to everyone ("it's faster")
- Creating access keys for Root
- Inline policies everywhere (can't audit)
- No MFA on console users
- Sharing one IAM user across team

---

## Interview Questions (with one-line answers)

1. **Difference between IAM User and Role?**
   User = long-term for person/app. Role = temporary credentials assumed by trusted entity.

2. **Why not use Root?**
   Root can't be restricted, no MFA enforcement, full billing access, no CloudTrail user attribution.

3. **Policy evaluation logic?**
   Explicit Deny > Explicit Allow > Implicit Deny.

4. **What is least privilege?**
   Give only permissions needed for the job — chai-maker can't open safe.

5. **Managed vs Inline policy?**
   Managed = reusable, versioned. Inline = embedded, hard to audit.

6. **How does IAM Role work for EC2?**
   Instance profile provides temporary credentials via metadata service.

7. **What is STS?**
   Security Token Service — issues temporary credentials for AssumeRole.

8. **How to give cross-account access?**
   Create Role in Account B with trust policy for Account A, then assume it.

9. **What is IAM Identity Center (SSO)?**
   Central place to manage human access across multiple AWS accounts with MFA.

10. **Difference between SCP and IAM policy?**
    SCP = guardrail at Org level (max permissions). IAM = actual permissions inside account.

11. **How to rotate access keys safely?**
    Create new key → update app → test → deactivate old → delete after 7 days.

12. **What is permission boundary?**
    Maximum permissions a user/role can ever have, even if admin policy attached.

---

## Cleanup

```bash
aws iam detach-user-policy --user-name dev-priya --policy-arn ...
aws iam delete-login-profile --user-name dev-priya
aws iam remove-user-from-group --user-name dev-priya --group-name Developers
aws iam delete-user --user-name dev-priya
```

## Next
→ `03-VPC/` — now that you have keys, let's build the private neighborhood for your chai shop.
