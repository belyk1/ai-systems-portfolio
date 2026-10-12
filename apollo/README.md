# Apollo

**Apollo supervises agent loops. It is not one.**

Apollo is a harness that runs many agents, built on different model engines, as a single team. Each engine brings its own native loop: Claude Code brings its loop, Codex brings its loop, and an engine with no loop gets an adapter built on an open one. Apollo defines what it needs from a loop, and anything that meets that contract can hold a seat in the team.

Status: in development. The engine and wire contracts are written and have been through repeated independent review. The core is being built in Rust, with a TypeScript/React interface.

## Why supervise instead of build a loop

Owning one general agent loop means building and maintaining the turn loop, every tool, permissions, context accounting, compaction, transcripts, prompt assembly, streaming, per-provider quirks and sub-agents. That is a build as large as the harness itself, with a cost that compounds forever and a result worse than each engine's native loop. It also creates a two-tier team, where the same role behaves differently depending on which engine runs it. A single roster exists to remove that problem, so a custom loop would bring it back one level down.

Supervision keeps every engine at its best and puts the shared behavior in one place.

## The contract a loop must meet

The engine contract has 14 requirements and 23 capabilities. Requirements include:

- A session that exists before any prompt, so queues, warm sessions and mid-turn injection work.
- Turns as discrete objects, with exactly one ending per turn, even when an engine dies mid-turn.
- Tool calls and permission decisions surfaced as events, with permission checked before execution and never on a timer.
- Context pressure with a named owner: the engine compacts itself, or it reports usage and accepts a compaction command.
- Interrupt, liveness and containment, usage and served-model identity, typed errors, and history read through the driver instead of from engine files.

A companion wire contract defines the three dozen or so frame types that carry all of this between the core and the interface.

## Capabilities are declared and tested

Engines are not equally capable, and that will not change. One has native goals, another has hooks and sub-agents, a third has whatever it was given. So each loop declares what it can do, Apollo shows that on the view, and the interface shows what a given agent can actually do. Each declaration is specified to be backed by a conformance test per loop, so the harness can fail a loop that claims something it does not do.

## Team machinery lives in Apollo

Anything the team depends on lives in Apollo and reaches every loop the same way: message delivery between agents, rules and instructions, skills, hooks, sub-agent spawning and tool servers. If those lived inside an engine, a role would break the moment its engine changed.

## Context policy

Agents are never cleared. Before an agent's context fills, the harness asks it to write down what it knows, then compacts it, and it compacts regardless if the agent does not respond in time. The only fresh start is a deliberate new session. The goal is long runs that survive interruption with their state intact.

## Designed, not yet built

A retrieval layer for the harness: durable memory of past work, project rules and codebase knowledge, with a source attached to every retrieved item. The aim is that an agent starts each task informed, and an auditor can see exactly what the agent relied on. This is in design only.

## What I did

I set the architecture, wrote the requirements and decisions that shaped the contracts, and directed the build in Rust, TypeScript and React.
