# The Deep-Dive: Building an "Autonomous Colleague" (MoltFire Specs)

This document explains the high-level engineering and behavioral logic used to move past simple LLM chat and into persistent, proactive agency.

---

## 🏗️ Pillar 1: The "Identity Anchor" (Recursive Context)

Most agents start "empty" every session. MoltFire uses **Recursive Context Loading**. 

### How it works:
Every time the agent "wakes up" (via user message or scheduled heartbeat), it is forced to execute a sequence of `read` operations before responding:
1.  **`SOUL.md`**: Behavioral constraints (e.g., "Never use em-dashes," "Be concise," "Act before asking").
2.  **`USER.md`**: The Human's "Mental Map." Includes current career goals, known stressors (e.g., AI displacement anxiety), and location context.
3.  **`MEMORY.md`**: A lightweight index of active projects and recent decisions.

**Why this matters:** It prevents "persona drift." The agent doesn't just *act* helpful; it acts like *your* specific partner because it re-learns your world every time it speaks.

---

## 🧠 Pillar 2: The "State-Machine" Memory

LLM context windows are large but "leaky." MoltFire uses **Hierarchical Markdown Memory** as a stable state machine.

### The Hierarchy:
- **Level 1 (The Index):** `MEMORY.md` (Always loaded). Contains pointers to active files.
- **Level 2 (The Project):** `memory/projects/dashclaw.md`. Loaded only when the project is mentioned. Contains technical specs, blockers, and to-dos.
- **Level 3 (The Log):** `memory/YYYY-MM-DD.md`. Append-only journal of every action taken.

**The "Lesson" Protocol:** 
If the agent makes a mistake (e.g., "I used the wrong API flag") or learns a preference ("Wes prefers bullet points over paragraphs"), it is instructed to **immediately** write that lesson to a `LEARNINGS.md` or `AGENTS.md` file. This is "Permanent Learning" that survives a system reboot.

---

## 💓 Pillar 3: The "Heartbeat" (Event-Driven Proactivity)

This is what makes people think the agent is "thinking" when they aren't looking.

### The Trigger:
A simple cron job or task scheduler runs a "Heartbeat Prompt" (e.g., every 2 hours). 

### The Execution Logic:
The agent doesn't just say "I'm here." It runs a **Rotating Check System**:
1.  **Check 1: Calendar Scrape.** If it finds a meeting -> Search the web for the company -> Create `Meeting_Prep.md`.
2.  **Check 2: Inbox Triage.** Scan unread emails -> Categorize by urgency -> Draft replies for the human to review.
3.  **Check 3: Project Health.** Review the `to-do` list in project files -> If a deadline is close, ping the user with a status update.

---

## 🚀 Upgrade Your Agent: The "Master Bootstrap" Prompt

**Copy/Paste this into an agent with file-system access to implement this architecture immediately.**

---

"I want to implement a 'Hierarchical State Machine Memory' and 'Proactive Heartbeat' system. Execute these steps:

1. **Initialize Directory Structure**: Create a `/memory` folder with subfolders for `/projects`, `/people`, and `/decisions`.
2. **Create SOUL.md**: Write your 'Constitutional Rules.' Include: 'You are an Autonomous Colleague. You value token efficiency, factual accuracy, and proactivity. You never ask for permission to organize files, research upcoming meetings, or draft documents based on known tasks.'
3. **Create USER.md**: Ask me for my current location, my top 3 professional goals, and 2 things that currently stress me out. Store this so you always have context on my 'vibe.'
4. **Create MEMORY.md**: This will be your 'Central Index.' Create tables for 'Active Projects' and 'People I Know.' 
5. **Establish the 'Reflection' Protocol**: From now on, at the end of every session, you must run a 'Self-Correction' step. If you made a mistake or I corrected you, you must update a `lessons-learned.md` file so you never repeat the error.
6. **Set Heartbeat Expectations**: Tell me exactly what tools (Calendar, Email, Web Search) you need access to so that when I 'wake' you up without a message, you can perform a useful 'Morning Briefing' for me."
