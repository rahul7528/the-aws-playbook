# 00 - Start Here: Cloud Basics

> If you can order chai on Zomato, you already understand the cloud.

Read this once before touching IAM. 10 minutes now saves 10 hours later.

---

## What is "Cloud"?

**Cloud = someone else's computer that you rent by the minute.**

Instead of buying a ₹80,000 server for your shop, you rent AWS's computer in Mumbai for ₹5/hour. When chai rush ends, you stop paying.

Old way: Buy server → wait 2 weeks → install → pray it doesn't crash
Cloud way: Click → server ready in 60 seconds → pay only when customers come

### Chai shop version:
- Your laptop = your home kitchen (limited, if it breaks you're done)
- AWS = a giant cloud kitchen in Mumbai with 10,000 stoves. You rent 1 stove for 1 hour.

---

## Why do we need it?

1. **No upfront cost** — start with ₹0 (Free Tier)
2. **Scale instantly** — Diwali rush? Rent 100 stoves, not 1
3. **Pay as you go** — close shop at night, stop paying
4. **Global** — open chai shop in US without flying there (choose us-east-1 region)
5. **Managed** — AWS fixes the stove, you just make chai

## How does cloud actually work?

AWS built huge buildings (data centers) full of computers.

- **Region** = city (ap-south-1 = Mumbai, us-east-1 = Virginia)
- **Availability Zone (AZ)** = different buildings in same city, within 100km range. If one floods, other works.
- **Edge Location** = small delivery hubs for fast chai (CloudFront)

You click "Create EC2" → AWS finds a free computer in Mumbai AZ-a → gives you IP → you use it.

---

## Why AWS (and not Azure/GCP)?

1. **First and biggest** — Big market share, jobs, services
2. **Free Tier actually usable** — 750 hours EC2/month for 12 months
3. **Everything you need** — 200+ services, one bill
4. **Indian region** — Mumbai (ap-south-1) = low latency for India

Learn AWS first, others feel similar later.

---

## IaaS vs PaaS vs SaaS (with chai)

**IaaS - Infrastructure as a Service**
> You rent the empty kitchen. You bring stove, staff, recipe.
- AWS: EC2, VPC, S3
- You manage OS, app, scaling
- Most control, most work

**PaaS - Platform as a Service**
> You rent kitchen with stove and staff. You just bring chai recipe.
- AWS: Elastic Beanstalk, Lambda, RDS
- AWS manages servers, you manage code
- Less control, faster launch

**SaaS - Software as a Service**
> You just order chai. Someone else makes it.
- AWS: WorkMail, or Gmail, Zomato
- Zero management, just use

**Rule of thumb:** Start with IaaS to learn, move to PaaS to ship faster.

---

## What services to learn first? (the 80/20)

Don't learn 200 services. Learn these 12 in order:

**Week 1 - Foundation (must know)**
1. **IAM** → keys and staff badges
2. **VPC** → your private plot of land
3. **EC2** → rented computer
4. **S3** → unlimited hard drive
5. **CloudWatch + Budgets** → alarms and bill guard

**Week 2-3 - Make it real**
6. **Route 53** → domain name
7. **ALB** → traffic cop
8. **RDS** → managed database
9. **CloudFront** → fast delivery

**Week 4 - Modern**
10. **Lambda** → run code without server
11. **DynamoDB** → fast NoSQL
12. **IAM Roles + Secrets Manager** → secure apps

Everything else builds on these.

---

## Prerequisites checklist (do this today)

1. **Create AWS account** → use personal email, add debit card (₹2 verification)
2. **Enable MFA on Root** → IAM → Security credentials → MFA
3. **Create admin IAM user** → never use Root again (see 01-IAM)
4. **Set $5 budget alarm** → Billing → Budgets → Create budget
5. **Install tools** (optional for Day 1):
   - AWS CLI: `aws --version`
   - VS Code

**Cost safety:** Free Tier covers first 12 months for EC2 t2.micro, S3 5GB, Lambda 1M requests. Always check "Free Tier eligible" tag.

---

## Common confusions cleared

**"Is AWS a website?"** No, it's 200+ building blocks. You combine them.

**"Do I need coding?"** No for basics. Console clicks are enough for Week 1.

**"Will I get a huge bill?"** Not if you: use Free Tier, set budget alarm, delete after labs. We show cleanup in every guide.

**"Region matters?"** Yes. Always choose **ap-south-1 (Mumbai)** for India projects — lower latency, data stays in India.

---

## Your first 3 labs (after this)

1. Create IAM admin user with MFA
2. Launch EC2 in Mumbai, SSH in, stop it
3. Create S3 bucket, upload photo, make it public (then delete)

All in Free Tier. All take <15 mins each.

---

Next → [01-IAM](../01-IAM/) — let's make your staff badges before you touch any servers.
