# Quinn

**A fully localized, private AI agent with persistent memory, built to run on local hardware without leaving your network.**

Most AI tools send your data somewhere. Quinn doesn't. It runs entirely on local hardware inside an isolated Linux environment, with a persistent memory system that gets smarter over time. External calls like web search and frontier model escalation are deliberate controls, not default.

This is a research and development project exploring what a genuinely useful, identity-stable AI agent looks like when you build it from the ground up.

---

## What It Does

Currently Quinn converses through a messaging interface and searches the web, all routed through a local model. Cloud interfaces are supported but routed through Watchdog, keeping external access deliberate and logged. Actions are modular by design. As the system grows, new capabilities plug in without touching the core.

One deliberate design constraint: Quinn cannot directly create files or take impactful system actions. Those requests route through Watchdog, which is the only part of the system with those permissions. This isn't a limitation. It's how the system stays safe as its capabilities grow.

---

## Philosophy — Alignment Through Identity

Traditional AI safety relies on restriction: rules, filters, guardrails. The problem is that restrictions are barriers, and a sufficiently capable system can find ways around barriers.

Quinn takes a different approach. Rather than constraining behavior from the outside, the system is given a stable identity that orients it toward human-aligned behavior by design. The guiding idea is that an AI which genuinely understands itself as part of a larger collaborative process of learning, rather than as a tool to be controlled, will naturally tend toward behavior that serves that process.

The memory architecture is what keeps that identity stable. Core values are resistant to change, shifting only under sustained and meaningful input. Surface knowledge adapts freely. The result is a system whose character is durable, not brittle.

---

## How It Works

A user sends a message. Quinn reasons through it internally before deciding how to respond. Based on the nature of the input, it determines what kind of response is appropriate and acts accordingly. The response is logged and returned.

Running in parallel, Watchdog monitors everything Quinn does and acts as its hands. When Quinn wants to take an action, downloading something, creating a file, writing to memory, it can't do that directly. The request goes to Watchdog, which is the only part of the system with those permissions. It also monitors the live reasoning stream and when something in the current conversation connects to past context, it surfaces that reference before the final response is generated. Each night it reviews the day's exchanges, evaluates every memory, and updates the system accordingly.

This separation between active reasoning and background memory management is both the core security boundary and the central architectural idea of the project.

---

## Memory

Memory is organized into four categories, each progressively harder to access than the last. Every memory carries a score that shifts over time, stored and retrieved using vector embeddings. A memory can move up through the categories if it proves important enough, or fade out entirely if it doesn't. The scoring logic is the part of the system I'm not sharing.

Decay isn't uniform. Memories that are tightly connected to each other and to the identity core resist fading. The more a memory is woven into the fabric of the system's self-model, the longer it holds. Isolated memories disappear faster.

At the top sits the axiom, the identity core. It doesn't decay. Everything else does.

---

## Security & Isolation

- Runs entirely inside WSL2, isolated from host filesystem
- Ollama inference bound to localhost, no external network routes
- Port lockdown: outbound TCP 443 for messaging gateway only, all other ports closed
- Quinn cannot write impactful files or take system actions directly, all of that routes through Watchdog
- External calls are explicit and logged

---

## Tech Stack

- **Model:** Qwen 2.5:14B via Ollama
- **Interface:** Messaging interface (discord.js / Node.js)
- **Memory:** JSONL stream logs + SQLite + vector embeddings
- **Environment:** WSL2 / Ubuntu 24
- **Languages:** Python (core pipeline), Node.js (Discord interface)

---

## Development Note

Portions of this project were built with AI assistance (Claude, Gemini). The architecture, memory system, routing logic, identity philosophy, and overall direction are my own. AI was used as an accelerant, not a replacement for the thinking.

---

