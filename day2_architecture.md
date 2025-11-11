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

```
User: "Convert 1250 USD to INR using Bank Transfer"
        │
        ▼
[🤖 Enhanced Currency Agent]
    │
    ├─> Calls 💳 get_fee_for_payment_method()
    │       └─ Returns: {"fee_percentage": 0.01}
    │
    ├─> Calls 💱 get_exchange_rate()
    │       └─ Returns: {"rate": 83.58}
    │
    ├─> Delegates to 🧮 Calculation Agent
    │       ├─ Generates Python code
    │       └─ Sends code to ⚙️ BuiltInCodeExecutor (Sandbox)
    │
    ├─> Receives computed result
    │
    └─> Formats detailed explanation and returns:
            - Fee breakdown 💳
            - Exchange rate 💱
            - Final converted amount 💰
```



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

---


### Agent Tool Patterns & Best Practices
## Focus: Building production-grade agent workflows that can connect to real services and handle human-in-the-loop decisions.

### 1. Concept Overview

| Topic                            | Purpose                                                                                                                           |
| -------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **MCP (Model Context Protocol)** | Lets your agent plug into external services (GitHub, Maps, Kaggle etc.) through a universal standard – no custom API code needed. |
| **Long-Running Operations**      | Allow an agent to pause for human approval or long tasks, and resume with the same state later.                                   |


### 2. MCP Integration

#### What is MCP?

MCP is like the USB protocol for agents — a standard way to connect models to external tools.
Instead of writing custom REST or SDK wrappers, your agent connects to an MCP Server, which advertises the tools it supports.

#### Architecture (Concept)
```
Agent (MCP Client)
   │
   ├─> Standard MCP Protocol (JSON-RPC)
   │
   ├─> GitHub MCP Server
   ├─> Slack MCP Server
   └─> Google Maps MCP Server
```
Each server exposes a consistent JSON-schema-based toolset, and the ADK (McpToolset) handles discovery and execution.

#### Code Walkthrough — Connecting to Everything Server

```
from google.adk.tools.mcp_tool.mcp_toolset import McpToolset
from google.adk.tools.mcp_tool.mcp_session_manager import StdioConnectionParams
from mcp import StdioServerParameters

mcp_image_server = McpToolset(
    connection_params=StdioConnectionParams(
        server_params=StdioServerParameters(
            command="npx",
            args=["-y", "@modelcontextprotocol/server-everything"],
            tool_filter=["getTinyImage"],
        ),
        timeout=30,
    )
)
```

#### What happens under the hood

- ADK runs npx @modelcontextprotocol/server-everything (a demo MCP server).
- Establishes stdio communication with the server.
- The server announces its tools → here, getTinyImage.
- The agent can now call getTinyImage() as if it were a local Python function.

#### Building the Image Agent

```
from google.adk.agents import LlmAgent
from google.adk.models.google_llm import Gemini

image_agent = LlmAgent(
    name="image_agent",
    model=Gemini(model="gemini-2.5-flash-lite", retry_options=retry_config),
    instruction="Use the MCP Tool to generate images for user queries",
    tools=[mcp_image_server],
)
```
Result:
Your agent just produced an image by talking to an external MCP server — no custom integration needed.

Extend to Other MCP Servers:
- Kaggle MCP → Access datasets and competitions
- GitHub MCP → Analyze PRs/issues
- Google Cloud MCP → Query BigQuery, manage storage
- All follow the same McpToolset(connection_params=...) pattern.

### 3. Long-Running Operations (Human-in-the-Loop)
#### The Problem

Most tools return immediately, but real-world operations often need time or human approval.
Example: “Ship 10 containers to Rotterdam — are you sure?”
We need a way to:
1. Pause the agent’s execution.
2. Wait for human input.
3. Resume with the same context and variables.

The Tool: place_shipping_order()
Logic Flow
1. Small order → auto-approved.
2. Large order (first call) → requests confirmation and pauses.
3. Large order (resume) → reads approval decision and finishes.

The Agent + Resumable App

Agents are stateless; they forget context between runs.
To resume after a pause, you wrap the agent in an App with ResumabilityConfig.

```
from google.adk.apps.app import App, ResumabilityConfig
from google.adk.sessions import InMemorySessionService
from google.adk.runners import Runner

shipping_agent = LlmAgent(
    name="shipping_agent",
    model=Gemini(model="gemini-2.5-flash-lite", retry_options=retry_config),
    instruction="Use the place_shipping_order tool...",
    tools=[FunctionTool(func=place_shipping_order)],
)

shipping_app = App(
    name="shipping_coordinator",
    root_agent=shipping_agent,
    resumability_config=ResumabilityConfig(is_resumable=True),
)

shipping_runner = Runner(app=shipping_app, session_service=InMemorySessionService())
```
Workflow Function – Pause & Resume

```
async def run_shipping_workflow(query: str, auto_approve: bool = True):
    session_id = f"order_{uuid.uuid4().hex[:8]}"

    # Initial run: may trigger approval request
    async for event in shipping_runner.run_async(user_id="test_user", session_id=session_id,
                                                 new_message=types.Content(role="user", parts=[types.Part(text=query)])):
        events.append(event)

    approval_info = check_for_approval(events)

    if approval_info:
        # Pause detected → simulate human decision
        async for event in shipping_runner.run_async(
            user_id="test_user",
            session_id=session_id,
            new_message=create_approval_response(approval_info, auto_approve),
            invocation_id=approval_info["invocation_id"],   # critical: resume same run
        ):
            print_agent_response([event])
    else:
        print_agent_response(events)
```

#### Execution Flow Summary
| Step | What Happens                                | Key Concept                                |
| ---- | ------------------------------------------- | ------------------------------------------ |
| 1    | User: “Ship 10 containers to Rotterdam”     | Start session                              |
| 2    | Agent calls `place_shipping_order()`        | Tool logic                                 |
| 3    | Tool requests confirmation                  | ADK emits `adk_request_confirmation` event |
| 4    | Workflow detects pause                      | Saves `invocation_id`                      |
| 5    | Human approves/rejects                      | Creates `FunctionResponse`                 |
| 6    | Workflow resumes same run (`invocation_id`) | ADK restores state                         |
| 7    | Tool finishes and returns final message     | Conversation continues seamlessly          |

#### 💡 Key ADK Concepts Used

| Concept                            | Description                                                 |
| ---------------------------------- | ----------------------------------------------------------- |
| **ToolContext**                    | Lets a tool ask for confirmation or check approval.         |
| **adk_request_confirmation event** | System-generated event signaling “pause here for approval.” |
| **invocation_id**                  | Unique ID linking initial call to resumed execution.        |
| **App + ResumabilityConfig**       | Persists agent state across pauses.                         |

### 4. Putting It Together – Demo Flows
| Scenario                         | Behavior                                                |
| -------------------------------- | ------------------------------------------------------- |
| **Ship 3 containers**            | Auto-approved immediately (`≤ 5`).                      |
| **Ship 10 containers → approve** | Pauses → simulated human approval → resumes → approved. |
| **Ship 8 containers → reject**   | Pauses → simulated rejection → resumes → rejected.      |

Demonstrates complete pause-resume lifecycle.

#### 📦 Shipping Agent – Pause & Resume Flow

```
User: "Ship 10 containers to Rotterdam"
        │
        ▼
[Shipping Agent]
  │
  ├─> Calls place_shipping_order()
  │     ├─ If ≤5 containers → auto-approve ✅
  │     └─ If >5 containers → request_confirmation()
  │
  ▼
ADK sends "adk_request_confirmation" event (agent paused ⏸️)
  │
  ├─ Human approves ✅ or rejects ❌
  │
  ▼
Workflow resumes same invocation_id
  │
  ├─ Tool reads confirmation → returns approved/rejected
  │
  ▼
Agent sends final message to user
```

### 5. Key Takeaways

| Pattern                     | When to Use                                                           | Core Components                                                    |
| --------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------ |
| **MCP Integration**         | Connect agents to external tools via standard protocol (no API code). | `McpToolset`, `StdioConnectionParams`                              |
| **Long-Running Operations** | Add human-in-the-loop or time-delayed steps.                          | `ToolContext`, `request_confirmation`, `App`, `ResumabilityConfig` |

#### Conceptual Links to Earlier Notebooks
| From                               | To               | Evolution                                                           |
| ---------------------------------- | ---------------- | ------------------------------------------------------------------- |
| **Day 2A (Custom Tools)**          | **Day 2B (MCP)** | From writing your own Python tools → to connecting community tools. |
| **Day 2A (Agent Tool Delegation)** | **Day 2B (LRO)** | From agent delegation → to time-based pause and resume.             |

💬 My Insights

This notebook showed me how enterprise-grade agents handle real-world workflows:
– MCP makes integration plug-and-play.
– Long-Running Operations give agents patience and accountability.
– Together, they transform static chatbots into reliable autonomous systems.*

TL;DR Summary
- MCP connects your agent to the world.
- LRO lets it think in human time.
- Combined with ADK App resumability, they make agents safe, compliant, and stateful.
