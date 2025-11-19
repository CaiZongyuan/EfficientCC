---
name: agentscope-sdk
description: This skill should be used when users need to work with AgentScope framework for building AI-powered applications. It provides comprehensive guidance on agent creation, memory management, tool integration, workflow orchestration, and advanced features like RAG, planning, and evaluation.
---

# AgentScope SDK

## Core Functionality

AgentScope SDK provides a comprehensive framework for building AI-powered applications with multi-agent orchestration, memory management, tool integration, and advanced workflow patterns. The framework supports multiple model providers, concurrent execution, and sophisticated agent behaviors through ReAct patterns.

## When to Use

This skill should be used when users need to work with AgentScope framework for building AI-powered applications. It provides comprehensive guidance on agent creation, memory management, tool integration, workflow orchestration, and advanced features like RAG, planning, and evaluation.

## Module Overview

### Core Agent Module
- **Functionality**: Agent creation, ReActAgent, custom agents, message handling, real-time steering, parallel tool calls, and structured output
- **Key APIs**: ReActAgent, AgentBase, ReActAgentBase, UserAgent, ToolResponse, TextBlock, Msg, DashScopeChatModel, DashScopeChatFormatter, InMemoryMemory, Toolkit
- **Detailed documentation**: `references/task_agent.py`, `references/quickstart_agent.py`, `references/quickstart_key_concept.py`, `references/quickstart_message.py`

### Memory & State Module
- **Functionality**: InMemoryMemory, long-term memory, state management, sessions, persistent storage
- **Key APIs**: MemoryBase, InMemoryMemory, StateModule, register_state, state_dict, load_state_dict, JSONSession, Mem0LongTermMemory, ReMePersonalLongTermMemory
- **Detailed documentation**: `references/task_memory.py`, `references/task_state.py`, `references/task_long_term_memory.py`

### Model Integration Module
- **Functionality**: Multiple providers (OpenAI, DashScope, Anthropic, Gemini, Ollama), embeddings, token counting, streaming responses
- **Key APIs**: OpenAIChatModel, DashScopeChatModel, AnthropicChatModel, GeminiChatModel, OllamaChatModel, OpenAITextEmbedding, GeminiTextEmbedding, DashScopeTextEmbedding, OpenAITokenCounter
- **Detailed documentation**: `references/task_model.py`, `references/task_embedding.py`, `references/task_token.py`

### Tool Systems Module
- **Functionality**: Tool functions, toolkit management, MCP integration, automatic tool organization
- **Key APIs**: Toolkit, register_tool_function, call_tool_function, get_json_schemas, HttpStatefulClient, HttpStatelessClient, ToolResponse, execute_python_code
- **Detailed documentation**: `references/task_tool.py`, `references/task_mcp.py`

### Workflows Module
- **Functionality**: Concurrent agents, conversations, routing, handoffs, debates, pipelines, message broadcasting
- **Key APIs**: MsgHub, sequential_pipeline, SequentialPipeline, fanout_pipeline, FanoutPipeline, stream_printing_messages, asyncio.gather, RoutingChoice
- **Detailed documentation**: `references/workflow_concurrent_agents.py`, `references/workflow_conversation.py`, `references/workflow_handoffs.py`, `references/workflow_routing.py`, `references/workflow_multiagent_debate.py`, `references/task_pipeline.py`

### Advanced Features Module
- **Functionality**: RAG, planning, evaluation, tracing, Studio integration, prompt formatting
- **Key APIs**: PlanNotebook, TextReader, ImageReader, SimpleKnowledge, QdrantStore, GeneralEvaluator, @trace_llm, @trace_reply, DashScopeMultiAgentFormatter
- **Detailed documentation**: `references/task_plan.py`, `references/task_rag.py`, `references/task_eval.py`, `references/task_hook.py`, `references/task_prompt.py`, `references/task_studio.py`, `references/task_tracing.py`

## Workflow

1. **Set up AgentScope environment**:
   - Install agentscope package: `pip install agentscope`
   - Set up API keys as environment variables (e.g., `DASHSCOPE_API_KEY`)
   - Initialize AgentScope with `agentscope.init()`

2. **Create basic agents**:
   - Use ReActAgent for reasoning and acting capabilities
   - Configure with model, memory, formatter, and toolkit
   - Reference `references/task_agent.py` for detailed agent creation patterns

3. **Implement memory and state management**:
   - Add InMemoryMemory for conversation history
   - Use StateModule for persistent state management
   - Consider long-term memory integration with mem0 or ReMe
   - Reference `references/task_memory.py` and `references/task_state.py` when complex memory patterns are needed

4. **Integrate tools and model providers**:
   - Register tool functions with Toolkit
   - Configure multiple model providers (OpenAI, DashScope, Anthropic, etc.)
   - Set up embeddings for RAG applications
   - Reference `references/task_tool.py` and `references/task_model.py` for advanced tool integration

5. **Design workflows and agent orchestration**:
   - Use MsgHub for multi-agent communication
   - Implement routing patterns for agent selection
   - Create pipelines for sequential processing
   - Reference `references/workflow_routing.py` and `references/task_pipeline.py` for complex workflow patterns

6. **Add advanced features when needed**:
   - Implement RAG with TextReader and vector stores
   - Add planning capabilities with PlanNotebook
   - Set up evaluation frameworks
   - Enable tracing and monitoring
   - Reference specific documentation in `references/` directory for each feature

7. **Common patterns and best practices**:
   - Use async/await patterns for concurrent operations
   - Implement proper error handling with try/catch blocks
   - Use structured output with Pydantic models for consistency
   - Apply progressive disclosure in complex applications
   - Reference `references/quickstart_key_concept.py` for fundamental patterns

## Resource References

- **For detailed API specifications**: `references/task_agent.py`, `references/task_model.py`, `references/task_tool.py`
- **For code examples and templates**: `references/quickstart_agent.py`, `references/quickstart_message.py`
- **For complex workflows**: `references/workflow_conversation.py`, `references/workflow_routing.py`, `references/task_pipeline.py`
- **For advanced features**: `references/task_rag.py`, `references/task_plan.py`, `references/task_eval.py`
- **For development tools**: `references/task_studio.py`, `references/task_tracing.py`, `references/task_hook.py`