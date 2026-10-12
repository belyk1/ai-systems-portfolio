# Athena

An adaptive learning agent. It teaches one concept at a time from cited sources, checks the learner without help, records what it observed, and brings the concept back later. The engine knows nothing about any subject: a subject arrives as data.

Status: version 0 is built and tested (488 automated tests as of 2026-10-08). It is being rewritten in Rust once its host application is ready.

## Design

- **The learner's record is an append-only event log.** State is recomputed from the log by a deterministic rule and never edited, so every statement about what a learner knows can be traced to what was observed.
- **Four states per concept, fixed review intervals.** The planner schedules due reviews first, then the next concept. Prerequisites are advice and never a block, and a learner can say they already know something, which changes nothing until a question confirms it.
- **Subjects are data.** A subject pack is a concept graph, a goal and the source text lessons are written from. The same engine, with no subject-specific code, runs every pack. A test fails if the engine ever names a vendor, a subject or a person.
- **Every lesson claim cites a passage.** Code checks each citation, and any quoted excerpt must appear verbatim in the source. An excerpt the model invented is dropped, never shown.
- **Every drafted question goes through checks before a learner sees it.** A checker model works the question out blind, judges that answer, and reviews the grading rubric. The checks are imperfect, as the evaluation below shows.
- **Visuals are drawn in a sealed frame** that can load nothing from the network and cannot touch the page around it. The frame was independently reviewed after it was built, and the fixes that review required were made.
- **One local tool at a time on the graphics card.** A resource guard lets a local model and a local image tool take turns instead of crawling together.

## Evaluation: who should write, and who should check

The design question: should one local model do everything, or should a stronger model write while the local model checks and grades? Blind evaluation on a 14-concept Python subject, two rounds, judged by an independent stronger model that was not told which design wrote what.

| | Local writer, round 1 | Strong writer, round 1 | Local writer, round 2 | Strong writer, round 2 |
|---|---|---|---|---|
| Lesson claims supported by their cited source | 42 / 49 | 77 / 77 | 73 / 89 | 77 / 77 |
| First-draft questions judged sound | 4 / 14 | 13 / 14 | 1 / 14 | 12 / 14 |

Round 2 used one more judge check than round 1, so the question rows are not strictly comparable across rounds. Within a round, both designs were judged together.

The stronger writer produced fully supported lessons and mostly sound questions. The local writer's lessons were mostly right, but most of its questions were not. A model that checks a draft it did not write catches mistakes the writer cannot see in its own work. The decision that followed: a stronger model writes, a local model checks and grades, and the learner's answers never leave their machine.

The first round also exposed two faults in the built-in checks. The blind pass had been told to answer using only what the passages state, so it refused every question that needed working out. And nothing caught a grading criterion that the question's own wording already answered. Both were fixed and re-measured on fresh drafts. The checker remains usable, not strong: in round 2, a few unsound questions still passed it and some sound ones were refused.

**Limits, stated plainly:**
- One subject and 14 concepts.
- The stronger writer was a stand-in: a stronger model answered the exact requests from a file, instead of being called through the adapter.
- The stand-in writer and the judges come from the same model family, so a shared blind spot would not show.
- One judge per round, and only application questions were tested.
- Some unsound questions would still have reached a learner in each design.

## Not in version 0

Reaching beyond the loaded sources into adjacent material to widen a learner's understanding (an intended feature), hints, misconception notes, charts and concept maps as pictures, and generated or fetched images.

## What I did

I designed Athena, directed the build, ran the evaluations and made the decisions the results supported.
