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
