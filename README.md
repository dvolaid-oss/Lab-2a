# Lab 2a - Should we trust the AI?

AI-Enhanced Cybersecurity, Lecture 5.

## What is this lab about?

Companies have a security team (a SOC) that watches computer activity for attacks. More and more, an AI system helps them. It looks at the activity, decides what looks suspicious, and suggests what to do, for example "cut this server off the network".

The point of the lab is to **not just accept what the AI says**. You act as the security analyst. You check the evidence yourself and then make the final decision.

## How the system works

The course gave us a small practice system. All the activity in it is fake, made up by a program.

```
fake activity -> detector -> AI helpers -> dashboard -> me (the human decides)
```

| Part | What it does, in simple words |
|---|---|
| Detector | Gives every event a risk score from 0 to 100 |
| XAI (Explainable AI) | Explains *why* the AI thinks something is suspicious |
| RAI (Responsible AI) | Checks the rules: what is allowed, and what needs a human to approve |
| Meta-AI | Checks the AI's own work: is it sure, is anything missing? |
| Human (me) | Makes the final decision and writes down why |

All messages between the parts go through a message system called Kafka (Redpanda), and every decision is saved in a log file (the audit trail).

## What I did

1. Installed and started the system. The tests passed (5 out of 5).
2. Let it run for about a minute. It created **63 alerts**.
3. Picked two cases and checked each one step by step.
4. Recorded my decision for each one in the dashboard.
5. Checked that my decisions were really saved.

## What I decided

**Case 1, alert-0-17 (risk 100). My decision: escalate (pass it up to a manager)**
Someone tried to log in to the backup account 23 times from an IP in Iran, and 48 MB of data left the database server. That looks like a real attack. But all the evidence comes from **one single log line**, and cutting off a database server is a big step. A decision like that should not be made by one analyst alone.

**Case 2, alert-0-0 (risk 80). My decision: reject (false alarm)**
A completely normal file access from inside the company. The AI flagged it only because of the time of day.

## The most interesting thing I found

**Every single event became an alert, 63 out of 63, even the normal ones.**

The reason: the system learned what "normal" looks like only at 6 in the morning. I ran it at 7, so to the AI everyone looked suspicious.

The Meta-AI, whose job is to catch exactly this kind of mistake, approved all 63 cases and found no problem.

**Lesson:** an AI can sound confident and follow all the rules and still be wrong. That's why a human has to check the evidence.

## Files in this repo

| File | What's inside |
|---|---|
| [Lab2a_Worksheet.md](Lab2a_Worksheet.md) | The full worksheet: every section and question from the lab guide, answered |
| `screenshots/` | Pictures from the dashboard |
| `evidence/` | The raw data I exported from the system, as proof |
| [docs/README_HE.md](docs/README_HE.md) | An explanation of this repo in Hebrew |
