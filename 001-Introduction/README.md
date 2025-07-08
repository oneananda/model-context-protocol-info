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


