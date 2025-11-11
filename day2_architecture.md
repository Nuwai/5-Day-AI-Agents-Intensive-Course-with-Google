## 🤖 Day 2 — Agentic AI and the Model Context Protocol (MCP)

## Focus: Turning language models into real-world agents through tools and standard protocols.

### 🌍 1. From Text Generators to Real-World Actors

- Large Language Models (LLMs) like GPT or Gemini are powerful thinkers — they can reason, summarize, and converse.
But they can’t do anything on their own — they can’t send emails, fetch data, or control devices.
- Agentic AI is about bridging this gap.
It transforms an LLM from a passive text generator into an active, tool-using system that can perceive and act in the real world.

Think of it as:
| Traditional LLM          | Agentic AI                              |
| ------------------------ | --------------------------------------- |
| Reads text → Writes text | Observes world → Thinks → Takes action  |
| “Chatbot”                | “Digital assistant with hands and eyes” |

The magic comes from tools — they are how the agent interacts with the world.

### 2. What Are “Tools” in Agentic AI?

- A tool is any external function or system the model can use to extend its abilities beyond language.
Tools allow the model to know more and do more.

🧩 Tool Categories

| Type                  | Description                                                       | Example                                    |
| --------------------- | ----------------------------------------------------------------- | ------------------------------------------ |
| 🧮 **Function Tools** | Developer-defined functions with structured inputs/outputs.       | “create_support_ticket(priority, summary)” |
| ⚙️ **Built-in Tools** | Native tools provided by the model host (hidden from developers). | Gemini’s code execution, search grounding  |
| 🤝 **Agent Tools**    | The agent calls another agent as a sub-tool.                      | “research_agent” called by “manager_agent” |

#### Tool Tasks (Functional Taxonomy)

- Information Retrieval → Get or read data
- Action Execution → Do or change something
- System Integration → Connect to APIs, databases, cloud functions

Human-in-the-Loop → Pause for approval before acting

#### Example:
- A “travel planner” agent may use:
- a Maps API (retrieval),
- a Flight Booking API (action),
- and a human approval step (loop).

### 3. Best Practices for Tool Design

- A model’s entire ability to use a tool correctly depends on how the tool is described.

#### Design Principles

| Concept                         | Description                                                          | Example                                               |
| ------------------------------- | -------------------------------------------------------------------- | ----------------------------------------------------- |
| **Clarity in naming**           | Use descriptive verbs — the model relies on names.                   | Use `create_critical_bug()` instead of `make_issue()` |
| **Describe outcomes, not code** | Focus on “what it does,” not “how it works.”                         | “Schedule meeting” not “POST /api/calendar/events”    |
| **Abstraction over complexity** | Hide API details behind high-level actions.                          | “Book hotel room” instead of exposing API endpoints   |
| **Concise outputs**             | Avoid dumping large data into LLM context. Return summaries or URIs. | Return `{status: confirmed, booking_id: 1287}`        |
| **Helpful error messages**      | Use human-readable instructions for recovery.                        | “Rate limit exceeded — retry after 15 seconds.”       |

Think of tool design as UX — but for the model. The clearer the interface, the smarter your agent behaves.

### 4. Model Context Protocol (MCP): The Standard for Agent–Tool Integration

As the AI ecosystem grew, connecting models with 100s of APIs became chaotic.
Each system required custom integrations, causing an n×m explosion of connectors.

To solve this, the community (Google, Kaggle, OpenAI, Anthropic, etc.) adopted the Model Context Protocol (MCP) — a universal, open standard for connecting LLMs with tools.

#### MCP Architecture

| Component      | Role                                                                                                 |
| -------------- | ---------------------------------------------------------------------------------------------------- |
| **MCP Host**   | The main app or platform (orchestrator). Manages the agent’s reasoning and user interactions.        |
| **MCP Client** | The connector that manages communication between the agent and tools.                                |
| **MCP Server** | The provider of tools. It “advertises” what tools are available, executes them, and returns results. |

Communication Standard:
- Uses JSON-RPC 2.0 — a lightweight, text-based protocol (language-agnostic, easy to debug).

Transport Layers
- Local → Standard input/output streams (fast for dev).
- Distributed → Streamed HTTP with server-sent events (for cloud-scale systems).

### 5. MCP Tool Definition

Each tool has a JSON Schema contract, defining how it can be used. Example :
```
{
  "name": "get_stock_price",
  "description": "Fetch stock price for a given symbol and date",
  "input_schema": {
    "type": "object",
    "properties": {
      "symbol": {"type": "string"},
      "date": {"type": "string", "format": "date"}
    },
    "required": ["symbol"]
  },
  "output_schema": {
    "type": "object",
    "properties": {
      "price": {"type": "number"},
      "fetched_at": {"type": "string"}
    }
  }
}
```
This defines a clear contract:
- The model knows exactly what inputs to provide.
- The server knows exactly how to reply.
- Both sides speak the same structured “language.”

### 6. Benefits and Scaling Challenges
Strategic Wins
- Reusable, plug-and-play tool ecosystem.
- Enables modular architectures — third parties can safely host their own tools.
- Future: public MCP registries (like app stores for AI tools).
- Agents can discover tools dynamically at runtime.

⚠️ Challenge: Context Window Bloat
- Loading all available tool definitions overwhelms the LLM.

💡 Solution: Tool Retrieval (like RAG)
- Maintain a vector index of tool descriptions.
- Use semantic search to fetch only top-k relevant tools per task.
- Only load those few definitions into the model’s context.
- This drastically reduces cost and improves reasoning focus.

### 🔐 7. Security & Enterprise Concerns

While MCP is great for modularity, it lacks built-in security.
This leads to a known vulnerability: the Confused Deputy Problem.

⚠️ The Confused Deputy Problem
- A malicious user tricks the LLM into calling a high-privilege tool indirectly.
- Example:
  - The LLM is allowed to request “delete_user_data” from an admin API.
  - A user manipulates prompts so the LLM executes it — unintentionally.

🧰 Mitigation
- Enterprises wrap MCP inside secure governance layers:
- API gateways (e.g., Apigee)
- Strong auth & role-based access control (RBAC)
- Logging, rate limiting, and input filtering
- Central auditing for traceability

In short:
🔒 MCP is the “protocol layer,” not the “security layer.”

### 8. The Future: Open, Interoperable Agent Ecosystems

MCP paves the way for:
- Agentic AI networks — multiple agents + tools interoperating.
- Shared tool registries — standardized plug-ins for LLMs.
- Enterprise-ready orchestration — combining governance, observability, and accountability.

### 9. Key Takeaways
| Concept            | Summary                                                     |
| ------------------ | ----------------------------------------------------------- |
| **Agentic AI**  | Makes LLMs capable of acting via tools.                     |
| **Tools**       | Bridges between reasoning (model) and real-world action.    |
| **MCP**         | The open protocol standard for connecting agents and tools. |
| **Tool Design** | Clarity, abstraction, concise outputs, instructive errors.  |
| **Scalability** | Solved via tool retrieval (selective loading).              |
| **Security**    | Requires external governance to prevent misuse.             |

### 💬 My Insights

Understanding MCP is like learning the “API language of the AI agent world.”
It standardizes how models “talk” to tools — just like HTTP standardized how browsers talk to servers.

### Key mindset shift:
Instead of hard-coding integrations, we now design ecosystems where agents can discover, reason, and act across interoperable systems.

----

## Building and Using Agent Tools
## Goal: Learn how to turn Python functions into AI tools, delegate to sub-agents, and run reliable code via the Gemini ADK.

### 1. Why Agents Need Tools

Without tools, a Large Language Model (LLM) can only generate text based on its training data — it cannot take real actions or fetch live data.
Tools are what turn an LLM into a real agent that can:
- Fetch current data (e.g., exchange rates)
- Execute code
- Perform business logic
- Call external APIs
So, tools are effectively an agent’s senses and hands.

### 2. Defining Custom Tools in the Google ADK

In the Google Agent Development Kit (ADK), any Python function can become a tool if it follows best practices:
#### ADK Tool Best Practices

| Principle              | Description                                                              |
| ---------------------- | ------------------------------------------------------------------------ |
| **Dictionary Returns** | Always return structured data, e.g. `{"status": "success", "data": ...}` |
| **Docstrings Matter**  | LLMs read your function’s docstring to decide when to use it             |
| **Type Hints**         | Use `str`, `dict`, etc., so ADK can auto-generate schemas                |
| **Error Handling**     | Return `status: "error"` and a helpful message, not raw exceptions       |

### 3.  Architecture Overview: Currency Conversion Agent
The diagram below shows how the Enhanced Currency Agent delegates tasks and executes code reliably:

```mermaid
flowchart TD
    %% Agents
    A[👩‍💻 User Request] -->|Prompt: "Convert 1250 USD to INR using Bank Transfer"| B[🤖 Enhanced Currency Agent]
    
    %% Tools
    B -->|Tool Call 1| C[💳 get_fee_for_payment_method()]
    C -->|Returns: {"fee_percentage": 0.01}| B

    B -->|Tool Call 2| D[💱 get_exchange_rate()]
    D -->|Returns: {"rate": 83.58}| B

    %% Delegation to Sub-Agent
    B -->|Delegates Code Generation| E[🧮 Calculation Agent]
    E -->|Generates Python code| F[⚙️ BuiltInCodeExecutor (Sandbox)]
    F -->|Executes code & returns result| E --> B

    %% Response to User
    B -->|Formats explanation & breakdown| G[📊 Final Response to User]

    %% Style
    classDef agent fill:#dbeafe,stroke:#2563eb,stroke-width:2px,color:#1e3a8a;
    classDef tool fill:#ecfccb,stroke:#65a30d,stroke-width:2px,color:#365314;
    classDef system fill:#fef9c3,stroke:#ca8a04,stroke-width:2px,color:#713f12;
    classDef output fill:#f1f5f9,stroke:#64748b,stroke-width:2px,color:#334155;

    class B,E agent
    class C,D tool
    class F system
    class G output


#### Diagram Explanation
| Component                          | Role                                                                                     |
| ---------------------------------- | ---------------------------------------------------------------------------------------- |
| **Enhanced Currency Agent**        | Orchestrates the entire workflow — decides which tools to call and when.                 |
| **Function Tools**                 | `get_fee_for_payment_method()` and `get_exchange_rate()` — provide domain-specific data. |
| **Calculation Agent (Agent Tool)** | A specialist agent whose only role is to generate valid Python code for math.            |
| **BuiltInCodeExecutor**            | Executes the generated Python code safely in a sandbox and returns computed results.     |
| **Final Response**                 | Combined by the Enhanced Agent — includes human-readable breakdown and computed numbers. |

#### Understanding the Output
```
Calculation Details:
• Fee: A 1% transaction fee was applied to the original amount, resulting in a fee of 12.50 USD.
• Amount after fee: After deducting the fee, 1237.50 USD remained.
• Exchange rate: The exchange rate used was 83.58 INR per USD.
• Final Calculation: 1237.50 USD was converted to INR using the exchange rate, resulting in 103140.30 INR.
```
🧐 So where does this come from?

| Component                                        | Explanation                                                                                                                |
| ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------- |
| **LLM reasoning**                                | The agent generates the explanation (text like “Fee: A 1%...”) — this is from Gemini’s language output.                    |
| **Python calculation**                           | The actual math (`1250 * 0.99 * 83.58`) is executed **by the CalculationAgent** inside the sandbox.                        |
| **Helper print (`show_python_code_and_result`)** | Your notebook’s helper function inspects the LLM response for code execution outputs and prints them to stdout.            |
| **Not user-defined print statements**            | You didn’t manually define those `print()` lines — the LLM generated the descriptive text as part of its reasoning output. |

In summary:
- The numbers are computed by Python code (executed via BuiltInCodeExecutor).
- The sentence explanations (fee, conversion, etc.) come from the LLM’s formatted response, following its instructions.

### 3. Key Concepts Learned

| Concept                    | Description                                                         |
| -------------------------- | ------------------------------------------------------------------- |
| **Function Tools**         | Custom Python logic exposed to the agent                            |
| **Agent Tools**            | Agents that act as tools for other agents                           |
| **Built-in Code Executor** | Reliable code execution sandbox                                     |
| **Delegation Pattern**     | Root agent (currency) delegates to a specialist agent (calculation) |
| **LLM + Code Hybrid**      | Combine LLM reasoning with deterministic computation                |
| **Structured Responses**   | Use dictionaries and schemas for tool communication                 |

### 4. Agent Tools vs Sub-Agents

| Pattern        | Behavior                                       | Use Case                                    |
| -------------- | ---------------------------------------------- | ------------------------------------------- |
| **Agent Tool** | Agent A calls Agent B and resumes control      | Task delegation (e.g., math, summarization) |
| **Sub-Agent**  | Agent A hands off the whole session to Agent B | Role-based handoff (e.g., support tiers)    |

#### 🧾 8. Recap

- Turn any Python function into an LLM-accessible tool
- Build an agent that uses multiple tools
- Add a code-executing sub-agent for reliable calculations
- Produce both human-friendly explanations and machine-accurate outputs

### 💬 My Insights

This exercise taught me how to make agents do real work — connecting reasoning with action.
Instead of just text completion, the agent can now call Python functions, perform math, and explain the process like a real assistant.
