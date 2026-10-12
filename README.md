# AI systems portfolio

Kyle Burroughs — designing and directing the build of agent systems: platforms where agents from different model engines work as one team, tutors that check their own accuracy, and work toward models that create 3D assets and play games.

The source for these projects lives in private repositories. This repo is the public record of what each one is, how it is designed, and what has been measured. The claims come from the projects' own notes and test results, and each limit is stated next to the result it limits.

| Project | What it is | Status |
|---|---|---|
| [Apollo](apollo/) | A harness that supervises agent loops from many engines as one team | In development: contracts written and independently reviewed, core in Rust, UI in TypeScript/React |
| [Athena](athena/) | An adaptive learning agent that follows a learner and tests its own output | Version 0 built and tested; to be rewritten in Rust once its host is ready |
| [Reliability framework](governance/) | The rules that keep autonomous agents accountable and their work auditable | In daily use across the agents that build these projects |
| [Game agents](game-agents/) | Tooling and measurement for agents that mod and play games | Control benchmark complete (best configuration 4.3x faster than a fresh agent); mod workflow and vision-based play in progress |
| [Perseus](perseus/) | A learned generator for production-grade 3D hair geometry | Research and development: data and evaluation foundation built, learning trials ongoing |
| [Sketch and Juno](sketch-juno/) | Creative agents for structuring stories and writing branching dialogue | Design approved; app under construction |

## How I work

- **Design the contract first.** Each platform has a written contract before code, and the contract is reviewed by someone other than its author.
- **Measure, then decide.** Choices between models and designs are made on scored evaluations, such as blind judging for Athena and timed, ground-truth-checked runs for the game benchmark, and the limits of each evaluation are written down.
- **Keep the failures.** Failed experiments stay in the record with their numbers. A failure that is documented does not get repeated.

## Contact

[LinkedIn](https://www.linkedin.com/in/kyle-burroughs-152187297)

Content © Kyle Burroughs. All rights reserved.
