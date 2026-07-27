# AWS Cloud Security Lab

An interactive, browser-based simulator for learning how AWS IAM authorization actually works — no AWS account required, nothing ever touches real infrastructure.

Paste in a real IAM policy, get an instant risk breakdown, watch how a small misconfiguration chains into a full account takeover, then see how Zero Trust conditions stop that same attack cold.

**Live demo:** https://manieshneupane.com.np/blog/AWS-Security-Lab/

---

## Why This Exists

Most cloud security "bugs" aren't code bugs at all — they're permissions that were scoped too loosely. Reading AWS docs about IAM evaluation order or `iam:PassRole` escalation is one thing; watching a policy get evaluated, watching the attack chain execute step by step, and watching a Zero Trust condition block it is what actually makes the mental model click.

This lab was built to close that gap for anyone moving from application security into cloud/AWS security — engineers, security researchers, and students alike.

## Features

### 1. IAM Policy Simulator
- Paste any JSON IAM policy and test it against specific API actions
- Mirrors AWS's real evaluation order: implicit deny → explicit deny → explicit allow
- Flags high-risk permission patterns automatically, including:
  - `iam:PassRole` combined with compute-launch permissions
  - `iam:CreateAccessKey` / `iam:CreateLoginProfile` on unscoped resources
  - `iam:AttachUserPolicy` / `iam:PutUserPolicy` self-escalation paths
  - `iam:UpdateAssumeRolePolicy` trust-policy tampering
- Real-time risk scoring: Low / Medium / High / Critical

### 2. Attack Simulation
Step-by-step walkthroughs of real-world AWS attack chains, including:
- SSRF → instance metadata credential theft (IMDSv1 vs. IMDSv2)
- Public S3 bucket exploitation
- CloudTrail logging evasion
- Lambda code-injection persistence
- KMS key disablement (cloud ransomware)

### 3. Zero Trust Demo
Shows how context-aware IAM conditions — `aws:SourceIp`, `aws:PrincipalOrgID`, `aws:MultiFactorAuthPresent` — stop the attack chains from Module 2, even when credentials are already leaked.

### 4. Network Security
Covers Security Groups vs. Network ACLs, and how VPC Endpoints/PrivateLink keep traffic off the public internet entirely.

## How It Works

The entire evaluation engine runs client-side in the browser:

```
Policy JSON  →  Parse & validate  →  Evaluation engine  →  Risk diagnostics
                                    (Deny > Allow > Implicit Deny)
```

No API calls, no backend, no AWS credentials of any kind — it's a teaching simulator, not a live scanner.

## Tech Stack

- HTML / CSS / JavaScript (client-side only)
- No backend, no external API calls, no data leaves the browser

## Getting Started

Clone the repo and open `index.html` directly, or serve it locally:

```bash
git clone https://github.com/Maniesh-Neupane/AWS-Security-Lab.git
cd AWS-Security-Lab
# then just open index.html in a browser, or:
python3 -m http.server 8000
```

## Related Write-Up

A full technical deep dive covering the IAM evaluation logic, all five privilege-escalation vectors, and the real-world 2019 breach this lab is modeled after is available here:
[Deep Dive into the AWS Cloud Security Lab](https://manieshneupane.com.np/blog/)

## About

Built by **Maniesh Neupane** ([@pwn4arn](https://x.com/pwn4arn)) — Web Application Penetration Tester and Bug Bounty Researcher, transitioning into AWS Cloud Security.

- Portfolio: [manieshneupane.com.np](https://manieshneupane.com.np)
- LinkedIn: [Maniesh Neupane](https://np.linkedin.com/in/manieshneupane)
- Bugcrowd: ManieshNeupane18

## License

This project is provided for educational purposes. See [LICENSE](LICENSE) for details.
