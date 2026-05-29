# AWS Fieldbook — Learn AWS Like a Story, Not a Manual

> A beginner-friendly, DevOps-focused field guide to Amazon Web Services. Built for the curious non-techie, the new cloud engineer, and anyone tired of dense docs.

If you've ever opened the AWS console and thought "where do I even start?" — this repo is for you.

---

## 🎯 What we're building

A simple space where **each AWS service gets its own folder** with:
- **Why we use it** — in plain English, with real-life analogies
- **How we use it** — click-by-click console steps, CLI commands, and Terraform
- **What it connects to** — the services it works with in real production setups
- **How much it costs** — with Free Tier tips and Indian pricing examples
- **Common mistakes & interview questions** — so you learn by doing, not memorizing

No jargon walls. No assumptions. Just "what is this thing, why does it matter, and how do I try it safely?"

Inspired by the [linux-command-fieldbook](https://github.com/rahul7528/linux-command-fieldbook), but for cloud.

---

## 👤 Who is this for?

- **Absolute beginners** — you’ve heard of "the cloud" but never logged in
- **Students & career switchers** — preparing for AWS Cloud Practitioner / DevOps roles
- **Developers moving to DevOps** — need to understand infra, not just code
- **Non-tech teammates** — PMs, designers, founders who want to speak AWS

If you can use a smartphone, you can start here.

---

## 🗺️ How to use this repo

1. **Start with the story, not the service.** Read `/00-Start-Here`
2. **Pick a service folder** — e.g. `11-EC2`, `19-S3`, `01-IAM`
3. Inside each folder you'll find:
   ```
   README.md       → the story (why, how, connects)
   /images/        → simple diagrams
   /commands/      → copy-paste CLI & Terraform
   ```
4. Try it in your **AWS Free Tier** account (we mark what's free)

---

## 📚 What you'll learn

By the end, you’ll understand how real companies build on AWS:

**Foundation:** IAM → VPC → EC2 → S3  
**Web App:** Route53 → CloudFront → WAF → ALB → ECS/Fargate → RDS  
**Serverless:** API Gateway → Lambda → DynamoDB → S3  
**Observability:** CloudWatch → CloudTrail → X-Ray  
**CI/CD:** CodeCommit → CodeBuild → CodeDeploy → CodePipeline

Each service page answers 5 questions:
1. What is it? (analogy first)
2. Why do we use it?
3. How do we set it up?
4. What does it cost?
5. What plugs into it?

---

## 📂 Repo Structure

```
aws-fieldbook/
├── 00-Start-Here/
├── 01-IAM/
├── 02-Organizations/
├── 03-VPC/
├── ...
├── 50-Cost-Budgets/
```

Folders are numbered in learning order. You don’t need to go in order — jump to what you need.

> Full list of 50+ production services is in the complete starter kit.

---

## 🧭 Beginner Learning Paths

**Path 1 — "I know nothing" (2 weeks)**
Day 1-3: IAM, S3, EC2
Day 4-7: VPC, Route53, CloudWatch
Day 8-14: Build a static website on S3 + CloudFront

**Path 2 — "I want a DevOps job" (4 weeks)**
Add: ECR, ECS/EKS, CodePipeline, RDS, Secrets Manager, Systems Manager

---

## 🤝 Contribute

This is a community fieldbook. Found a simpler analogy? A cheaper way? A mistake?
1. Fork the repo
2. Edit the service README
3. Add a diagram to `/images`
4. Open a PR with "beginner-friendly" in the title

Guidelines: Use simple language, add one real-world example, keep commands tested.

---

## ⚠️ Cost Safety

- We always note Free Tier limits
- Use `aws budgets` alerts from Day 1
- Destroy what you create: every guide ends with cleanup steps

---

## 📄 License

MIT — learn, fork, teach, share.

---

**Made with curiosity in Faridabad, India.**  
Star ⭐ the repo if it helps you, and share it with one friend who’s scared of AWS.
