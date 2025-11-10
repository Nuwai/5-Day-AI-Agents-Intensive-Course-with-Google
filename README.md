# 🤖 Kaggle × Google: 5-Day Intensive AI Agent Course
## Overview

This repository documents my learning journey through the Kaggle × Google “AI Agents” 5-Day Intensive Course (2025) — an official hands-on program designed to introduce developers and researchers to Google’s Agent Development Kit (ADK) and Gemini API.

Throughout the course, participants learn how to design, orchestrate, and deploy AI agents that can reason, take actions, and collaborate to solve complex tasks.
All notebooks, notes, and implementations are based on the official Google ADK tutorials and my own reflections and experiments.

## 🧩 Course Structure

| **Day**   | **Topic**                    | **Core Concepts**                                       |
| --------- | ---------------------------- | ------------------------------------------------------- |
| **Day 1** | Introduction to AI Agents    | Building your first AI agent with Gemini + ADK          |
| **Day 2** | Multi-Agent Systems          | Designing specialized agent teams and workflow patterns |
| **Day 3** | Custom Tools & Functions     | Integrating APIs, extending agent capabilities          |
| **Day 4** | Long-Running & Memory Agents | Session state, persistence, and iterative reasoning     |
| **Day 5** | Deployment & Applications    | Real-world use cases and system design best practices   |

## Learning Highlights
### Day 1: Your First AI Agent

Main Goal: Build and run a simple agent that can reason and take actions using Google’s Agent Development Kit (ADK).

What I Implemented

- Configured the Gemini 2.5 Flash-Lite model via API key authentication.
- Installed and imported the google-adk toolkit in Kaggle.
- Created a “Helpful Assistant Agent” capable of:
- Reasoning through prompts.
- Taking actions using the google_search tool.
- Producing dynamic, up-to-date answers.
- Explored the ADK Web UI for live debugging and observability.

💡 Key Learning Points

- gents ≠ Chatbots:
- Agents don’t just respond; they decide, act, and observe to refine their answers.
 ```Prompt → Agent → Thought → Action → Observation → Final Answer```
- Tool Integration: By adding external tools (e.g., Google Search), an agent gains grounded reasoning instead of hallucinations.
- Reasoning Traceability: The ADK Web UI makes it possible to visualize internal reasoning steps (“thoughts” and “actions”).
- Workflow Design: Clear “instruction” and “description” prompts define an agent’s behavior and reliability.

#### My Reflection

Building the first agent demonstrated how LLMs evolve into goal-driven systems when connected to tools.
It reinforced the shift from prompt engineering to agent orchestration, where context, control, and observation loops define intelligence.

## Multi-Agent Systems & Workflow Patterns
Main Goal: Extend from a single agent to a multi-agent system, where agents collaborate under orchestrated workflows.

What I Implemented

- Built a Research + Summarization System using specialized agents:
  - ResearchAgent → gathers info via Google Search
  - SummarizerAgent → produces concise summaries
  - CoordinatorAgent → manages workflow via AgentTool

- Explored deterministic workflow types:
  - Sequential Agents: fixed pipelines (Outline → Write → Edit)
  - Parallel Agents: concurrent execution for independent subtasks
  - Loop Agents: iterative refinement cycles (Writer ↔ Critic)

💡 Key Learning Points
Divide & Conquer: Complex tasks become reliable when split into smaller, specialized agents.
Workflow Patterns:
| Pattern             | Use Case             | Key Strength                |
| ------------------- | -------------------- | --------------------------- |
| **SequentialAgent** | Linear pipelines     | Predictable, ordered output |
| **ParallelAgent**   | Independent subtasks | Efficiency + speed          |
| **LoopAgent**       | Quality refinement   | Iterative improvement       |

- State Sharing: output_key enables agents to exchange and build upon each other’s outputs.
- Scalability Insight: Multi-agent orchestration mirrors team collaboration — coordination and specialization are key.

#### My Reflection

Multi-agent workflows introduced the engineering discipline of AI systems design.
I learned how orchestration logic, rather than prompt complexity, determines success.
This perspective bridges traditional software engineering with modern AI development.

🧰 Tech Stack

- Languages: Python 3
- Frameworks / Libraries: google-adk, google-genai, kaggle-secrets
- LLM: Gemini 2.5 Flash-Lite
- Environment: Kaggle Notebooks
- Tools: ADK Web UI, Google AI Studio API Keys

  ----
