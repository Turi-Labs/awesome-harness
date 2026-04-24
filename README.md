# Awesome Harness [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of resources for building reliable AI agent harnesses - the scaffolding of context, tools, memory, sandboxing, and guardrails that surrounds a model and determines whether an agent actually works.

**Harness engineering** is the practice of shaping the environment *around* the model. The model is fixed; the harness is yours to design. This list is organized by the problems you'll actually face: managing context, designing tools, isolating execution, evaluating quality, and keeping agents safe in production.

Entries are selected for practical signal - primary-source articles, reference implementations, and production-tested frameworks over generic commentary.

---

## Contents

- [Foundations](#foundations)
- [Context & Memory](#context--memory)
- [Tool Design & Protocols](#tool-design--protocols)
- [Sandboxing & Execution](#sandboxing--execution)
- [Orchestration & Frameworks](#orchestration--frameworks)
- [Evaluation & Benchmarks](#evaluation--benchmarks)
- [Observability & Reliability](#observability--reliability)
- [Guardrails & Security](#guardrails--security)
- [Reference Implementations](#reference-implementations)
- [Templates](#templates)
- [Contributing](#contributing)

---

## Foundations

The conceptual bedrock. Read these before building anything.

- [Harness engineering (OpenAI)](https://openai.com/index/harness-engineering/) - OpenAI's field report on building a large production app with Codex using architectural constraints, repo-local instructions, browser validation, and telemetry. The essay that named the discipline.
- [Effective harnesses for long-running agents (Anthropic)](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) - Initializer agents, feature lists, `init.sh`, self-verification, and handoff artifacts for agents that span multiple context windows.
- [Harness design for long-running application development (Anthropic)](https://www.anthropic.com/engineering/harness-design-long-running-apps) - Follow-up focused on task state and evaluator design for sustained app generation.
- [The Anatomy of an Agent Harness (LangChain)](https://blog.langchain.com/the-anatomy-of-an-agent-harness/) - Clean decomposition of an agent as model + harness, with prompts, tools, middleware, orchestration, and runtime infrastructure as first-class parts.
- [Harness Engineering (Martin Fowler / Thoughtworks)](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html) - Frames harness work into context engineering, architectural constraints, and entropy control ("garbage collection"). Good conceptual grounding.
- [Building effective agents (Anthropic)](https://www.anthropic.com/engineering/building-effective-agents) - When to use workflows vs. autonomous agents, how to structure tools, and where harness design beats raw prompting.
- [Your Agent Needs a Harness, Not a Framework (Inngest)](https://www.inngest.com/blog/your-agent-needs-a-harness-not-a-framework) - State, retries, traces, and concurrency as first-class infrastructure concerns, not afterthoughts.
- [Skill Issue: Harness Engineering for Coding Agents (HumanLayer)](https://www.humanlayer.dev/blog/skill-issue-harness-engineering-for-coding-agents) - The practical argument that weak agent results are usually harness failures, not model failures.
- [Improving Deep Agents with harness engineering (LangChain)](https://blog.langchain.com/improving-deep-agents-with-harness-engineering/) - Evidence that harness changes alone move benchmark numbers significantly.
- [Scaling Managed Agents (Anthropic)](https://www.anthropic.com/engineering/managed-agents) - Meta-harness architecture decoupling session logs, harness loops, and sandboxes in long-horizon agents.
- [Unrolling the Codex agent loop (OpenAI)](https://openai.com/index/unrolling-the-codex-agent-loop/) - Deep dive into the Codex harness loop: prompt growth, tool-call replay, and stateless execution tradeoffs.
- [12 Factor Agents (HumanLayer)](https://www.humanlayer.dev/blog/12-factor-agents) - Operating principles for production agents: explicit prompts, state ownership, and clean pause-resume behavior.
- [Agent Harness for LLM Agents: A Survey](https://www.preprints.org/manuscript/202604.0428) - Academic survey that formalizes and taxonomizes harness design as a distinct research area.

## Context & Memory

The context window is a budget, not a dumping ground.

- [Effective context engineering for AI agents (Anthropic)](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents) - Managing the context window as working memory: what to include, what to evict, how to compact.
- [Context Engineering for AI Agents: Lessons from Building Manus](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus) - KV-cache locality, tool masking, filesystem memory, and keeping useful failures in-context. Hands-on.
- [Context Engineering for Coding Agents (Thoughtworks)](https://martinfowler.com/articles/exploring-gen-ai/context-engineering-coding-agents.html) - Shaping the task environment so coding agents stay grounded across long sessions.
- [Advanced Context Engineering (HumanLayer)](https://www.humanlayer.dev/blog/advanced-context-engineering) - Patterns for reducing context drift and making sessions easier to resume.
- [OpenHands Context Condensation](https://openhands.dev/blog/openhands-context-condensensation-for-more-efficient-ai-agents) - Bounded conversation memory preserving goals, progress, critical files, and failing tests.
- [Writing a good CLAUDE.md (HumanLayer)](https://www.humanlayer.dev/blog/writing-a-good-claude-md) - Practical guide to durable, repo-local agent instructions that survive session boundaries.
- [everything-claude-code](https://github.com/affaan-m/everything-claude-code) - Large community repo of harness practices around memory, skills, and context control for coding agents.
- [claude-mem](https://github.com/thedotmack/claude-mem) - Plugin-style memory layer that captures session history and reinjects relevant context into future runs.
- [Context-Engineering Handbook](https://github.com/davidkimai/Context-Engineering) - First-principles handbook on context engineering for agent systems.
- [planning-with-files](https://github.com/OthmanAdi/planning-with-files) - Skill package for persistent file-based planning in coding-agent workflows.

## Tool Design & Protocols

How you expose capabilities to the agent determines what the agent can safely do.

- [Writing effective tools for agents (Anthropic)](https://www.anthropic.com/engineering/writing-tools-for-agents) - Tool interface design: naming, schemas, error surfaces, and how to make tools easier to call correctly.
- [Code execution with MCP (Anthropic)](https://www.anthropic.com/engineering/code-execution-with-mcp) - Giving agents controlled execution power through explicit, inspectable tool boundaries.
- [AGENTS.md](https://github.com/agentsmd/agents.md) - Open format for repo-local instructions that tell agents how to work inside a specific codebase.
- [AGENT.md](https://github.com/agentmd/agent.md) - Companion standardization effort for machine-readable agent instructions across tools.
- [Model Context Protocol (MCP)](https://github.com/modelcontextprotocol/modelcontextprotocol) - Core specification for MCP-based tool and context interoperability across runtimes.
- [MCP Servers (official)](https://github.com/modelcontextprotocol/servers) - Official collection of MCP server implementations across tools and domains.
- [GitHub Spec Kit](https://github.com/github/spec-kit) - GitHub's toolkit for spec-driven development, useful when agents execute against explicit product/engineering specs.
- [12-Factor AgentOps](https://www.12factoragentops.com/) - Operations companion to 12-Factor Agents focused on context discipline, validation, and reproducibility.
- [LangChain MCP Adapters](https://github.com/langchain-ai/langchain-mcp-adapters) - Adapters connecting LangChain components with MCP server ecosystems.

## Sandboxing & Execution

Agents that run code need isolated environments. Don't skip this.

- [Beyond permission prompts: sandboxing Claude Code (Anthropic)](https://www.anthropic.com/engineering/claude-code-sandboxing) - Reducing approval friction without losing control through better sandboxing and policy design.
- [Daytona](https://github.com/daytonaio/daytona) - Secure, elastic sandbox infrastructure with file, Git, LSP, and execution APIs for AI-generated code.
- [E2B](https://github.com/e2b-dev/E2B) - Secure cloud environments with real tools for production-grade agent code execution.
- [CUA](https://github.com/trycua/cua) - Infrastructure stack for computer-use agents with sandbox, SDK, and benchmark support.
- [SWE-ReX](https://github.com/SWE-agent/SWE-ReX) - Sandboxed code execution infrastructure for AI coding agents at local and cloud scale.
- [Tensorlake](https://github.com/tensorlakeai/tensorlake) - Serverless runtime for agent sandboxes with MicroVM isolation, snapshots, and suspend-resume.
- [Agent Sandbox (kubernetes-sigs)](https://github.com/kubernetes-sigs/agent-sandbox) - Kubernetes-native sandbox control plane for isolated, stateful agent runtimes.
- [Mitigating Prompt Injection in Software Agents (OpenHands)](https://openhands.dev/blog/mitigating-prompt-injection-attacks-in-software-agents) - Confirmation mode, analyzers, sandboxing, and hard policies for reducing prompt-injection risk.

## Orchestration & Frameworks

Multi-agent coordination, loop control, and workflow design.

- [How we built our multi-agent research system (Anthropic)](https://www.anthropic.com/engineering/multi-agent-research-system) - Architecture write-up: separation of roles, structured coordination, and harness design for research agents.
- [Agent Frameworks, Runtimes, and Harnesses, Oh My! (LangChain)](https://blog.langchain.com/agent-frameworks-runtimes-and-harnesses-oh-my/) - Clear decomposition of what belongs in a framework vs. a runtime vs. a harness.
- [LangGraph](https://github.com/langchain-ai/langgraph) - Graph-based runtime for resilient stateful agents with deterministic workflow control.
- [OpenAI Agents SDK](https://github.com/openai/openai-agents-python) - Lightweight framework for multi-agent workflows, handoffs, and production patterns.
- [deepagents](https://github.com/langchain-ai/deepagents) - LangChain's open-source harness for long-running, tool-using agents with middleware patterns.
- [AutoGen (Microsoft)](https://github.com/microsoft/autogen) - Multi-agent interaction framework with orchestration and conversation management.
- [PydanticAI](https://github.com/pydantic/pydantic-ai) - Type-safe Python framework for agents with strong schema contracts.
- [mcp-agent](https://github.com/lastmile-ai/mcp-agent) - Practical agent framework centered on MCP tool ecosystems and workflow composition.
- [AgentKit (Inngest)](https://github.com/inngest/agent-kit) - TypeScript toolkit for durable, workflow-aware agents on event-driven infrastructure.
- [Humans and Agents in Software Engineering Loops (Thoughtworks)](https://martinfowler.com/articles/exploring-gen-ai/humans-and-agents.html) - A clear mental model for where humans should strengthen the harness rather than micromanage artifacts.
- [Ralph Wiggum as a Software Engineer](https://ghuntley.com/ralph/) - The minimalist `while :; do cat PROMPT.md | claude-code; done` harness pattern: single-task loops, prompt stacking, bounded subagent parallelism.

## Evaluation & Benchmarks

You can't improve what you can't measure.

- [Demystifying Evals for AI Agents (Anthropic)](https://www.anthropic.com/engineering/demystifying-evals-for-ai-agents) - What to measure when agents have many possible trajectories to the same goal or failure.
- [Quantifying infrastructure noise in agentic coding evals (Anthropic)](https://www.anthropic.com/engineering/infrastructure-noise) - Runtime configuration alone can swing coding benchmark scores by more than many leaderboard gaps.
- [Evaluating Deep Agents: Our Learnings (LangChain)](https://blog.langchain.com/evaluating-deep-agents-our-learnings/) - Single-step, full-run, and multi-turn eval design for stateful agents.
- [Testing Agent Skills Systematically with Evals (OpenAI)](https://developers.openai.com/blog/eval-skills/) - Turning agent traces into repeatable evals with JSONL logs and deterministic checks.
- [Promptfoo](https://github.com/promptfoo/promptfoo) - Config-driven prompt/agent/RAG testing, comparison, and red-team evaluation tool. CI-friendly.
- [DeepEval](https://github.com/confident-ai/deepeval) - LLM evaluation framework supporting agent and workflow quality testing.
- [lm-evaluation-harness (EleutherAI)](https://github.com/EleutherAI/lm-evaluation-harness) - Canonical benchmark harness for consistent LLM evaluation across tasks.
- [SWE-bench](https://github.com/SWE-bench/SWE-bench) - Standard benchmark for software engineering agents working on real GitHub issues and tests.
- [Terminal-Bench](https://www.tbench.ai/) - Benchmark suite for terminal-native agents in shells, filesystems, and verification-heavy environments.
- [Harbor](https://github.com/harbor-framework/harbor) - Generalized harness for evaluating and improving agents at scale, released alongside Terminal-Bench 2.0.
- [OSWorld](https://os-world.github.io/) - Real computer-use benchmark with 369 tasks across Ubuntu, Windows, and macOS with execution-based evaluators.
- [WebArena](https://webarena.dev/) - Self-hostable web environment for evaluating autonomous agents on realistic tasks.
- [AgentBench](https://github.com/THUDM/AgentBench) - Cross-environment benchmark spanning OS, databases, web, and more for comparing harness generality.
- [HAL: Holistic Agent Leaderboard](https://hal.cs.princeton.edu/) - Leaderboard with attention to reliability and cost, not just task completion.
- [auto-harness](https://github.com/neosigmaai/auto-harness) - Benchmark-gated optimization loop that mines failures, edits agent code, and guards against regressions overnight.

## Observability & Reliability

If you can't see what your agent is doing, you can't debug or improve it.

- [Langfuse](https://github.com/langfuse/langfuse) - Open-source LLM engineering platform for traces, metrics, prompts, and evals. Self-hostable.
- [Opik](https://github.com/comet-ml/opik) - End-to-end debug/eval/monitoring stack for LLM apps and agent workflows.
- [Arize Phoenix](https://github.com/Arize-ai/phoenix) - Open platform for AI observability, tracing, and evaluation analytics.
- [TensorZero](https://github.com/tensorzero/tensorzero) - Open LLMOps stack unifying gateway, observability, evaluation, and optimization.
- [AgentOps SDK](https://github.com/AgentOps-AI/agentops) - Monitoring and benchmarking SDK for agent workflows with cost and trace tracking.
- [claude-code-reverse](https://github.com/Yuyz0112/claude-code-reverse) - Tooling to visualize and inspect Claude Code LLM interaction traces. Useful for debugging harness behavior.
- [Assessing internal quality while coding with an agent (Thoughtworks)](https://martinfowler.com/articles/exploring-gen-ai/ccmenu-quality.html) - Moving quality checks into the loop rather than relying on after-the-fact manual review.

## Guardrails & Security

Control what the agent can affect and where it can go.

- [LiteLLM](https://github.com/BerriAI/litellm) - Unified LLM gateway/proxy with cost tracking, load balancing, and guardrails.
- [Portkey Gateway](https://github.com/Portkey-AI/gateway) - AI gateway with routing and guardrails for multi-model production traffic.
- [ContextForge (IBM)](https://github.com/IBM/mcp-context-forge) - Registry and proxy unifying MCP, A2A, and REST/gRPC endpoints with centralized governance and observability.
- [AgentGateway](https://github.com/agentgateway/agentgateway) - Agentic proxy gateway for AI agents and MCP server ecosystems.
- [Anchoring AI to a reference application (Thoughtworks)](https://martinfowler.com/articles/exploring-gen-ai/anchoring-to-reference.html) - Constraining agents with concrete exemplars for more consistent, bounded output.
- [Claude Code: Best practices for agentic coding (Anthropic)](https://code.claude.com/docs) - Repo structure, checkpoints, validation, and safe delegation patterns for agentic coding.

## Reference Implementations

Real systems you can read, run, and learn from.

- [Claude Code](https://github.com/anthropics/claude-code) - Anthropic's official terminal coding agent. The canonical example of a production harness.
- [OpenHands](https://github.com/OpenHands/OpenHands) - Open-source AI software engineer for repo-level coding task execution. Well-documented harness design.
- [SWE-agent](https://github.com/SWE-agent/SWE-agent) - Research coding agent that makes the harness, prompts, tools, and environment design directly inspectable.
- [aider](https://github.com/Aider-AI/aider) - Terminal coding assistant with repo mapping, git-aware edits, and built-in lint/test feedback loops.
- [Citadel](https://github.com/SethGammon/Citadel) - Harness for Claude Code and Codex with isolated worktrees, multi-agent coordination, and persisted memory.
- [Harness Evolver](https://github.com/raphaelchristi/harness-evolver) - Claude Code plugin that autonomously evolves harnesses using multi-agent proposers and LangSmith-backed evaluation.
- [mini-swe-agent](https://github.com/SWE-agent/mini-swe-agent) - Minimal coding agent implementation. Good for understanding harness primitives without framework overhead.
- [mini-coding-agent](https://github.com/rasbt/mini-coding-agent) - Minimal harness illustrating approvals, memory, bounded delegation, and durable transcripts.
- [Building agents with the Claude Agent SDK (Anthropic)](https://claude.com/blog/building-agents-with-the-claude-agent-sdk) - Production-oriented SDK guide covering sessions, tools, and orchestration support.
- [learn-claude-code](https://github.com/shareAI-lab/learn-claude-code) - Hands-on harness tutorial for building Claude Code-like systems from scratch.

## Templates

Drop-in harness scaffolding for real projects.

These templates come from the `awesome-harness-engineering` project and are designed for agents working on codebases:

| Template | Purpose |
|---|---|
| `PLAN.md` | Task planning with milestones, scope boundaries, risks, and verification gates |
| `IMPLEMENT.md` | Append-only execution log capturing decisions, deviations, and open questions |
| `AGENTS.md` | Project-level agent instructions: permissions, tooling constraints, verification gates |
| `HARNESS_CHECKLIST.md` | Pre-deployment verification covering tool design, context, permissions, and safety |

See [walkinglabs/awesome-harness-engineering](https://github.com/walkinglabs/awesome-harness-engineering) for the full template set.

---

## Contributing

Contributions are welcome. A good entry:
- Addresses a **specific harness problem** (context, tools, sandboxing, evals, observability, guardrails)
- Is a primary source - engineering blog, paper, reference implementation, or working open-source project
- Has a one-sentence annotation explaining **why it matters**, not just what it is
- Is not redundant with an existing entry

Open a PR with your addition under the right section, including the annotation.

## License

[CC0 1.0](./LICENSE) - Public domain. Use freely.
