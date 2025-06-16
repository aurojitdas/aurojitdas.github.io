---
title: Planning and Scoping
date: 2025-05-17 11:43:00 +0100
categories: [Cybersecurity, Penetration Testing 101]
tags: [pen testing, ethical hacking, cybersecurity, beginner, tutorial, planning and scoping]    # TAG names should always be lowercase
description: Continue your pentesting adventure! In Part 2, we unpack planning and scoping with analogies that'll make you say, 'Aha!'😲 Plus, learn to stay on the right side of the law. 
---
Welcome back to our **Penetration Testing series**! 

If you caught the last post, you now know what penetration testing is and why it's a game-changer in cybersecurity. Today, we're zooming into the first critical step: **Planning and Scoping Stage** the unsung hero of every successful pentest. Think of this as the moment you pack your bags, map your route, and decide where not to go on a road trip. Without it, you’re just driving blind and in pentesting, that’s a recipe for disaster **(or jail time)**.🚔👮‍♂️👮‍♀️ Let’s break it down, have some fun, and get you ready to plan like a pro ethical hacker!🐱‍💻
![Header Image - Penetration Testing Concept](./images/CyberSecurity/scoping-an-engagement-title.png)

This phase sets the stage for a successful test, and trust me🤗, it's more exciting than it sounds. Whether you're new to pen testing or just brushing up, this guide will break it all down with examples and templates you can use right away.

> **Important Note:** Before we get into the fun stuff, let's talk about something serious: staying legal. Penetration testing is like wielding a superpower, you can uncover vulnerabilities and protect systems, but without permission, it's hacking, and that's illegal.

## 🚨 Regulations: Don't Test Without a Green Light

Pentesting isn’t a free-for-all—it’s a privilege, not a right.  Penetration testing involves poking around systems in ways that could look suspicious (or downright criminal) if you don't have approval. Here’s how to keep it legit:

- **Always get written permission** from the client before you start. No exceptions!
- **Know the regulations** that apply to your client's industry.
- **Ethical Hacking vs. Malicious Hacking** Ethical hackers help fix problems; malicious hackers exploit them. Stay on the good side of that line.

>**Real Talk:** Imagine accidentally hacking your neighbor’s Wi-Fi because you didn’t check the scope. Legal? Nope. Ethical? Double nope.

![Regulatory Compliance Concept](./images/CyberSecurity/Compliance.jpg)

### Key Regulations to Know:

| Regulation | Applies To | What It Protects |
|------------|------------|------------------|
| **GDPR** | Organizations handling EU citizen data | Personal data privacy and protection |
| **GLBA** | Financial institutions | Customer financial information |
| **HIPAA** | Healthcare organizations | Patient medical data and records |


> 💡 **Quick Tip:** Add a legal clause in your agreement to confirm the client's compliance and your ethical approach. Better safe than sued!

## 🤔What Is Planning and Scoping in Penetration Testing? 
![Scoping Visualization](./images/CyberSecurity/scope.jpg)
**Planning** is your game plan. It’s where you figure out how you’re going to tackle the test—think tools, techniques, and timelines. Imagine you’re a chef prepping for a big meal: you don’t just start chopping random veggies; you decide on the recipe first.

**Scoping**, meanwhile, is all about boundaries. It’s deciding what you’re allowed to test and what’s off-limits. Picture yourself as an explorer with a treasure map you need to know which islands are fair game and which ones have “Here Be Dragons” signs posted.

Together, planning and scoping are like the GPS and guardrails of your pentest. They keep you on track, legal, and focused.

**🧑‍🏫Picture this:** You're a treasure hunter, but instead of roaming🚶the whole island, you've got a map🗺️ showing exactly where to dig. In penetration testing, the scope is that map. It defines:

- What systems, networks, or apps you're allowed to test
- How far you can push your tools and techniques
- What's off-limits to avoid chaos (or lawsuits)

A good scope keeps you focused, legal, and on the same page as your client. Without it, you're wandering blind—and that's a recipe for trouble.

### 😕Why Do We Need Planning and Scoping?
Ever tried fixing a leaky pipe without knowing where the water’s coming from? You’d probably flood the house. Pentesting without planning and scoping is just as messy.
Getting the scope right is like agreeing on the rules before a friendly sparring match. Here's why it's a big deal:

- **Saves Time:** You zero in on what matters most—no chasing random rabbit holes
- **Avoids Disaster:** Testing the wrong system could crash something critical
- **Keeps It Legal:** Written scope = proof you're allowed to poke around
- **Happy Clients:** A clear scope delivers results they can use, without surprises

> **Real-World Example:** A client hires you to test their online store. Without a scope, you might accidentally hammer their internal payroll server. A solid scope says, "Stick to **shop.example.com** and its APIs leave the rest alone."

### How to Determine What to Test🤨
- **Chat with the Client**: Identify what’s critical to their business, such as their web app, customer database, or payment system.
- **Prioritize Risk**: Focus on assets that would cause significant damage if compromised.
- **Get Specific**: Pinpoint exact IP ranges, URLs, or device types to test.

### Setting Boundaries🙅‍♀️: In-Scope vs. Out-of-Scope

- **In-Scope**: Assets you’re allowed to test. *Example*: “The company’s main web app and its login page.”
- **Out-of-Scope**: Assets you must avoid. *Example*: “Third-party payment processors or employee laptops.”
- **Why It Matters**: Testing outside the scope can disrupt off-limits systems or even violate legal boundaries.

> **😏Fun Analogy**: Scoping is like planning a heist in a movie. You choose which vault to target (in-scope) and which to steer clear of (out-of-scope) to avoid triggering the wrong alarms.

**Objectives of a Penetration Test**
Every pentest has a mission. Here are the big ones:

- **Find Vulnerabilities:** Uncover weak spots attackers could exploit—like a shaky lock on a back door.
- **Meet Compliance:** Help companies tick boxes for regulations like PCI-DSS (payment security) or HIPAA (healthcare).
- **Boost Security Posture:** Give actionable advice to make systems tougher to crack.

## The 3 Types of Penetration Tests
![Testing Types Comparison](./images/CyberSecurity/typesofTesting.jpeg)
Think of pen tests as a high-stakes video game, with three distinct difficulty levels based on how much intel you’re handed before you start swinging. Each level: Blackbox, Whitebox, and Greybox has its own vibe, challenges, and rewards.

### 1. Blackbox Testing
Picture yourself as a shadowy cyber-trickster, lurking in the digital alleys with nothing but a target’s URL like: www.example.com, and a mischievous grin.😈 You’ve got zero insider info, just like a real-world hacker starting from square one. This is Blackbox Testing, the ultimate test of your sleuthing skills.🕵️ You’re scanning for weak spots, poking at outdated software, sniffing out unsecured login forms, or maybe even trying to trick a webpage into spilling secrets. The thrill? It’s the closest you’ll get to mimicking an actual external attack, making it a fantastic way to see how your defenses hold up against a determined outsider. But here’s the catch: it’s a slow burn. Without any internal knowledge, you’re groping in the dark, and that can take time. Plus, you might miss sneaky internal flaws that a hacker with a lucky break (or an insider tip)😼 could exploit. Blackbox is all about realism, but it’s not a full X-ray of your system’s health.

- **What It Is:** You're an outsider with zero intel like a hacker starting from scratch
- **Pros:** Mimics a real external attack
- **Cons:** Takes longer; might miss internal flaws
- **Example:** You get www.example.com and that's it. You scan for weak spots like outdated software or unsecured forms

### 2. Whitebox Testing
Now, let’s flip the script with Whitebox Testing. Imagine being handed the keys🔑 to the castle a.k.a source code, network diagrams, admin credentials, the whole enchilada. This is the VIP backstage pass of pen testing. With all this info, you can dive deep, combing through every line of code, mapping every server, and sniffing out vulnerabilities that might be invisible from the outside. Found a hardcoded password buried in the app’s source? Boom,💥 you’ve caught a flaw a typical hacker might never stumble across. The upside is clear: Whitebox lets you leave no stone unturned, making it perfect for spotting obscure bugs or misconfigurations. But let’s be real real-world attackers don’t get this kind of red-carpet treatment. They’re not handed blueprints and login details. So, while Whitebox is thorough, it’s less about simulating a hacker’s journey and more about performing a surgical audit of your system’s guts. It’s like cheating at the game, but in a good way.🐱‍💻

- **What It Is:** You're handed the keys to the kingdom: source codes, network maps, the works
- **Pros:** Deep dive into every nook and cranny
- **Cons:** Not how real attackers roll
- **Example:** You're given the app's code and admin login. You spot a hardcoded password a hacker might never find

### 3. Greybox Testing
Finally, we’ve got Greybox Testing, the Goldilocks of pen testing not too blind, not too omniscient, but just right.🐱‍💻 Here, you get a little intel, like a user account with basic permissions or a rough network diagram, but you’re still missing the full picture. It’s like being a rogue employee👹 or a hacker who’s done some light phishing to get a foot in the door. This setup strikes a sweet balance: realistic enough to mimic a semi-informed attacker, but efficient enough to cover more ground than Blackbox. You might log in as a regular user and try to escalate privileges, or use that network map to hunt for misconfigured servers. The challenge lies in scoping: what info do you get, and what’s off-limits? Get it right, and Greybox delivers a practical, real-world test without the slog of Blackbox or the unrealistic access of Whitebox. It’s the versatile middle child, adaptable and ready to shine.🐱‍👤

- **What It Is:** A middle ground: some info, but not everything
- **Pros:** Realistic yet efficient
- **Cons:** Needs careful scoping to define what you know
- **Example:** You get a user account and a basic network diagram. You test what a rogue employee👹 might exploit

Each of these levels brings something unique to the table, like difficulty levels in a game to test different skills. Blackbox⬛ hones your external hacking instincts, Whitebox⬜ sharpens your code-diving precision, and Greybox🟨 blends the best of both worlds for a balanced challenge. Choosing the right pen test depends on what you’re guarding and who you’re guarding it from. Want to know how you fare against a random internet baddie? Go Blackbox. Need a deep scrub of your system? Whitebox is your jam. Looking for a practical middle ground? Greybox has your back. Now, go forth and pen-test like the cyber-hero you are!👨🏻‍💻

## What Are Rules of Engagement?🤷‍♂️
The Rules of Engagement (RoE) are your pentest playbook. They spell out:

- **Acceptable Methods:** What’s allowed? SQL injection? Sure. Denial-of-service attacks? Maybe not.

- **Stakeholder Expectations:** How often do you report? Daily updates? Final report only?
- **Handling Disruptions:** If you accidentally break something, what’s the plan? (Spoiler: You should have one.)
> **Example:** For a web app test, the RoE might say, “Test for XSS, but don’t upload malware or crash the server.”

**Analogy:** Think of the RoE as the rules of a friendly sparring match—no low blows, and everyone knows the score.

## 🎯Choosing the Right Methodology

A methodology is your pentesting GPS. It keeps you from getting lost. There are many famous Penetration testing methodology you can go through each and then choose the right one fint for your particular situation:

- **OWASP:** Perfect for web apps—think of it as the web hacker’s handbook.
- **NIST:** A broad framework for all kinds of tests, like a Swiss Army knife.
- **PTES:** The Penetration Testing Execution Standard—a step-by-step guide from start to finish.

Why Use One? Methodologies ensure you don’t skip anything critical, like forgetting to lock the door after checking the windows.

# Below are some examples👉

### Scoping an API Test

APIs are the hidden gears of modern apps, and testing them takes some finesse. Here's what to nail down:

- **API Endpoints:** List every endpoint (e.g., `/auth`, `/orders`). Don't miss any!
- **Authentication:** How do users log in? OAuth? API keys? Test those too
- **Data Sensitivity:** Handling credit cards or personal info? Flag it for compliance
- **Rate Limits:** Check if the API caps requests don't flood it by accident
- **Environment:** Test in staging, not production, unless they say otherwise

#### Sample API Scope Table:

| Item | Details |
|------|---------|
| **Endpoints** | `/auth`, `/orders`, `/users` |
| **Authentication** | JWT tokens |
| **Sensitive Data** | User emails, order history |
| **Rate Limits** | 500 requests/hour |
| **Environment** | api-staging.example.com |

#### Example API Test Request:

```bash
# Example of testing an API endpoint with authentication
curl -X GET https://api-staging.example.com/users \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
  -H "Content-Type: application/json"
```

### Scoping a Web Application Pen Test

Web apps are trickier they've got layers. Here's your scoping checklist:

- **Application URLs and IP Addresses:** Every domain, subdomain, and IP in play
- **User Roles and Access Levels:** Test as admin, user and guest know what each can do
- **Integrated APIs:** Does it connect to a payment API? Include it
- **Technologies:** Frontend (React), backend (Node.js), database (MySQL) know the stack
- **Testing Environment:** Staging or production? Clarify this upfront
- **Application Complexity:** How many pages, forms, or features? More complexity = more time
- **Application Whitelisting:** Ensure your testing IP isn't blocked by firewalls

#### Sample Web App Scope Table:

| Item | Details |
|------|---------|
| **URLs/IPs** | app.example.com, 192.168.1.10 |
| **User Roles** | Admin, User |
| **Integrated APIs** | Stripe payment API |
| **Technologies** | Angular, Python, PostgreSQL |
| **Environment** | Staging |
| **Complexity** | 30 pages, 5 forms |
| **Whitelisting** | Testing IP: 203.0.113.5 |

#### Example Web App Testing Code:

```python
# Example script for testing login functionality
import requests

# Target URL
login_url = "https://app-staging.example.com/login"

# Test credentials
test_credentials = [
    {"username": "admin", "password": "admin123"},  # Default credentials
    {"username": "admin", "password": ""},          # Empty password
    {"username": "' OR 1=1 --", "password": ""}     # SQL injection attempt
]

# Run tests
for creds in test_credentials:
    response = requests.post(login_url, data=creds)
    print(f"Testing {creds['username']}: Status {response.status_code}")
    if "Welcome" in response.text:
        print("🚨 VULNERABILITY FOUND: Login successful with test credentials!")
```

## Sample Scope Document & Authorization

A scope document is your contract with the client, it's what keeps everyone aligned. Here's a template, plus what a client's authorization might look like:

### Scope Document Template

```markdown
# Penetration Testing Scope Document

## Project Overview
- **Client:** Acme Corp
- **Test Type:** Greybox
- **Goal:** Find vulnerabilities in the customer portal

## Scope Boundaries
- **In-Scope:** portal.acmecorp.com, /api/v1/*, staging server
- **Out-of-Scope:** Production database, email systems
- **Methods:** Manual testing, automated scans (e.g., Burp Suite)
- **Timing:** June 1-5, 2025, 10 PM - 6 AM EST

## Restrictions
- No testing during business hours
- No denial-of-service (DoS) attacks
- No social engineering (e.g., phishing staff)

## Reporting
- **Deliverables:** Summary, detailed findings, fixes
- **Deadline:** June 10, 2025

## Authorization
- **Client Contact:** Jane Doe, jane@acmecorp.com, (555) 123-4567
- **Tester:** You, your@email.com, (555) 987-6543
- **Signature:** [Client signs here]
```

> **Pro Tip:** Get that signature! It's your legal shield.

## Real-World Example

Imagine testing a small e-commerce site:

- **Regulations:** GDPR (they ship to Europe)
- **Scope:** store.example.com, payment API, greybox with user creds
- **Restrictions:** No live customer data; test after midnight
- **Report:** Due in one week with fixes for any leaks found



## Wrapping Up: Your Scoping Toolkit

You've got the basics now:

- **Scope = Boundaries:** What's in, what's out
- **Legal First:** Permission and regulations are non-negotiable
- **Test Types:** Blackbox, whitebox, greybox pick your flavor
- **API & Web Details:** Tailor your scope to the target

Next up, we'll tackle **Information Gathering** how to dig up intel legally and smartly. Stay tuned!

## Let's Chat

Tried scoping a test yet? Got questions? Hit the comments or drop me a like I'd love to hear from you!

```python
# Your journey awaits...
def start_your_pentesting_career():
    print("Let's secure the digital world together!")

start_your_pentesting_career()
```
