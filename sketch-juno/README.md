# Sketch and Juno

Creative agents for narrative and workflow.

Status: Sketch is a project tool for structuring a story. Juno's design is approved and the application is under construction.

## Juno

A script-writing application where one person writes a branching script line by line. Juno offers options, and the writer keeps, edits or replaces them.

Design decisions that shape it:
- **The writer keeps the final say.** The agent proposes, and nothing enters the script without the writer's choice.
- **Each request is built fresh for one speaker,** carrying only that speaker's character breakdown. That keeps every character consistent and keeps context small.
- **The model is designed to be swappable.** One provider interface sits between the application and the model. One provider is connected so far, with a second planned.
- **Branching rules are data.** Conditions on choices (attribute requirements, background, relationships, hidden-until) are data, and Juno can propose new condition kinds for the writer to approve.
- **Juno reads a Sketch project (outline, beats, cast) and never writes into it.**

## What I did

I designed the application and its interaction model and directed the build.
