# Perseus

A learned generator for production-grade 3D hair geometry, with precise and reversible editing and an editable Blender master.

Status: research and development. The data, evaluation and editing foundations are built and verified. No generated hairstyle has yet been accepted as professional quality, and the project's own notes say so.

## What is built

- **A lossless source bank.** 707 source assets held exactly: 30.2 million vertices, 40.4 million ordered faces and 786,816 components. Splits are isolated by asset family, and the test set stays sealed. The source assets stay outside version control and are used for research evaluation only.
- **Fixed evaluation gates.** Each learning trial is run against pass/fail gates set in advance: useful geometry, shape inspection and distance checks. A formula-supplied improvement is not counted as a learned improvement.
- **Precise, reversible editing.** Edits change only the selected vertices. In a tested case, 22 selected vertices changed while 18,688 protected vertices and every face stayed exactly the same, with undo and redo working across fresh sessions.
- **Blender round trips.** In a controlled test case, save and reopen preserved every attribute the pipeline carries, with ownership and stale-data refusals tested. Generated UV, skin and material data are not part of this yet.
- **An experimental native-format carrier.** A generated and edited candidate was written into a template, compiled and read back independently, with every position and face matching. Shading, bounds, skin deformation, materials and in-game appearance are not yet qualified.

## What the learning trials have established

Several model families have been trained and measured against the gates and closed as failures: a compact autoencoder, a multiplicative decoder, graph- and image-conditioned position models, a UV model, a direct field model, a marginal-residual model, and ordered-metric and local-patch models. Their results and failed outputs are kept, and the training is not extended or replayed. The current candidate generates coarse anchor geometry before finer geometry, using topology relationships to place and shape what follows. It has not yet passed the learning gate.

The documented record of these trials is what makes each next one faster: it shows what not to repeat, and the gates mean progress is measured instead of declared.

## Still required

Coherent original shape and topology, conditioning on a selected design and a head, fitting, materials, skinning and deformation, level-of-detail variants, and artistic acceptance in the target engine.

## What I did

I set the product contract, the data and evaluation design and the roadmap, and directed the build and the experiments.
