## Elite TypeScript Port Whitepaper

### Executive Summary

This project will deliver a faithful, playable, fully typed browser port of
Elite by using autonomous agents for most translation work and deterministic
verification for most correctness work, with `BBC Master 128 Elite
(Acornsoft)` as the primary reference build. The repository does not yet
contain the port itself. It contains the ingredients needed to build it: a
fully annotated 6502 source corpus, a large mirrored commentary corpus, and a
modern BBC Micro / BBC Master emulator-toolchain that serves as the reference
machine.

The practical consequence is that this is not a reverse-engineering project in
the usual sense. The source corpus names routines, explains variables, describes
algorithms in plain English, and documents the original build pipeline. The
agent reads intent, not opcodes.

### Goal

The goal is a browser-native version of Elite that can be played, tested, and
evolved without treating 6502 assembly as the runtime implementation. The
implementation path is to translate Elite's game logic into idiomatic
TypeScript while preserving observable behaviour.

The port must preserve:

- Simulation behaviour
- Data formats and important state transitions
- Deterministic outputs for computational systems
- The feel and presentation of the reference game, subject to human review

The port must improve:

- Type safety
- Readability
- Modularity
- Testability
- Replaceable platform adapters for rendering, input, audio, and persistence

### Operational Reference

The goal is "Elite in the browser". The concrete implementation
corpus currently present in this repository is the BBC Master edition of Elite,
located under `docs/elite-source-code-bbc-master` and `docs/bbcelite-master`.
The primary reference variant for this project is `BBC Master 128 Elite
(Acornsoft)`, corresponding to the official Acornsoft SNG47 release documented
in the corpus. This whitepaper defines that build as the operational reference
implementation for agent work in this repository.

If the project later broadens to the original BBC Micro disc or cassette
editions, the method remains the same but the authoritative corpus and fixture
set will need to change.

### Documentation Base

#### 1. Authoritative source corpus

`docs/elite-source-code-bbc-master` is the byte-level and build-level ground
truth. It contains:

- Annotated BeebAsm sources such as
  `1-source-files/main-sources/elite-source.asm`, `elite-data.asm`, and
  `elite-loader.asm`
- Build scripts in `2-build-files`
- Assembly outputs in `3-assembled-output`
- Reference binaries in `4-reference-binaries`
- Final disc images in `5-compiled-game-discs`

This corpus is valuable because it documents both behaviour and provenance. It
explains entry points such as `TT170`, names routines and variables, embeds
commentary directly in the source, describes variant handling, and explains how
byte-accurate builds were produced from the reconstructed assembly.

For the port, this corpus is the primary authority for:

- What the code does
- How state is laid out
- Which routines depend on which other routines
- Which behaviours are variant-specific
- Which parts of the original build exist only to support binary reproduction

It is not freely reusable source code. The repository explicitly omits an
open-source license and must be treated as a copyrighted reference corpus.

#### 2. Commentary and guides

`docs/bbcelite-master` is the agent-facing commentary layer. It is a generated
Markdown mirror of the BBC Master section of `elite.bbcelite.com` and includes:

- High-level orientation in `index.md`
- A navigable map in `articles/map_of_the_source_code.md`
- Dependency clues in `articles/source_code_cross-references.md`
- Effort and category breakdowns in `articles/source_code_statistics.md`
- Symbol indexes under `indexes/`
- Hundreds of per-subroutine, per-variable, and per-workspace pages under
  `main/`

This mirror turns the assembly into a searchable documentation set. Routine
pages expose names, categories, summaries, context links, references, and in
many cases links to deeper explanatory articles. For autonomous agents, this is
the difference between translation from intent and blind transcription from raw
assembler.

#### 3. Live-site commentary not fully mirrored

The local mirror is strong but not complete. Some long-form material, especially
terminology pages and many "deep dive" articles, still live on the website and
are linked from the mirrored pages rather than fully vendored into this
repository. The local mirror is the default working set, and the live site is
the secondary source for conceptual gaps.

### Tooling and Reference Machine

`tools/jsbeeb` provides the modern tooling base. It is a JavaScript BBC Micro /
BBC Master emulator with an existing build, test, and automation story. For the
port project, it matters less as an end-user emulator and more as a
controllable oracle.

The `tools` directory also contains `tools/elite-compendium-bbc-master.dsd`, a
supplemental disc image artifact available alongside `jsbeeb` for emulator
boot, exploratory comparison, and manual play. It is useful context, but it is
not the canonical reference variant for the first port.

Its value falls into four categories:

- Execution: it can boot ROMs and disc images for the target machine and run the
  reference build
- Instrumentation: it can step by cycles, inspect memory and registers, and
  stop at addresses or breakpoints
- Capture: it can emit screenshots, text output, snapshots, and other stable
  artifacts
- Verification: it already includes CPU, timing, integration, and
  pixel-regression tests

The most important in-repo interface is `tools/jsbeeb/src/machine-session.js`.
It exposes a headless session API with booting, disc loading, cycle stepping,
memory reads and writes, execute/read/write breakpoints, captured VDU output,
and PNG screenshots. That API is the natural basis for a fixture-generation
harness.

Additional supporting capabilities already exist:

- Snapshot and restore support for deterministic setup and replay
- URL patching and scripted input for controlled experiments
- Visual comparison infrastructure via pixelmatch-based tests
- Separate CPU and timing test coverage that reinforces trust in the emulator as
  a reference platform

This means the project does not need to invent its own reference machine. The
port is built around a known emulator substrate and uses that substrate to
generate expected outputs.

### Port Architecture

The port separates simulation from platform concerns.

#### Pure computational core

Large parts of Elite are pure state transformation and belong in deterministic
TypeScript modules. This includes:

- Numeric helpers and fixed-width arithmetic
- Random number generation
- Galaxy and system generation
- Ship data handling
- Movement and geometry
- Combat tactics
- Market calculations
- Mission and commander-state logic
- Text/token expansion where it is data-driven rather than device-driven

These modules must accept explicit inputs, return explicit outputs, and
preserve 8-bit and 16-bit semantics where the original logic depends on
overflow, sign representation, carry behaviour, or bit layout.

#### Platform adapters

The remaining platform-bound systems sit behind interfaces:

- Rendering
- Keyboard and joystick input
- Audio
- Storage / save persistence
- Browser shell and UI

Canvas2D is the first practical rendering adapter. Clean interfaces allow
alternative adapters such as WebGL, WebGPU, or Three.js-backed renderers to
follow without rewriting the game logic.

#### Typed data model

The original code stores meaning inside workspaces, zero page, and compact ship
blocks. The TypeScript port must expose these as explicit typed structures while
preserving their original semantics. In other words, the port removes
accidental opacity, not behavioural precision.

### Verification Strategy

Verification is split by the nature of the system under test.

#### Automated deterministic verification

Computational modules are proven against the `BBC Master 128 Elite
(Acornsoft)` reference build by extracting known-good outputs from `jsbeeb` and
checking them into the port as fixtures. Examples include:

- RNG sequences
- Galaxy and system seeds
- Market prices and availability
- Ship movement updates
- Geometry and projection helpers
- Combat-state transitions
- Text token expansion
- Commander save/load transforms

For timing-sensitive or stateful subsystems, harnesses capture machine state at
controlled points using snapshots, breakpoints, and memory reads rather than
relying on ad hoc observation.

#### Visual and rendering verification

Where the output is stable enough, visual subsystems use screenshot-based or
pixel-diff-based regression tests. This is useful for bounded scenes,
dashboards, loading screens, or isolated drawing routines.

#### Human verification

Not everything is automated. The starfield can be technically correct and still
feel wrong. Docking can be mechanically close and still play badly.
Input latency, animation cadence, audio feel, and the overall sense of the game
must be checked by humans playing the port against the reference build.

Agents can verify bytes, states, and screenshots. Humans verify experience.

### Dependency-Ordered Delivery Plan

The port follows the shape of the documentation rather than the romance of the
game loop. Foundational systems first, dependent systems next, presentation
last.

1. Foundation
   - Fixed-width numeric utilities
   - Bit/byte helpers
   - Lookup tables
   - Core type definitions
   - Workspace and ship-block representations
2. Deterministic world data
   - RNG
   - Galaxy and system generation
   - Ship data tables
   - Text and token data plumbing
3. Simulation core
   - Movement
   - Geometry and projection helpers
   - Docking and collision checks
   - Universe bookkeeping
   - Combat tactics
4. Orchestration
   - Main flight loop
   - Main game loop
   - Missions
   - Economy, equipment, and commander state
5. Adapters and presentation
   - Rendering
   - Input
   - Audio
   - Browser UI and persistence
6. Product validation
   - Side-by-side playtesting
   - Regression capture expansion
   - Documentation updates
   - Packaging and browser delivery

Each iteration follows the same loop: read the documentation, port one coherent
unit, capture or extend fixtures, verify, and only then move outward to
dependents.

### Suggested Documentation Roles During Porting

The repository already contains a natural division of labour between documents.

Use `docs/elite-source-code-bbc-master` for:

- Ground-truth routine logic
- Build and variant behaviour
- Entry points and memory layout
- Byte-level caveats such as workspace noise and checksum rules

Use `docs/bbcelite-master` for:

- Discovering relevant routines and variables
- Understanding categories and subsystem boundaries
- Following call/reference chains
- Reading plain-English summaries before touching assembly
- Prioritising work by source-code map and statistics

Use `tools/jsbeeb` for:

- Fixture extraction
- Runtime probing
- Visual capture
- End-to-end regression
- Human comparison against the live reference

### Scope Decisions and Non-Goals

The port distinguishes between what must be preserved for gameplay fidelity and
what only existed to satisfy the original delivery medium.

Preserve:

- Game behaviour
- Data formats where they affect play or saves
- Reference variant behaviour
- Major audiovisual characteristics

Do not port literally unless fidelity requires it:

- Loader copy protection
- Disc boot choreography
- Encryption steps
- Workspace-noise tricks used to reproduce original binaries
- Other build artifacts whose purpose is byte-identical 6502 output rather than
  browser gameplay

Variant scope must also be explicit. For the first port, the primary reference
variant is `BBC Master 128 Elite (Acornsoft)`, i.e. the official Acornsoft
SNG47 release documented in the local corpus. Other BBC Master variants are
later feature flags unless there is a strong reason to port them from day one.

### Risks and Constraints

- The documentation corpus is rich, but some high-value conceptual material
  still lives only on the website.
- The authoritative source corpus is copyrighted reference material, not a
  permissively licensed code drop.
- The emulator tooling is strong, but a port-specific fixture pipeline still
  needs to be built on top of it.
- Visual fidelity and input feel remain partly human problems.
- The repository currently contains documentation and tooling, not a TypeScript
  scaffold. Early work will therefore involve creating both the port codebase
  and the extraction harnesses that validate it.

### End State

The end state is a playable, faithful, fully typed Elite running in the
browser, implemented primarily by autonomous agents, checked continuously
against deterministic evidence from the reference build, and accepted against a
human-verified standard of fidelity.

The project succeeds when the reference assembly corpus is no longer the only
executable truth, but remains the standard against which the new
implementation is judged.
