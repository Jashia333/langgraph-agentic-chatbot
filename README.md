# 🧠 LangGraph Agentic Chatbot

A multi-tool, agentic AI chatbot built using **LangGraph**, **LangChain**, and **memory-based state management**. This project demonstrates how to build a graph-based LLM agent with human-interrupt tools, search capabilities, memory, and decision routing using LangGraph’s declarative interface.

---

## 🚀 Features

* ✅ **Tool-Oriented Agent**: Bind tools (like Tavily search or human escalation) to the LLM.
* 🔁 **Memory via StateGraph**: Uses `MemorySaver` for maintaining dialogue history.
* 🔄 **Interrupt Handling**: Captures user queries requiring human-in-the-loop responses.
* 🔧 **Graph-Driven Workflow**: Implements a LangGraph state machine with edges, conditionals, and tool routing.
* 💬 **LLM Backend**: Supports `groq:llama3-8b-8192` or other ChatGroq-compatible models.
* 📡 **Streaming Invocation**: Agent responses stream token-by-token with `stream_mode="values"`.

---

## ⚙️ Setup

### 1. Clone the repository

```bash
git clone https://github.com/Jashia333/langgraph-agentic-chatbot.git
cd langgraph-agentic-chatbot
```

### 2. Install dependencies

Make sure you have Python 3.10+ and run:

```bash
pip install -r requirements.txt
```

### 3. Set environment variables

Create a `.env` file with the following:

```env
GROQ_API_KEY=your_groq_key
TAVILY_API_KEY=your_tavily_key
```

---

## 🧠 How It Works

### ➤ Agent Construction

The agent is defined as a `StateGraph` with the following flow:

1. **START** → **LLM Tool Binding Node** (`human_assistance_llm`)
2. Conditional edge → If LLM calls a tool → **Tool Node**
3. **END**

### ➤ Tools

* `human_assistance`: Simulates requesting a human (via interrupt + resume).
* `TavilySearch`: Uses LangChain's Tavily plugin for web search.

### ➤ Memory

Memory is handled using:

```python
from langgraph.checkpoint.memory import MemorySaver
```

This enables the agent to maintain a threaded session.

---

## 🧪 Example Run

### `client.py` Example

```python
user_input = "I need help from a real AI expert. Can you ask someone to assist me?"
config = start_session(90)

events = human_feedback_llm.invoke(
    {"messages": [user_input]},
    config=config,
    stream_mode="values"
)

# Then resume with human feedback
human_command = Command(resume={"data": "Sure! Please check LangGraph for expert workflows."})
events = human_feedback_llm.stream(human_command, config, stream_mode="values")
```

---

## 📌 Concepts Demonstrated

| Concept           | Description                                     |
| ----------------- | ----------------------------------------------- |
| `StateGraph`      | Declarative LangGraph flow                      |
| `bind_tools()`    | Attach tools to LLM                             |
| `interrupt()`     | Pause for external/human input                  |
| `MemorySaver`     | Threaded memory management                      |
| `ToolNode`        | Executes tools from tool calls                  |
| `tools_condition` | Conditional transitions based on LLM tool usage |

---

## 🧰 Tool Addition Guide

To add a new tool:

1. Define the function with a docstring and return value
2. Add it to the `tools_used` list
3. Rebind with `llm.bind_tools([...])`

Example:

```python
def greet(name: str) -> str:
    """Greets a user by name"""
    return f"Hello, {name}!"
```

---

## 🧑‍💻 Author

Created by [@Jashia333](https://github.com/Jashia333)
AI + LangGraph + Multi-agent Tooling Enthusiast

---

## 📄 License

This project is licensed under the MIT License.

---

Let me know if you'd like a version that includes screenshots, demo GIFs, or badge formatting!
