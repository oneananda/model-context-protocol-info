# Core Concepts of MCP

MCP is a protocol that defines how contextual information (like user intent, preferences, session history, etc.) is exchanged between components of an AI system, especially between models and clients or orchestration layers.


Context: Structured information passed to the model (e.g., previous turns, user profile).

Messages: Units of communication between the client and the model.

Turns: A single round of interaction between the user and the model.

Components: Clients, model endpoints, routers/orchestrators.

## Minimal Setup

Start with a basic setup:

A user input (a question).

A context object (some metadata or session state).

A model endpoint that can receive and respond to MCP-style input.

## Build a Simple Example

Write a minimal program that:

Sends an MCP turn to a language model (or a stub service).

Receives a response and logs it.

### STEP 1: Define the MCP Payload Format

Create a Python dictionary to represent the MCP message format:

````python
mcp_turn = {
    "type": "turn",
    "user_id": "user-123",
    "messages": [
        {
            "role": "user",
            "content": "Hello, what is the capital of France?"
        }
    ],
    "context": {
        "session_id": "session-456",
        "timestamp": "2025-07-03T12:00:00Z"
    }
}
````

| Field      | Type   | Description                                                                                           |
| ---------- | ------ | ----------------------------------------------------------------------------------------------------- |
| `type`     | string | Indicates the kind of payload. `"turn"` means it's a user turn in a dialogue.                         |
| `user_id`  | string | Unique identifier for the user sending the message. Used for tracking sessions and personalization.   |
| `messages` | list   | A list of message objects. Each message has a `role` (e.g., `"user"` or `"assistant"`) and `content`. |
| `context`  | object | Structured metadata that provides context like session ID, timestamps, preferences, etc.              |

**Turn**

Definition: A turn is a single interaction cycle between a user and the system (typically a language model).

It may contain just a user message (input only), or both user message and model reply.

MCP uses turns to structure communication incrementally, just like conversational turns in real life.

**Think of it as: "A user says something → the model may respond → that’s a turn."**

**Message**

Definition: A message is an individual communication element within a turn.

Each message has a role and content.

Roles typically include:

"user" – messages from the human

"assistant" – model-generated replies

(optionally) "system" – special instructions or context (e.g., constraints or goals)

**A turn can include one or more messages (e.g., user input and assistant response).**

**Context**

Definition: Context is structured metadata attached to a turn that helps the model understand the situation.

Context may include:

Session ID – to track continuity across turns

Timestamp – to order and time-stamp interactions

User Preferences – like language, units (e.g., Celsius), tone

Previous Interactions – past messages, history summaries

Environment Variables – location, app state, permissions

**Think of context as the envelope the message comes in, giving background info.**

**Turn Response**

Definition: This is the model’s reply to a turn

The response can also include updated context, like model-inferred user intent or modified preferences.

**Session**

Definition: A session refers to the ongoing conversation or interaction between the user and the model, possibly over multiple turns.

Identified by session_id

Enables memory, personalization, and state tracking

**Instruction (System Prompt)**

In some setups, an additional system-level instruction can guide the model's behavior.

**This can appear in the messages list or separately in context**.

---

### ✅ **Summary Table**

| Concept         | Type     | Purpose                                                                                |
| --------------- | -------- | -------------------------------------------------------------------------------------- |
| `turn`          | Request  | A single cycle of user input (possibly with context)                                   |
| `message`       | Object   | A unit of communication (can be from user, assistant, or system)                       |
| `context`       | Object   | Metadata that informs the model's behavior (e.g., session info, preferences)           |
| `turn_response` | Response | The model’s reply to a `turn`, often includes updated context                          |
| `session`       | ID       | Identifies and groups multiple turns within a single interaction (used for continuity) |
| `instruction`   | Message  | A system-level behavior directive (optional)                                           |

---

### 🔁 **Turn Lifecycle Flow**

1. **User** sends input to the **client**.
2. The **client** creates a `turn`, including the user's message and any **context**.
3. The `turn` is sent to the **model**.
4. The **model** processes the `turn` and generates a `turn_response` (with or without updated context).
5. The **client** receives the `turn_response` and displays the assistant's reply to the **user**.

---

### 🔄 **MCP Lifecycle Flow**

* The **User** interacts with the **Client**, sending a message.
* The **Client** creates a **Turn Object**, which includes the user's message and any relevant **context** (e.g., session or preferences).
* The **Turn Object** is sent to the **Model** for processing.
* The **Model** processes the message and generates a **Turn Response** (model output), possibly with updated context.
* The **Turn Response** is sent back to the **Client**.
* The **Client** displays the assistant's reply to the **User**.
* A **Logger** or **monitoring tool** might capture the interaction for tracking purposes.

---

