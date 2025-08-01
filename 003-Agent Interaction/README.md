﻿# Agent Interaction

### 🔹 How Agents Use the Context

**Context** is the metadata that helps the agent understand:

* **Who** is speaking (roles),
* **What** the ongoing conversation is about (session memory),
* **How** to behave (instructions/goals),
* **What constraints** to follow (tools, functions, limits).

#### Context may include:

* **Session ID**: Links current interaction to prior ones.
* **Instruction**: System-level directive like `"Be concise."`
* **Message History**: Recent user/assistant turns.
* **Tools/Functions**: APIs available to the agent.
* **Memory**: Facts the agent should retain (e.g., `"user is a software engineer"`).

> Agents do not hallucinate context — they act based on what is **explicitly given or remembered** via context.

---

### 🔹 Multi-Turn Communication Design

MCP models conversations as a series of **turns**, each turn containing:

| Element    | Description                           |
| ---------- | ------------------------------------- |
| `message`  | A user/assistant/system utterance     |
| `context`  | Relevant metadata or history          |
| `turn_id`  | Unique identifier for tracking        |
| `response` | The agent’s reply to the current turn |

**Design pattern:**

1. Each turn builds on the previous one.
2. Turns are linked via `session_id`.
3. Context can be dynamic (updating with new info per turn).
4. A single turn might lead to:

   * Immediate response
   * Tool invocation
   * Updated context for next turn

This enables:

* Persistent memory
* Task planning
* Role-based flow (e.g., user vs system)

---

### 🔹 Role of `system` / `user` / `assistant` Messages

| Role        | Purpose                                                                 |
| ----------- | ----------------------------------------------------------------------- |
| `system`    | Sets behavior: instructions, tone, tools, constraints                   |
| `user`      | Provides prompts, tasks, or questions                                   |
| `assistant` | Model’s response to `user` based on `system` guidance and prior context |

**Example:**

```json
[
  { "role": "system", "content": "You are a helpful coding assistant." },
  { "role": "user", "content": "How do I write a loop in Python?" },
  { "role": "assistant", "content": "You can use a for loop like this:\nfor i in range(5): print(i)" }
]
```

---

### 🔹 Examples of Structured Turn Messages

Here’s a structured **turn message** with full context:

```json
{
  "session_id": "abc123",
  "turn_id": "turn-001",
  "messages": [
    { "role": "system", "content": "You are an agent that explains medical terms simply." },
    { "role": "user", "content": "What is hypertension?" }
  ],
  "context": {
    "user_profile": { "name": "John", "age": 45 },
    "language": "English",
    "memory": { "prior_questions": ["What is diabetes?"] }
  },
  "response": {
    "role": "assistant",
    "content": "Hypertension is high blood pressure. It means the force of blood pushing against your arteries is too high."
  }
}
```

---
