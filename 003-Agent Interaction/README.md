# Agent Interaction

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
