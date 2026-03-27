# PRD: Elite TypeScript Browser Port

**Source Document:** `docs/elite-typescript-port-step-by-step.md`

## 1. Introduction/Overview

This PRD defines the work required to build a browser-native TypeScript port of
`BBC Master 128 Elite (Acornsoft)` in this repository from scratch without
treating the 6502 assembly as the runtime implementation.

The port code does not exist yet. The work includes defining where new code
should live, how it should be verified, and the exact macro-order in which the
port should be built.

The target result is a playable browser version of
`BBC Master 128 Elite (Acornsoft)` that:

- preserves gameplay behaviour, important data formats, and deterministic
  computational outputs
- uses TypeScript modules instead of 6502 assembly as the runtime
- verifies computational correctness against the reference build via `jsbeeb`
- verifies audiovisual fidelity with screenshots, controlled scenarios, and
  human playtesting

## 2. Goals

- Ship a playable browser version of `BBC Master 128 Elite (Acornsoft)`.
- Preserve gameplay behaviour, important data formats, and deterministic
  computational outputs.
- Use TypeScript modules instead of 6502 assembly as the runtime.
- Verify computational correctness against the reference build via `jsbeeb`.
- Verify audiovisual fidelity with screenshots, controlled scenarios, and
  human playtesting.
- Document exactly what "correct" means before implementation work starts.
- Build fixtures before or alongside each subsystem instead of after the fact.
- Keep the final codebase typed, modular, and understandable without reading
  assembly.
- Make the project buildable, testable, and runnable by an engineer other than
  the original implementer.

## 3. User Stories

### US-001: Freeze The Reference Contract
**Description:** As a port engineer, I want the reference variant, corpus, and
phase-1 non-goals written down so that implementation starts from a single
correctness target.

**Acceptance Criteria:**
- [ ] `docs/elite-typescript-port-step-by-step.md` is created and
      maintained as the runbook.
- [ ] `docs/tasks/reference-contract.md` records the canonical machine, the
      canonical game disc, the canonical reference corpus, the non-canonical
      exploratory artifact, and the explicit phase-1 non-goals.
- [ ] The canonical machine is `BBC Master 128 (DFS)` in `jsbeeb`.
- [ ] No implementation work starts until the reference variant and non-goals
      are written down.
- [ ] Typecheck/lint passes.

### US-002: Create The TypeScript And Browser Scaffold
**Description:** As a port engineer, I want a stable TypeScript browser shell
and test layout so that runtime code, harness code, and fixtures have clear
homes before subsystem work begins.

**Acceptance Criteria:**
- [ ] Root `package.json`, `tsconfig.json`, `vite.config.ts`,
      `vitest.config.ts`, `index.html`, `src/app/browser-entry.ts`,
      `src/app/game-shell.ts`, `src/ui/`, and `tests/` are created.
- [ ] The new port uses TypeScript everywhere, defines path aliases for
      `src/core`, `src/platform`, and `src/ui`, and keeps emulator harness code
      outside the browser bundle where possible.
- [ ] Scripts exist for `dev`, `build`, `test`, `test:reference`, and
      `test:visual`.
- [ ] The local Node version aligns with `tools/jsbeeb/package.json` unless
      there is a deliberate reason to split the toolchains.
- [ ] A browser app starts, a test runner starts, and the repository has stable
      places for runtime code, harness code, and fixtures.
- [ ] Typecheck/lint passes.

### US-003: Build The Reference Harness On Top Of jsbeeb
**Description:** As a port engineer, I want a deterministic reference harness
so that the TypeScript port has a deterministic oracle for fixture extraction
and can compare evidence against the reference build.

**Acceptance Criteria:**
- [ ] `scripts/reference/session.ts`, `scripts/reference/boot-reference.ts`,
      `scripts/reference/snapshot.ts`, `scripts/reference/breakpoints.ts`, and
      `tests/reference/reference-harness.test.ts` are created.
- [ ] `scripts/reference/boot-reference.ts` boots the SNG47 disc and confirms
      the session contract.
- [ ] The harness exposes booting the reference disc, run-for-cycle and
      run-until-address helpers, memory read/write helpers, key input helpers,
      text capture helpers, screenshot capture helpers, and snapshot/restore
      helpers.
- [ ] The harness can boot the SNG47 disc, take a screenshot, capture text
      output, stop on breakpoints, read memory, write memory, and restore from
      a snapshot.
- [ ] Typecheck/lint passes.

### US-004: Define Fixture Conventions Before Subsystem Ports
**Description:** As a port engineer, I want standard fixture conventions so
that every later subsystem can prove correctness the same way.

**Acceptance Criteria:**
- [ ] `tests/fixtures/reference/manifest.json`,
      `tests/fixtures/reference/README.md`, `tests/fixtures/visual/README.md`,
      `scripts/fixtures/extract-fixture.ts`, and
      `scripts/fixtures/capture-scene.ts` are created.
- [ ] Fixture locations are defined for `rng`, `galaxy`, `market`, `text`,
      `movement`, `tactics`, `commander`, `screens`, and `scenes`.
- [ ] Every fixture record includes the reference disc and model, source
      routine or routines, setup method, exact input bytes or state values,
      exact expected output bytes, values, or image, and the capture script
      used to produce it.
- [ ] No core subsystem is implemented without at least one extraction path and
      one fixture location already defined.
- [ ] Typecheck/lint passes.

### US-005: Implement Fixed-Width Arithmetic And Bit Semantics
**Description:** As a port engineer, I want fixed-width arithmetic helpers so
that JavaScript number semantics do not leak into gameplay logic.

**Acceptance Criteria:**
- [ ] `src/core/arithmetic/u8.ts`, `src/core/arithmetic/u16.ts`,
      `src/core/arithmetic/bitwise.ts`,
      `src/core/arithmetic/sign-magnitude.ts`,
      `src/core/arithmetic/fixed-width.ts`, and
      `tests/unit/arithmetic/*.test.ts` are created.
- [ ] The arithmetic layer handles masking to 8 and 16 bits, carry and borrow
      helpers, signed and unsigned interpretations, rotate and shift behaviour,
      sign-magnitude helpers for coordinate storage, and small readability
      wrappers for simulation code.
- [ ] Tests cover wraparound, carry, sign-magnitude decode/encode, and any
      other byte-level behaviour depended on by RNG, movement, combat, and
      saves.
- [ ] Typecheck/lint passes.

### US-006: Model Workspaces, Ship Blocks, And Commander Data Explicitly
**Description:** As a port engineer, I want typed state structures so that the
runtime preserves original semantics without using opaque memory blobs as the
main API.

**Acceptance Criteria:**
- [ ] `src/core/state/workspaces/zp.ts`, `src/core/state/workspaces/wp.ts`,
      `src/core/state/workspaces/up.ts`, `src/core/state/ships/k-block.ts`,
      `src/core/state/ships/inwk.ts`,
      `src/core/state/commander/commander-state.ts`,
      `src/core/state/serialization/`, and `tests/unit/state/*.test.ts` are
      created.
- [ ] State objects expose named fields and typed records while also supporting
      lossless conversion to and from byte layouts where fixtures and save data
      require it.
- [ ] Serializer and deserializer tests round-trip the state structures
      cleanly.
- [ ] The state structures can represent all bytes needed by the first ported
      subsystems.
- [ ] Typecheck/lint passes.

### US-007: Port Static Game Data And Lookup Tables
**Description:** As a port engineer, I want typed gameplay data modules so that
the browser port can use reference data without depending on assembler-era
runtime structures.

**Acceptance Criteria:**
- [ ] `src/core/data/ship-blueprints.ts`, `src/core/data/markets.ts`,
      `src/core/data/tokens.ts`, `src/core/data/system-name-tokens.ts`,
      `src/core/data/dashboard.ts`, `src/core/data/lookup-tables.ts`, and
      `src/core/assets/` are created.
- [ ] Ship blueprints, tokens, market tables, fonts, and other gameplay data
      exist as typed assets or data modules.
- [ ] The runtime represents the data needed for behaviour and presentation but
      does not copy the annotated prose from the reference corpus into the
      runtime codebase.
- [ ] Tests confirm that ship blueprint data, token tables, and market tables
      match extracted reference fixtures.
- [ ] Typecheck/lint passes.

### US-008: Port RNG And Deterministic World Generation
**Description:** As a port engineer, I want exact world-generation logic so
that galaxy seeds, system generation, names, and markets reproduce reference
outputs exactly.

**Acceptance Criteria:**
- [ ] `src/core/world/rng.ts`, `src/core/world/galaxy/seed.ts`,
      `src/core/world/galaxy/system-generation.ts`,
      `src/core/world/galaxy/system-names.ts`,
      `src/core/world/market/market-generation.ts`,
      `tests/reference/rng.test.ts`, `tests/reference/galaxy.test.ts`, and
      `tests/reference/market.test.ts` are created.
- [ ] The implementation uses the listed local docs and only uses the listed
      online deep dives when the local mirror is not enough.
- [ ] Known galaxy seeds, system names, descriptions, prices, and
      availabilities match reference fixtures exactly.
- [ ] Typecheck/lint passes.

### US-009: Port Geometry, Projection, And Low-Level Presentation Primitives
**Description:** As a port engineer, I want deterministic math and drawing
primitives so that later ship, planet, sun, scanner, and dashboard renderers
can depend on stable reference-matched behaviour.

**Acceptance Criteria:**
- [ ] `src/core/presentation/vector.ts`,
      `src/core/presentation/arctan.ts`,
      `src/core/presentation/projection.ts`,
      `src/core/presentation/line-rasterizer.ts`,
      `src/core/presentation/pixel-plotter.ts`,
      `src/core/presentation/circle.ts`,
      `tests/reference/projection.test.ts`, and
      `tests/reference/rasterizer.test.ts` are created.
- [ ] Projection helpers and drawing primitives match reference outputs in
      isolated tests before any full-scene renderer is written.
- [ ] The implementation uses the listed local rendering and geometry docs as
      the first read.
- [ ] Typecheck/lint passes.

### US-010: Port Local-Universe Data Structures And Ship Movement
**Description:** As a port engineer, I want local-universe state and ship
movement logic so that the runtime can maintain the local bubble with reference
semantics.

**Acceptance Criteria:**
- [ ] `src/core/simulation/universe/universe-state.ts`,
      `src/core/simulation/universe/local-bubble.ts`,
      `src/core/simulation/ships/spawn.ts`,
      `src/core/simulation/ships/move-ship.ts`,
      `src/core/simulation/ships/orientation.ts`,
      `src/core/simulation/scanner/scanner-state.ts`,
      `tests/reference/movement.test.ts`, and
      `tests/reference/local-bubble.test.ts` are created.
- [ ] The implementation uses the listed local docs and the listed online deep
      dives for conceptual gaps only.
- [ ] Ship spawn, update, and orientation transforms match the reference
      harness for captured scenarios.
- [ ] Typecheck/lint passes.

### US-011: Port Tactics, Combat, Collision, Docking, And Scooping
**Description:** As a port engineer, I want Elite’s immediate interaction and
decision rules ported faithfully so that spaceflight behaviour feels like Elite
rather than a loose approximation.

**Acceptance Criteria:**
- [ ] `src/core/simulation/tactics/tactics.ts`,
      `src/core/simulation/combat/laser.ts`,
      `src/core/simulation/combat/missiles.ts`,
      `src/core/simulation/combat/collision.ts`,
      `src/core/simulation/docking/docking-checks.ts`,
      `src/core/simulation/docking/docking-computer.ts`,
      `src/core/simulation/cargo/scooping.ts`,
      `tests/reference/tactics.test.ts`,
      `tests/reference/combat.test.ts`, and
      `tests/reference/docking.test.ts` are created.
- [ ] The implementation uses the listed local docs and the listed online deep
      dives for conceptual gaps only.
- [ ] Captured combat and docking scenarios produce the same state transitions,
      targeting outcomes, and collision results as the reference machine.
- [ ] Typecheck/lint passes.

### US-012: Port The Main Flight Loop
**Description:** As a port engineer, I want a readable TypeScript flight loop
so that the port replaces the reference orchestration path without losing
observable behaviour.

**Acceptance Criteria:**
- [ ] `src/core/simulation/flight/main-flight-loop.ts`,
      `src/core/simulation/flight/flight-context.ts`,
      `src/core/simulation/flight/flight-step.ts`, and
      `tests/reference/main-flight-loop.test.ts` are created.
- [ ] The TypeScript flight loop is readable and modular, preserves ordering
      and observable effects, and does not preserve the original control-flow
      shape when a clearer structure produces identical behaviour.
- [ ] Stepped flight-loop captures from the port and `jsbeeb` agree for curated
      scenarios covering combat, docking, planet approach, and ordinary travel.
- [ ] Typecheck/lint passes.

### US-013: Port The Main Game Loop And Docked Flows
**Description:** As a port engineer, I want the top-level gameplay loop ported
so that the browser version can transition between in-flight and docked flows
spawn game events, and continue in the same order as the reference build.

**Acceptance Criteria:**
- [ ] `src/core/simulation/game/main-game-loop.ts`,
      `src/core/simulation/game/scheduler.ts`,
      `src/core/simulation/game/spawn-rules.ts`,
      `src/core/simulation/game/view-state.ts`, and
      `tests/reference/main-game-loop.test.ts` are created.
- [ ] The top-level loop can enter play, transition to docked flows, and
      spawn game events and continue event scheduling in the same order as the
      reference build.
- [ ] The implementation uses the listed local docs and the listed online deep
      dives for conceptual gaps only.
- [ ] Typecheck/lint passes.

### US-014: Port Economy, Equipment, Hyperspace, Missions, And Commander State
**Description:** As a port engineer, I want non-flight gameplay systems ported
so that the browser version becomes fully playable.

**Acceptance Criteria:**
- [ ] `src/core/simulation/economy/trade.ts`,
      `src/core/simulation/economy/equipment.ts`,
      `src/core/simulation/economy/status.ts`,
      `src/core/simulation/navigation/galaxy-map.ts`,
      `src/core/simulation/navigation/hyperspace.ts`,
      `src/core/simulation/missions/mission-state.ts`,
      `src/core/simulation/commander/save-format.ts`,
      `src/core/simulation/commander/save-load.ts`,
      `tests/reference/economy.test.ts`,
      `tests/reference/commander.test.ts`, and
      `tests/reference/mission.test.ts` are created.
- [ ] The player can trade, equip, hyperspace, progress missions, and save or
      load a commander while matching captured reference data.
- [ ] The implementation uses the listed local docs as the first read.
- [ ] Typecheck/lint passes.

### US-015: Port Text Systems And UI-Facing Token Expansion
**Description:** As a port engineer, I want data-driven TypeScript text systems
so that generated and scripted text no longer depends on screen-memory-era
printing logic.

**Acceptance Criteria:**
- [ ] `src/core/presentation/text/token-expansion.ts`,
      `src/core/presentation/text/goat-soup.ts`,
      `src/core/presentation/text/formatting.ts`, and
      `tests/reference/text.test.ts` are created.
- [ ] System names, market text, mission text, and generated descriptions match
      the reference outputs for a representative fixture set.
- [ ] The implementation uses the listed local docs and the listed online deep
      dives for conceptual gaps only.
- [ ] Typecheck/lint passes.

### US-016: Implement Rendering Adapters And Scene Renderers
**Description:** As a port engineer, I want browser rendering adapters and
scene renderers so that the game can draw in the browser without reusing the
emulator video pipeline.

**Acceptance Criteria:**
- [ ] `src/platform/render/canvas2d/canvas-renderer.ts`,
      `src/platform/render/canvas2d/framebuffer.ts`,
      `src/core/presentation/renderers/ship-renderer.ts`,
      `src/core/presentation/renderers/planet-renderer.ts`,
      `src/core/presentation/renderers/sun-renderer.ts`,
      `src/core/presentation/renderers/starfield-renderer.ts`,
      `src/core/presentation/renderers/scanner-renderer.ts`,
      `src/core/presentation/renderers/dashboard-renderer.ts`,
      `src/core/presentation/renderers/sights-renderer.ts`, and
      `tests/visual/*.test.ts` are created.
- [ ] Canvas2D is the first adapter, and the renderer boundary stays clean
      enough for a future WebGL or WebGPU renderer to replace it.
- [ ] Bounded scenes produce stable screenshots that pass pixel or perceptual
      regression thresholds against captured reference images.
- [ ] Typecheck/lint passes.
- [ ] Verify in browser using dev-browser skill.

### US-017: Implement Browser Input, Audio, Storage, And Shell Integration
**Description:** As a port engineer, I want browser platform adapters and shell
integration so that the browser build is actually playable.

**Acceptance Criteria:**
- [ ] `src/platform/browser/input/keyboard.ts`,
      `src/platform/browser/input/joystick.ts`,
      `src/platform/browser/audio/audio-engine.ts`,
      `src/platform/browser/storage/commander-storage.ts`,
      `src/app/game-shell.ts`, `src/ui/screens/*.ts`, `src/ui/hud/*.ts`, and
      `tests/integration/browser-shell.test.ts` are created.
- [ ] Keyboard control, optional joystick support, audio cues, save
      persistence, and screen transitions all work in the browser build.
- [ ] The implementation uses the listed local docs as the first read.
- [ ] Typecheck/lint passes.
- [ ] Verify in browser using dev-browser skill.

### US-018: Integrate Startup, Title Flow, And User-Facing Product Behaviour
**Description:** As a port engineer, I want startup and title-flow integration
so that the browser build behaves like a product rather than a set of
disconnected subsystems.

**Acceptance Criteria:**
- [ ] `src/app/bootstrap.ts`, `src/ui/screens/title-screen.ts`,
      `src/ui/screens/loading-screen.ts`,
      `src/ui/screens/flight-screen.ts`,
      `src/ui/screens/docked-screen.ts`, and
      `tests/integration/startup-flow.test.ts` are created.
- [ ] The title-screen and hangar feel are preserved, but disc-loader
      choreography and copy-protection mechanisms that existed only for the
      original delivery medium are not reproduced.
- [ ] The browser build boots to a usable title flow, enters gameplay, and
      returns to docked and title paths cleanly.
- [ ] Typecheck/lint passes.
- [ ] Verify in browser using dev-browser skill.

### US-019: Expand Automated Evidence And Perform Side-By-Side Playtesting
**Description:** As a port engineer, I want automated evidence and human
playtesting to expand together so that the port is proven mechanically and
experientially.

**Acceptance Criteria:**
- [ ] `tests/reference/acceptance/`, `tests/visual/acceptance/`,
      `docs/tasks/playtest-checklist.md`, and
      `docs/tasks/known-fidelity-gaps.md` are created and maintained.
- [ ] Automated checks cover deterministic fixtures for core simulation, visual
      regression scenes for dashboard, scanner, ships, sun, planet, and
      title/loading screens, save/load round trips, and end-to-end flow tests
      for launch, docking, trade, hyperspace, combat, death, and save/load.
- [ ] Human checks cover input feel, movement feel, docking feel, combat feel,
      audio feel, and general pacing and readability.
- [ ] Every known fidelity gap is either fixed, intentionally accepted, or
      recorded with a clear follow-up.
- [ ] Typecheck/lint passes.

### US-020: Package, Document, And Ship The Browser Port
**Description:** As a port engineer, I want packaging, documentation, and CI
artifacts so that another engineer can build, test, and understand the browser
port.

**Acceptance Criteria:**
- [ ] Root `README.md`, `docs/tasks/release-checklist.md`, browser build output
      configuration, and CI scripts for tests and builds are created or
      updated.
- [ ] The project provides one command for local development, one command for
      the full test suite, one command for reference extraction refresh, and
      one command for production build.
- [ ] Documentation explains the reference corpus, legal constraints, and how
      the verification harness works.
- [ ] Another engineer can clone the repository, install dependencies, run the
      browser build, run the tests, and understand the relationship between the
      port and the reference corpus.
- [ ] Typecheck/lint passes.

## 4. Functional Requirements

### FR-1: Reference Scope And Operational Rules

- **FR-1.1** The first implementation pass must treat
  `BBC Master 128 Elite (Acornsoft)` as the only reference variant.
- **FR-1.2** The implementation must use the local source and commentary corpus
  before reading raw assembly.
- **FR-1.3** Fixtures must be built before or alongside each subsystem, not
  after the fact.
- **FR-1.4** The port must reproduce gameplay behaviour rather than build-time
  copy protection, encryption, checksum choreography, or disc boot ceremony,
  unless gameplay fidelity depends on those behaviours.
- **FR-1.5** New port code must not be placed inside `tools/jsbeeb` unless an
  emulator API is genuinely missing and cannot be wrapped externally.
- **FR-1.6** The browser port must preserve gameplay behaviour, important data
  formats, and deterministic computational outputs.
- **FR-1.7** The runtime implementation must use TypeScript modules instead of
  6502 assembly.
- **FR-1.8** Computational correctness must be verified against the reference
  build via `jsbeeb`.
- **FR-1.9** Audiovisual fidelity must be verified with screenshots, controlled
  scenarios, and human playtesting.

### FR-2: Reference Materials And Repository Layout

- **FR-2.1** The implementation must use the following local sources as
  reference material:
  - `docs/intent.md`
  - `docs/elite-source-code-bbc-master/1-source-files/main-sources/elite-source.asm`
  - `docs/elite-source-code-bbc-master/1-source-files/main-sources/elite-data.asm`
  - `docs/elite-source-code-bbc-master/1-source-files/main-sources/elite-loader.asm`
  - `docs/elite-source-code-bbc-master/5-compiled-game-discs/elite-master-sng47.ssd`
  - `docs/bbcelite-master/index.md`
  - `docs/bbcelite-master/articles/map_of_the_source_code.md`
  - `docs/bbcelite-master/articles/source_code_cross-references.md`
  - `docs/bbcelite-master/indexes/subroutines.md`
  - `docs/bbcelite-master/indexes/variables.md`
  - `docs/bbcelite-master/indexes/workspaces.md`
- **FR-2.2** The implementation must use the following local emulator and
  tooling references:
  - `tools/jsbeeb/src/machine-session.js`
  - `tools/jsbeeb/tests/test-machine.js`
  - `tools/jsbeeb/docs/snapshot-format.md`
  - `tools/jsbeeb/tests/`
- **FR-2.3** Online references may be used only when the local mirror is not
  enough:
  - `https://elite.bbcelite.com/terminology/`
  - `https://elite.bbcelite.com/deep_dives/`
  - `https://elite.bbcelite.com/about_site/building_elite.html`
- **FR-2.4** New implementation code must use this target repository layout:

```text
src/
  app/
    browser-entry.ts
    game-shell.ts
  core/
    arithmetic/
    data/
    state/
    world/
    simulation/
    presentation/
  platform/
    browser/
      input/
      audio/
      storage/
    render/
      canvas2d/
  ui/
    screens/
    hud/
    components/
  services/
    runtime/
    assets/
scripts/
  reference/
  fixtures/
tests/
  unit/
  integration/
  reference/
  visual/
  fixtures/
    reference/
    visual/
```

- **FR-2.5** Existing repository locations must be used as follows:
  - keep source corpora under `docs/`
  - keep the emulator under `tools/jsbeeb`
  - keep temporary planning and runbooks under `docs/tasks`

### FR-3: Repeatable Porting Loop And Macro-Order

- **FR-3.1** After the initial scaffold exists, every subsystem must follow
  this repeatable loop:
  1. Start with the relevant `docs/bbcelite-master` pages, not the assembler.
  2. Use `articles/source_code_cross-references.md` and `indexes/` to identify
     every routine, variable, workspace, and table that participates.
  3. Read the exact implementation in
     `docs/elite-source-code-bbc-master/1-source-files/main-sources`.
  4. Decide whether the subsystem is pure deterministic logic, stateful
     simulation, rendering/presentation, or a platform adapter.
  5. Add or extend a `jsbeeb` extraction script before writing the ported
     module.
  6. Save fixture outputs under
     `tests/fixtures/reference/<subsystem>/`.
  7. Implement the TypeScript module in `src/`.
  8. Add unit, integration, reference, or visual tests as appropriate.
  9. Integrate only after the subsystem matches the captured evidence.
- **FR-3.2** The implementation must use this exact macro-order:
  1. Reference contract
  2. TypeScript scaffold
  3. Reference harness
  4. Fixture conventions
  5. Arithmetic
  6. Typed state
  7. Static data
  8. RNG and world generation
  9. Geometry and drawing primitives
  10. Local universe and movement
  11. Tactics, combat, docking, scooping
  12. Main flight loop
  13. Main game loop
  14. Economy, navigation, missions, saves
  15. Text systems
  16. Rendering
  17. Browser adapters
  18. Startup and product flow
  19. Automated and human validation
  20. Packaging and release
- **FR-3.3** Rendering, UI, and browser shell work must not move ahead of
  deterministic core simulation unless a small amount of shell scaffolding is
  required to keep the project runnable.

### FR-4: Freeze The Reference Contract

- **FR-4.1** The project must create and maintain this runbook at
  `docs/elite-typescript-port-step-by-step.md`.
- **FR-4.2** The project must create and maintain a short reference contract at
  `docs/tasks/reference-contract.md`.
- **FR-4.3** `reference-contract.md` must include:
  - canonical machine: `BBC Master 128 (DFS)` in `jsbeeb`
  - canonical game disc:
    `docs/elite-source-code-bbc-master/5-compiled-game-discs/elite-master-sng47.ssd`
  - canonical reference corpus:
    `docs/elite-source-code-bbc-master` and `docs/bbcelite-master`
  - non-canonical exploratory artifact:
    `tools/elite-compendium-bbc-master.dsd`
  - explicit non-goals for phase 1:
    loader copy protection, binary encryption, variant switches, non-SNG47
    builds
- **FR-4.4** Before implementation starts, the team must read:
  - `docs/intent.md`
  - `docs/bbcelite-master/index.md`
  - `docs/elite-source-code-bbc-master/README.md`
- **FR-4.5** No implementation work may begin until the reference variant and
  non-goals are written down.

### FR-5: Create The TypeScript And Browser Scaffold

- **FR-5.1** The project must create:
  - root `package.json`
  - root `tsconfig.json`
  - root `vite.config.ts`
  - root `vitest.config.ts`
  - `index.html`
  - `src/app/browser-entry.ts`
  - `src/app/game-shell.ts`
  - `src/ui/`
  - `tests/`
- **FR-5.2** The scaffold must:
  - use TypeScript everywhere in the new port
  - keep emulator harness code outside the browser bundle where possible
  - define path aliases up front for `src/core`, `src/platform`, and `src/ui`
  - add scripts for `dev`, `build`, `test`, `test:reference`, and
    `test:visual`
  - align the local Node version with `tools/jsbeeb/package.json` unless there
    is a deliberate reason to split the toolchains
- **FR-5.3** Before implementation starts, the team must read:
  - `tools/jsbeeb/package.json`
  - `tools/jsbeeb/README.md`
- **FR-5.4** The scaffold gate is met only when a browser app starts, a test
  runner starts, and the repository has stable places for runtime code, harness
  code, and fixtures.

### FR-6: Build The Reference Harness On Top Of jsbeeb

- **FR-6.1** The project must create:
  - `scripts/reference/session.ts`
  - `scripts/reference/boot-reference.ts`
  - `scripts/reference/snapshot.ts`
  - `scripts/reference/breakpoints.ts`
  - `tests/reference/reference-harness.test.ts`
- **FR-6.2** `scripts/reference/boot-reference.ts` must boot the SNG47 disc
  and confirm the session contract.
- **FR-6.3** This layer must expose:
  - booting the reference disc
  - run-for-cycle and run-until-address helpers
  - memory read/write helpers
  - key input helpers
  - text capture helpers
  - screenshot capture helpers
  - snapshot/restore helpers
- **FR-6.4** Before implementation starts, the team must read:
  - `tools/jsbeeb/src/machine-session.js`
  - `tools/jsbeeb/tests/test-machine.js`
  - `tools/jsbeeb/docs/snapshot-format.md`
- **FR-6.5** The harness gate is met only when it can boot the SNG47 disc, take
  a screenshot, capture text output, stop on breakpoints, read memory, write
  memory, and restore from a snapshot.

### FR-7: Define Fixture Conventions Before Subsystem Ports

- **FR-7.1** The project must create:
  - `tests/fixtures/reference/manifest.json`
  - `tests/fixtures/reference/README.md`
  - `tests/fixtures/visual/README.md`
  - `scripts/fixtures/extract-fixture.ts`
  - `scripts/fixtures/capture-scene.ts`
- **FR-7.2** Fixture structure must include:
  - `tests/fixtures/reference/rng/`
  - `tests/fixtures/reference/galaxy/`
  - `tests/fixtures/reference/market/`
  - `tests/fixtures/reference/text/`
  - `tests/fixtures/reference/movement/`
  - `tests/fixtures/reference/tactics/`
  - `tests/fixtures/reference/commander/`
  - `tests/fixtures/visual/screens/`
  - `tests/fixtures/visual/scenes/`
- **FR-7.3** Every fixture record should include:
  - reference disc and model
  - source routine or routines
  - setup method: boot path, snapshot, input sequence, or memory patch
  - exact input bytes or state values
  - exact expected output bytes, values, or image
  - capture script used to produce it
- **FR-7.4** No core subsystem may be implemented without at least one
  extraction path and one fixture location already defined.

### FR-8: Implement Fixed-Width Arithmetic And Bit Semantics

- **FR-8.1** The project must create:
  - `src/core/arithmetic/u8.ts`
  - `src/core/arithmetic/u16.ts`
  - `src/core/arithmetic/bitwise.ts`
  - `src/core/arithmetic/sign-magnitude.ts`
  - `src/core/arithmetic/fixed-width.ts`
  - `tests/unit/arithmetic/*.test.ts`
- **FR-8.2** The arithmetic layer must handle:
  - masking to 8 and 16 bits
  - carry and borrow helpers
  - signed and unsigned interpretations
  - rotate and shift behaviour
  - sign-magnitude helpers for coordinate storage
  - small utility wrappers that keep simulation code readable
- **FR-8.3** Before implementation starts, the team must read:
  - `docs/bbcelite-master/main/subroutine/dornd.md`
  - `https://elite.bbcelite.com/terminology/`
- **FR-8.4** Arithmetic tests must cover wraparound, carry, sign-magnitude
  decode and encode, and any other byte-level behaviour depended on by RNG,
  movement, combat, and saves.

### FR-9: Model Workspaces, Ship Blocks, And Commander Data Explicitly

- **FR-9.1** The project must create:
  - `src/core/state/workspaces/zp.ts`
  - `src/core/state/workspaces/wp.ts`
  - `src/core/state/workspaces/up.ts`
  - `src/core/state/ships/k-block.ts`
  - `src/core/state/ships/inwk.ts`
  - `src/core/state/commander/commander-state.ts`
  - `src/core/state/serialization/`
  - `tests/unit/state/*.test.ts`
- **FR-9.2** The mapping source for these structures must be:
  - `docs/bbcelite-master/main/workspace/zp.md`
  - `docs/bbcelite-master/main/workspace/wp.md`
  - `docs/bbcelite-master/main/workspace/up.md`
  - `docs/bbcelite-master/main/workspace/k_per_cent.md`
  - `docs/bbcelite-master/indexes/variables.md`
- **FR-9.3** State objects should expose named fields and typed records, but
  also support lossless conversion to and from byte layouts where fixtures and
  save data require it.
- **FR-9.4** State structures must round-trip cleanly through
  serializer/deserializer tests and represent all bytes needed by the first
  ported subsystems.

### FR-10: Port Static Game Data And Lookup Tables

- **FR-10.1** The project must create:
  - `src/core/data/ship-blueprints.ts`
  - `src/core/data/markets.ts`
  - `src/core/data/tokens.ts`
  - `src/core/data/system-name-tokens.ts`
  - `src/core/data/dashboard.ts`
  - `src/core/data/lookup-tables.ts`
  - `src/core/assets/`
- **FR-10.2** The primary sources for this work must be:
  - `docs/elite-source-code-bbc-master/1-source-files/main-sources/elite-data.asm`
  - `docs/bbcelite-master/game_data/`
  - `docs/bbcelite-master/indexes/variables.md`
- **FR-10.3** The runtime must represent the data needed for behaviour and
  presentation, but must not copy the annotated prose from the reference corpus
  into the runtime codebase.
- **FR-10.4** The runtime must load ship blueprint data, token tables, and
  market tables from typed modules with tests confirming they match extracted
  reference fixtures.

### FR-11: Port RNG And Deterministic World Generation

- **FR-11.1** The project must create:
  - `src/core/world/rng.ts`
  - `src/core/world/galaxy/seed.ts`
  - `src/core/world/galaxy/system-generation.ts`
  - `src/core/world/galaxy/system-names.ts`
  - `src/core/world/market/market-generation.ts`
  - `tests/reference/rng.test.ts`
  - `tests/reference/galaxy.test.ts`
  - `tests/reference/market.test.ts`
- **FR-11.2** The local docs for this work must be:
  - `docs/bbcelite-master/main/subroutine/dornd.md`
  - `docs/bbcelite-master/main/workspace/zp.md#rand`
  - `docs/bbcelite-master/main/subroutine/gvl.md`
  - `docs/bbcelite-master/main/subroutine/tt151.md`
  - `docs/bbcelite-master/main/variable/qq23.md`
- **FR-11.3** The following online deep dives may be used when the local mirror
  is not enough:
  - "Galaxy and system seeds"
  - "Generating system data"
  - "Generating system names"
  - "Twisting the system seeds"
  - "Market item prices and availability"
- **FR-11.4** Known galaxy seeds, system names, descriptions, prices, and
  availabilities must match reference fixtures exactly.

### FR-12: Port Geometry, Projection, And Low-Level Presentation Primitives

- **FR-12.1** The project must create:
  - `src/core/presentation/vector.ts`
  - `src/core/presentation/arctan.ts`
  - `src/core/presentation/projection.ts`
  - `src/core/presentation/line-rasterizer.ts`
  - `src/core/presentation/pixel-plotter.ts`
  - `src/core/presentation/circle.ts`
  - `tests/reference/projection.test.ts`
  - `tests/reference/rasterizer.test.ts`
- **FR-12.2** Before implementation starts, the team must read:
  - `docs/bbcelite-master/main/subroutine/arctan.md`
  - `docs/bbcelite-master/main/subroutine/proj.md`
  - `docs/bbcelite-master/main/subroutine/loinq_part_1_of_7.md`
  - `docs/bbcelite-master/main/subroutine/hloin.md`
  - `docs/bbcelite-master/main/subroutine/pixel.md`
  - `docs/bbcelite-master/main/subroutine/circle2.md`
- **FR-12.3** Projection helpers and drawing primitives must match reference
  outputs in isolated tests before any full-scene renderer is written.

### FR-13: Port Local-Universe Data Structures And Ship Movement

- **FR-13.1** The project must create:
  - `src/core/simulation/universe/universe-state.ts`
  - `src/core/simulation/universe/local-bubble.ts`
  - `src/core/simulation/ships/spawn.ts`
  - `src/core/simulation/ships/move-ship.ts`
  - `src/core/simulation/ships/orientation.ts`
  - `src/core/simulation/scanner/scanner-state.ts`
  - `tests/reference/movement.test.ts`
  - `tests/reference/local-bubble.test.ts`
- **FR-13.2** Before implementation starts, the team must read:
  - `docs/bbcelite-master/main/variable/univ.md`
  - `docs/bbcelite-master/main/subroutine/nwshp.md`
  - `docs/bbcelite-master/main/subroutine/nwsps.md`
  - `docs/bbcelite-master/main/subroutine/mveit_part_1_of_9.md`
  - `docs/bbcelite-master/main/subroutine/mveit_part_9_of_9.md`
  - `docs/bbcelite-master/main/workspace/zp.md#inwk`
  - `docs/bbcelite-master/main/workspace/k_per_cent.md`
- **FR-13.3** The following online deep dives may be used when the local mirror
  is not enough:
  - "The local bubble of universe"
  - "Ship data blocks"
  - "Program flow of the ship-moving routine"
  - "Orientation vectors"
  - "Tidying orthonormal vectors"
- **FR-13.4** Ship spawn, update, and orientation transforms must match the
  reference harness for captured scenarios.

### FR-14: Port Tactics, Combat, Collision, Docking, And Scooping

- **FR-14.1** The project must create:
  - `src/core/simulation/tactics/tactics.ts`
  - `src/core/simulation/combat/laser.ts`
  - `src/core/simulation/combat/missiles.ts`
  - `src/core/simulation/combat/collision.ts`
  - `src/core/simulation/docking/docking-checks.ts`
  - `src/core/simulation/docking/docking-computer.ts`
  - `src/core/simulation/cargo/scooping.ts`
  - `tests/reference/tactics.test.ts`
  - `tests/reference/combat.test.ts`
  - `tests/reference/docking.test.ts`
- **FR-14.2** Before implementation starts, the team must read:
  - `docs/bbcelite-master/main/subroutine/tactics_part_1_of_7.md`
  - `docs/bbcelite-master/main/subroutine/tactics_part_7_of_7.md`
  - `docs/bbcelite-master/main/subroutine/hitch.md`
  - `docs/bbcelite-master/main/subroutine/dockit.md`
  - `docs/bbcelite-master/main/subroutine/main_flight_loop_part_7_of_16.md`
  - `docs/bbcelite-master/main/subroutine/main_flight_loop_part_11_of_16.md`
- **FR-14.3** The following online deep dives may be used when the local mirror
  is not enough:
  - "Docking checks"
  - "The docking computer"
  - "Program flow of the tactics routine"
  - "In the crosshairs"
- **FR-14.4** Captured combat and docking scenarios must produce the same state
  transitions, targeting outcomes, and collision results as the reference
  machine.

### FR-15: Port The Main Flight Loop

- **FR-15.1** The project must create:
  - `src/core/simulation/flight/main-flight-loop.ts`
  - `src/core/simulation/flight/flight-context.ts`
  - `src/core/simulation/flight/flight-step.ts`
  - `tests/reference/main-flight-loop.test.ts`
- **FR-15.2** Before implementation starts, the team must read:
  - `docs/bbcelite-master/main/subroutine/main_flight_loop_part_1_of_16.md`
  - `docs/bbcelite-master/main/subroutine/main_flight_loop_part_16_of_16.md`
  - `docs/bbcelite-master/articles/map_of_the_source_code.md`
- **FR-15.3** The TypeScript flight loop should be readable and modular,
  preserve ordering and observable effects, but not preserve the original
  control-flow shape if a clearer structure produces identical behaviour.
- **FR-15.4** Stepped flight-loop captures from the port and `jsbeeb` must
  agree for curated scenarios covering combat, docking, planet approach, and
  ordinary travel.

### FR-16: Port The Main Game Loop And Docked Flows

- **FR-16.1** The project must create:
  - `src/core/simulation/game/main-game-loop.ts`
  - `src/core/simulation/game/scheduler.ts`
  - `src/core/simulation/game/spawn-rules.ts`
  - `src/core/simulation/game/view-state.ts`
  - `tests/reference/main-game-loop.test.ts`
- **FR-16.2** Before implementation starts, the team must read:
  - `docs/bbcelite-master/main/subroutine/tt170.md`
  - `docs/bbcelite-master/main/subroutine/main_game_loop_part_1_of_6.md`
  - `docs/bbcelite-master/main/subroutine/main_game_loop_part_6_of_6.md`
  - `docs/bbcelite-master/main/subroutine/title.md`
  - `docs/bbcelite-master/main/subroutine/doentry.md`
- **FR-16.3** The following online deep dives may be used when the local mirror
  is not enough:
  - "Program flow of the main game loop"
  - "Scheduling tasks with the main loop counter"
- **FR-16.4** The top-level loop must enter play, transition to docked flows,
  spawn game events, and continue event scheduling in the same order as the
  reference build.

### FR-17: Port Economy, Equipment, Hyperspace, Missions, And Commander State

- **FR-17.1** The project must create:
  - `src/core/simulation/economy/trade.ts`
  - `src/core/simulation/economy/equipment.ts`
  - `src/core/simulation/economy/status.ts`
  - `src/core/simulation/navigation/galaxy-map.ts`
  - `src/core/simulation/navigation/hyperspace.ts`
  - `src/core/simulation/missions/mission-state.ts`
  - `src/core/simulation/commander/save-format.ts`
  - `src/core/simulation/commander/save-load.ts`
  - `tests/reference/economy.test.ts`
  - `tests/reference/commander.test.ts`
  - `tests/reference/mission.test.ts`
- **FR-17.2** Before implementation starts, the team must read:
  - `docs/bbcelite-master/main/subroutine/trademode.md`
  - `docs/bbcelite-master/main/subroutine/eqshp.md`
  - `docs/bbcelite-master/main/subroutine/status.md`
  - `docs/bbcelite-master/main/subroutine/hme2.md`
  - `docs/bbcelite-master/main/subroutine/hyp.md`
  - `docs/bbcelite-master/main/subroutine/sve.md`
  - `docs/bbcelite-master/main/subroutine/wfile.md`
  - `docs/bbcelite-master/main/subroutine/rfile.md`
  - `docs/bbcelite-master/main/subroutine/check.md`
  - `docs/bbcelite-master/main/subroutine/dfault.md`
  - `docs/bbcelite-master/main/variable/chk.md`
  - `docs/bbcelite-master/main/variable/chk2.md`
  - `docs/bbcelite-master/main/variable/na_per_cent.md`
  - `docs/bbcelite-master/main/variable/na2_per_cent.md`
- **FR-17.3** The player must be able to trade, equip, hyperspace, progress
  missions, and save or load a commander while matching captured reference
  data.

### FR-18: Port Text Systems And UI-Facing Token Expansion

- **FR-18.1** The project must create:
  - `src/core/presentation/text/token-expansion.ts`
  - `src/core/presentation/text/goat-soup.ts`
  - `src/core/presentation/text/formatting.ts`
  - `tests/reference/text.test.ts`
- **FR-18.2** Before implementation starts, the team must read:
  - `docs/bbcelite-master/main/subroutine/detok.md`
  - `docs/bbcelite-master/main/subroutine/detok2.md`
  - `docs/bbcelite-master/main/subroutine/detok3.md`
  - `docs/bbcelite-master/main/variable/jmtb.md`
  - `docs/bbcelite-master/game_data/variable/tkn1.md`
  - `docs/bbcelite-master/game_data/variable/rutok.md`
- **FR-18.3** The following online deep dives may be used when the local mirror
  is not enough:
  - "Printing text tokens"
  - "Extended text tokens"
  - "Extended system descriptions"
- **FR-18.4** System names, market text, mission text, and generated
  descriptions must match the reference outputs for a representative fixture
  set.

### FR-19: Implement Rendering Adapters And Scene Renderers

- **FR-19.1** The project must create:
  - `src/platform/render/canvas2d/canvas-renderer.ts`
  - `src/platform/render/canvas2d/framebuffer.ts`
  - `src/core/presentation/renderers/ship-renderer.ts`
  - `src/core/presentation/renderers/planet-renderer.ts`
  - `src/core/presentation/renderers/sun-renderer.ts`
  - `src/core/presentation/renderers/starfield-renderer.ts`
  - `src/core/presentation/renderers/scanner-renderer.ts`
  - `src/core/presentation/renderers/dashboard-renderer.ts`
  - `src/core/presentation/renderers/sights-renderer.ts`
  - `tests/visual/*.test.ts`
- **FR-19.2** Before implementation starts, the team must read:
  - `docs/bbcelite-master/main/subroutine/ll9_part_1_of_12.md`
  - `docs/bbcelite-master/main/subroutine/ll9_part_12_of_12.md`
  - `docs/bbcelite-master/main/subroutine/planet.md`
  - `docs/bbcelite-master/main/subroutine/sun_part_1_of_4.md`
  - `docs/bbcelite-master/main/subroutine/sun_part_4_of_4.md`
  - `docs/bbcelite-master/main/subroutine/stars.md`
  - `docs/bbcelite-master/main/subroutine/scan.md`
  - `docs/bbcelite-master/main/subroutine/dials_part_1_of_4.md`
  - `docs/bbcelite-master/main/subroutine/dials_part_4_of_4.md`
  - `docs/bbcelite-master/main/subroutine/sight.md`
- **FR-19.3** Canvas2D must be the first adapter, and the renderer boundary
  must remain clean enough that a future WebGL or WebGPU renderer can replace
  it.
- **FR-19.4** Bounded scenes must produce stable screenshots that pass pixel or
  perceptual regression thresholds against captured reference images.

### FR-20: Implement Browser Input, Audio, Storage, And Shell Integration

- **FR-20.1** The project must create:
  - `src/platform/browser/input/keyboard.ts`
  - `src/platform/browser/input/joystick.ts`
  - `src/platform/browser/audio/audio-engine.ts`
  - `src/platform/browser/storage/commander-storage.ts`
  - `src/app/game-shell.ts`
  - `src/ui/screens/*.ts`
  - `src/ui/hud/*.ts`
  - `tests/integration/browser-shell.test.ts`
- **FR-20.2** Before implementation starts, the team must read:
  - `docs/bbcelite-master/main/subroutine/shift.md`
  - `docs/bbcelite-master/main/subroutine/return.md`
  - `docs/bbcelite-master/main/subroutine/djoy.md`
  - `docs/bbcelite-master/main/subroutine/rdjoy.md`
  - `docs/bbcelite-master/main/subroutine/boop.md`
  - `docs/bbcelite-master/main/subroutine/beep.md`
  - `docs/bbcelite-master/main/subroutine/noise.md`
  - `docs/bbcelite-master/main/subroutine/soint.md`
  - `docs/bbcelite-master/main/workspace/sound_variables.md`
- **FR-20.3** Keyboard control, optional joystick support, audio cues, save
  persistence, and screen transitions must work in the browser build.

### FR-21: Integrate Startup, Title Flow, And User-Facing Product Behaviour

- **FR-21.1** The project must create:
  - `src/app/bootstrap.ts`
  - `src/ui/screens/title-screen.ts`
  - `src/ui/screens/loading-screen.ts`
  - `src/ui/screens/flight-screen.ts`
  - `src/ui/screens/docked-screen.ts`
  - `tests/integration/startup-flow.test.ts`
- **FR-21.2** Before implementation starts, the team must read:
  - `docs/bbcelite-master/main/subroutine/title.md`
  - `docs/elite-source-code-bbc-master/1-source-files/main-sources/elite-loader.asm`
  - `docs/bbcelite-master/main/subroutine/hanger.md`
- **FR-21.3** The implementation must preserve title-screen and hangar feel,
  but must not reproduce disc-loader choreography or copy-protection
  mechanisms that existed only for the original delivery medium.
- **FR-21.4** The browser build must boot to a usable title flow, enter
  gameplay, and return to docked/title paths cleanly.

### FR-22: Expand Automated Evidence And Perform Side-By-Side Playtesting

- **FR-22.1** The project must create and maintain:
  - `tests/reference/acceptance/`
  - `tests/visual/acceptance/`
  - `docs/tasks/playtest-checklist.md`
  - `docs/tasks/known-fidelity-gaps.md`
- **FR-22.2** Automated checks must cover:
  - deterministic fixtures for core simulation
  - visual regression scenes for dashboard, scanner, ships, sun, planet, and
    title/loading screens
  - save/load round trips
  - end-to-end flow tests for launch, docking, trade, hyperspace, combat,
    death, and save/load
- **FR-22.3** Human checks must cover:
  - input feel
  - movement feel
  - docking feel
  - combat feel
  - audio feel
  - general pacing and readability
- **FR-22.4** Every known fidelity gap must be either fixed, intentionally
  accepted, or recorded with a clear follow-up.

### FR-23: Package, Document, And Ship The Browser Port

- **FR-23.1** The project must create or update:
  - root `README.md`
  - `docs/tasks/release-checklist.md`
  - browser build output configuration
  - CI scripts for tests and builds
- **FR-23.2** Release requirements must include:
  - one command for local development
  - one command for the full test suite
  - one command for reference extraction refresh
  - one command for production build
  - documentation that explains the reference corpus, legal constraints, and
    how the verification harness works
- **FR-23.3** The release gate is met only when another engineer can clone the
  repository, install dependencies, run the browser build, run the tests, and
  understand the relationship between the port and the reference corpus.

## 5. Non-Goals (Out of Scope)

- Loader copy protection is out of scope for phase 1.
- Binary encryption is out of scope for phase 1.
- Variant switches are out of scope for phase 1.
- Non-SNG47 builds are out of scope for phase 1.
- Build-time copy protection, encryption, checksum choreography, and disc boot
  ceremony are out of scope unless gameplay fidelity depends on them.
- Treating 6502 assembly as the runtime implementation is out of scope.
- Putting new port code inside `tools/jsbeeb` is out of scope unless an
  emulator API is genuinely missing and cannot be wrapped externally.
- Keeping opaque memory blobs as the main runtime API is out of scope.
- Copying annotated prose from the reference corpus into the runtime codebase
  is out of scope.
- Reusing the emulator's video pipeline is out of scope.
- Preserving the original flight-loop control-flow shape is out of scope when a
  clearer structure produces identical behaviour.
- Reproducing disc-loader choreography or copy-protection mechanisms that
  existed only for the original delivery medium is out of scope.

## 6. Design Considerations

- The port should be browser-native and implemented from scratch in this
  repository.
- The runtime should be TypeScript, not 6502 assembly.
- Evidence capture should be built into the development workflow before module
  integration.
- The implementation should start from `docs/bbcelite-master` pages before raw
  assembler and should use cross-references and indexes to map participating
  routines, variables, workspaces, and tables.
- Each subsystem should be classified as pure deterministic logic, stateful
  simulation, rendering/presentation, or platform adapter before it is ported.
- State structures should expose named fields and typed records while retaining
  lossless byte-layout conversion where fixtures and save data need it.
- The TypeScript flight loop should be readable and modular while preserving
  ordering and observable effects.
- Text output should become data-driven TypeScript rather than screen-memory-era
  printing logic.
- Canvas2D should be the first rendering adapter, but the renderer boundary
  should remain replaceable by a future WebGL or WebGPU implementation.
- Title-screen and hangar feel should be preserved even though original
  disc-loader choreography and copy-protection mechanisms are not reproduced.

## 7. Technical Considerations

- Emulator harness code should stay outside the browser bundle where possible.
- Path aliases should be defined up front for `src/core`, `src/platform`, and
  `src/ui`.
- The local Node version should align with `tools/jsbeeb/package.json` unless
  there is a deliberate reason to split the toolchains.
- The reference harness should wrap
  `tools/jsbeeb/src/machine-session.js` and incorporate behaviour demonstrated
  in `tools/jsbeeb/tests/test-machine.js` and
  `tools/jsbeeb/docs/snapshot-format.md`.
- The harness must support booting, cycle stepping, address stepping, memory
  reads and writes, key input, text capture, screenshot capture, and
  snapshot/restore.
- Fixture records should always capture source routines, setup method, exact
  inputs, exact expected outputs, and the capture script used.
- Browser input, audio, storage, and rendering adapters should remain separate
  from deterministic core simulation.
- Browser build output configuration and CI scripts should exist by release
  time.
- Documentation should explain the reference corpus, legal constraints, and the
  verification harness.
- Online references should be used only when the local mirror is not enough for
  a conceptual gap.

## 8. Success Metrics

- The browser build is playable from title screen through ordinary gameplay.
- Deterministic subsystems pass fixture-backed reference tests.
- Known galaxy seeds, system names, descriptions, prices, and availabilities
  match reference fixtures exactly.
- Projection helpers, drawing primitives, ship movement, combat, docking, main
  loops, text output, and other curated scenarios agree with captured reference
  evidence.
- Rendering passes bounded visual regression checks against captured reference
  images.
- Save/load is compatible with the chosen commander-state rules for the port.
- Human side-by-side playtesting says the game feels like the reference build.
- Every known fidelity gap is fixed, intentionally accepted, or recorded with a
  clear follow-up.
- The codebase is typed, modular, and understandable without reading assembly.
- Another engineer can clone the repository, install dependencies, run the
  browser build, run the tests, and understand the relationship between the
  port and the reference corpus.

## 9. Open Questions

None identified in source document.

## 10. Additional Context

### Existing Reference Material

**Local sources**

- `docs/intent.md`: project intent and delivery order
- `docs/elite-source-code-bbc-master/1-source-files/main-sources/elite-source.asm`:
  main game source
- `docs/elite-source-code-bbc-master/1-source-files/main-sources/elite-data.asm`:
  ship, text, and game data source
- `docs/elite-source-code-bbc-master/1-source-files/main-sources/elite-loader.asm`:
  loader and title/startup behaviour
- `docs/elite-source-code-bbc-master/5-compiled-game-discs/elite-master-sng47.ssd`:
  canonical reference disc to boot in the emulator
- `docs/bbcelite-master/index.md`: orientation for the BBC Master variant
- `docs/bbcelite-master/articles/map_of_the_source_code.md`: subsystem map
- `docs/bbcelite-master/articles/source_code_cross-references.md`: dependency
  and usage map
- `docs/bbcelite-master/indexes/subroutines.md`,
  `docs/bbcelite-master/indexes/variables.md`,
  `docs/bbcelite-master/indexes/workspaces.md`: symbol discovery

**Local emulator/tooling**

- `tools/jsbeeb/src/machine-session.js`: headless boot, run, memory,
  breakpoint, input, and screenshot API
- `tools/jsbeeb/tests/test-machine.js`: lower-level machine control and
  snapshot/restore support
- `tools/jsbeeb/docs/snapshot-format.md`: saved-state structure
- `tools/jsbeeb/tests/`: emulator regression patterns to copy for the port

**Online references for conceptual gaps**

Use these only when the local mirror is not enough:

- `https://elite.bbcelite.com/terminology/`
- `https://elite.bbcelite.com/deep_dives/`
- `https://elite.bbcelite.com/about_site/building_elite.html`

### Definition Of Done

The implementation is done when all of the following are true:

- the browser build is playable from title screen through ordinary gameplay
- deterministic subsystems pass fixture-backed reference tests
- rendering passes bounded visual regression checks
- save/load is compatible with the chosen commander-state rules for the port
- human side-by-side playtesting says the game feels like the reference build
- the codebase is typed, modular, and understandable without reading assembly
