# Game agents

Two linked efforts: agents that build game mods, and agents that control a game through its interface. Both aim at the same long-term question, which is how an agent acts on a live environment it can only see.

Status: in progress. The efficiency benchmark below is complete. The mod-building workflow is under construction, and vision-based play is the direction under consideration.

## Mod-building agents

An agent workflow with custom tools, being built so that agents can create quests, items, characters, mechanics and effects for a commercial game, then test them inside the running game. The aim is for the same agents to build a mod and verify it in one loop.

## Control benchmark

To choose a model and a setup, I ran one nine-checkpoint in-game task ten times under different configurations. Each run was timed from the agent's first action to the end of the task and checked against ground truth, and the failed attempts are kept in the record.

| Configuration | Time | Tool calls |
|---|---|---|
| Fresh agent, no skills | 6:44 | 78 |
| Agent writing skills as it works | 8:11 | 78 |
| Agent with those skills, plus one correction from the judge | 3:32 | 37 |
| Smaller model, improved skills | 2:05 | 24 |
| Larger model, improved skills | 3:06 | 26 |
| Skills written by a different agent | 5:53 | 67 |
| Improved skills + a helper that handles waits and screen checks | 2:41 | 25 |
| Improved skills, helper offered but not used | 2:44 | 27 |
| Larger model + improved skills + co-pilot doing every stretch | 2:05 | 14 |
| Smaller model + improved skills + co-pilot doing every stretch | **1:33** | **8** |

All ten runs completed all nine checkpoints. The best configuration finished 4.3x faster than the fresh agent, with about a tenth of the tool calls.

What the runs showed:
- Skills the agent wrote for itself, plus one correction from the judge, took the same task from 8:11 to 3:32.
- A co-pilot that handles every routine stretch (waits, screen checks, input) cut the larger model from 3:06 to 2:05 and the smaller model from 2:05 to 1:33.
- One desktop-control tool treated the game's own cursor re-centring as the user moving the mouse and cancelled timed actions, so a different control path was needed. The failed attempts and their findings are kept.

**Limits:** one task, one scripted route, one run per configuration. One planned comparison (a second control tool) was not run.

## Direction: continuous vision with action chunking

Follow-up runs without a script (changing a setting, finding a save, reaching a location, shooting a target) did not succeed reliably. My conclusion is that free exploration needs continuous vision, not periodic screenshots. The idea under consideration is a high-frame-rate vision agent that plans in short chunks of actions and reacts to what it sees, so that agents can play games and, later, carry a reactive and predictive loop to real-world design and operation. Nothing in this direction is built yet.

## What I did

I designed the workflow and tooling, set up the benchmark and its judging checks, and drew the conclusions that set the direction.
