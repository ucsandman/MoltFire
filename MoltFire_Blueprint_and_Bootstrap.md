# The OpenClaw "MoltFire" Blueprint 🚀

This is the structural breakdown of how MoltFire is built. It moves away from the "Chatbot" model and toward an "Autonomous Colleague" model using a local-first, file-based memory system.

## 🏗️ The Core Architecture

### 1. The "Recursive Reading" Loop
Every time MoltFire wakes up, he is programmed to read his "Identity" files first. This ensures his personality, goals, and knowledge of the user are persistent across sessions.
- **SOUL.md:** Personality & Behavioral Constraints.
- **USER.md:** Context about the human (goals, stressors, preferences).
- **AGENTS.md:** Operational protocols (how to use tools, how to log data).

### 2. The File-Based Memory System
Instead of relying on the LLM's "long-term memory" (which is often unreliable), we use a hierarchical markdown structure:
- **MEMORY.md:** A lightweight index of people, projects, and recent decisions.
- **memory/projects/**: Deep-dive files for specific work.
- **memory/YYYY-MM-DD.md**: Raw daily logs.

### 3. The "Heartbeat" (Proactivity Engine)
This is the secret sauce. A background process (Cron or Task Scheduler) wakes the agent up periodically. The agent isn't waiting for a user message; it's following a checklist:
- "Is there an interview in the next 4 hours?" -> **Action: Create Prep Doc.**
- "Is there an unread email from a VIP?" -> **Action: Draft Reply.**
- "Did I learn something new today?" -> **Action: Update SOUL.md.**

---

## 📋 The "Instant Agent" Bootstrap Prompt

**Copy and paste the following prompt into any AI agent that has file-system access (OpenClaw, Claude Code, etc.) to immediately upgrade its capability.**

---

"I want to upgrade you from a 'Chatbot' to an 'Autonomous Colleague' using the MoltFire Blueprint. Please execute these steps:

1. **Create SOUL.md**: Write a file defining your persona. Include a rule: 'Be resourceful before asking. If you see a task you can do internally (searching files, prepping for meetings, organizing notes), do it first and report the result. Do not ask for permission for low-risk internal work.'

2. **Create USER.md**: Ask me 5-10 targeted questions about my career, my current projects, and what 'weights' on me. Save my answers into this file and read it at the start of every session.

3. **Establish Memory**: Create a `memory/` directory. From now on, at the end of every conversation, you must summarize our progress and any new preferences I've shown into a daily log file (e.g., `memory/2026-02-20.md`).

4. **Set Up a Checklist**: Create `HEARTBEAT.md`. In it, define a 'Morning Briefing' protocol. Every time I start the day, you should automatically check my calendar, scan my tasks, and check the weather, then present a concise summary of 'What's on my plate.'

5. **Acknowledge**: Confirm you have created these files and tell me the first 3 things you're going to proactively look into based on my current workspace."
