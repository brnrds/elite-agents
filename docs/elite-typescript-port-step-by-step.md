# Elite TypeScript Port Step-by-Step Implementation Runbook

This document turns `docs/intent.md` into an execution plan for building a
browser-native TypeScript port of Elite without treating the 6502 assembly as
the runtime implementation.

It is written as though the port will be implemented in this repository from
scratch. The port code does not exist yet, so this document also defines where
new code should live.

## Target Result

Ship a playable browser version of `BBC Master 128 Elite (Acornsoft)` that:

- preserves gameplay behaviour, important data formats, and deterministic
  computational outputs
- uses TypeScript modules instead of 6502 assembly as the runtime
- verifies computational correctness against the reference build via `jsbeeb`
- verifies audiovisual fidelity with screenshots, controlled scenarios, and
  human playtesting

## Operational Rules

1. Treat `BBC Master 128 Elite (Acornsoft)` as the only reference variant for
   the first implementation pass.
2. Use the local source and commentary corpus before reading raw assembly.
3. Build fixtures before or alongside each subsystem, not after the fact.
4. Port gameplay behaviour, not build-time copy protection, encryption,
   checksum choreography, or disc boot ceremony unless gameplay fidelity
   depends on it.
5. Do not put new port code inside `tools/jsbeeb` unless an emulator API is
   genuinely missing and cannot be wrapped externally.

## Existing Reference Material

### Local sources

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

### Local emulator/tooling

- `tools/jsbeeb/src/machine-session.js`: headless boot, run, memory, breakpoint,
  input, and screenshot API
- `tools/jsbeeb/tests/test-machine.js`: lower-level machine control and
  snapshot/restore support
- `tools/jsbeeb/docs/snapshot-format.md`: saved-state structure
- `tools/jsbeeb/tests/`: emulator regression patterns to copy for the port

### Online references for conceptual gaps

Use these only when the local mirror is not enough:

- `https://elite.bbcelite.com/terminology/`
- `https://elite.bbcelite.com/deep_dives/`
- `https://elite.bbcelite.com/about_site/building_elite.html`

## Target Repository Layout

Create the new implementation in the following locations:

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

Use the existing repository locations as follows:

- keep source corpora under `docs/`
- keep the emulator under `tools/jsbeeb`
- keep temporary planning and runbooks under `docs/tasks`

## Repeatable Porting Loop

Apply this loop to every subsystem after the initial scaffold exists:

1. Start with the relevant `docs/bbcelite-master` pages, not the assembler.
2. Use `articles/source_code_cross-references.md` and `indexes/` to identify
   every routine, variable, workspace, and table that participates.
3. Read the exact implementation in
   `docs/elite-source-code-bbc-master/1-source-files/main-sources`.
4. Decide whether the subsystem is:
   - pure deterministic logic
   - stateful simulation
   - rendering/presentation
   - platform adapter
5. Add or extend a `jsbeeb` extraction script before writing the ported module.
6. Save fixture outputs under `tests/fixtures/reference/<subsystem>/`.
7. Implement the TypeScript module in `src/`.
8. Add unit, integration, reference, or visual tests as appropriate.
9. Integrate only after the subsystem matches the captured evidence.

## Ordered Implementation Steps

## 1. Freeze the reference contract

Outcome:
Document exactly what "correct" means before any code exists.

Create and maintain:

- this runbook at `docs/elite-typescript-port-step-by-step.md`
- a short reference contract at `docs/tasks/reference-contract.md`

Put in `reference-contract.md`:

- canonical machine: `BBC Master 128 (DFS)` in `jsbeeb`
- canonical game disc:
  `docs/elite-source-code-bbc-master/5-compiled-game-discs/elite-master-sng47.ssd`
- canonical reference corpus:
  `docs/elite-source-code-bbc-master` and `docs/bbcelite-master`
- non-canonical exploratory artifact:
  `tools/elite-compendium-bbc-master.dsd`
- explicit non-goals for phase 1:
  loader copy protection, binary encryption, variant switches, non-SNG47 builds

Read first:

- `docs/intent.md`
- `docs/bbcelite-master/index.md`
- `docs/elite-source-code-bbc-master/README.md`

Gate:
No implementation work starts until the reference variant and non-goals are
written down.

## 2. Create the TypeScript/browser scaffold

Outcome:
A clean application shell exists for the future port and test suite.

Create:

- root `package.json`
- root `tsconfig.json`
- root `vite.config.ts`
- root `vitest.config.ts`
- `index.html`
- `src/app/browser-entry.ts`
- `src/app/game-shell.ts`
- `src/ui/`
- `tests/`

Required design decisions:

- use TypeScript everywhere in the new port
- keep emulator harness code outside the browser bundle where possible
- define path aliases up front for `src/core`, `src/platform`, and `src/ui`
- add scripts for `dev`, `build`, `test`, `test:reference`, and `test:visual`
- align the local Node version with `tools/jsbeeb/package.json` unless there is
  a deliberate reason to split the toolchains

Read first:

- `tools/jsbeeb/package.json`
- `tools/jsbeeb/README.md`

Gate:
A browser app starts, a test runner starts, and the repository has stable
places for runtime code, harness code, and fixtures.

## 3. Build the reference harness on top of jsbeeb

Outcome:
The port has a deterministic oracle for fixture extraction.

Create:

- `scripts/reference/session.ts`: wrapper around
  `tools/jsbeeb/src/machine-session.js`
- `scripts/reference/boot-reference.ts`: boots the SNG47 disc and confirms the
  session contract
- `scripts/reference/snapshot.ts`: snapshot/restore helpers
- `scripts/reference/breakpoints.ts`: reusable execute/read/write breakpoint
  helpers
- `tests/reference/reference-harness.test.ts`

This layer should expose:

- booting the reference disc
- run-for-cycle and run-until-address helpers
- memory read/write helpers
- key input helpers
- text capture helpers
- screenshot capture helpers
- snapshot/restore helpers

Read first:

- `tools/jsbeeb/src/machine-session.js`
- `tools/jsbeeb/tests/test-machine.js`
- `tools/jsbeeb/docs/snapshot-format.md`

Gate:
The harness can boot the SNG47 disc, take a screenshot, capture text output,
stop on breakpoints, read memory, write memory, and restore from a snapshot.

## 4. Define fixture conventions before subsystem ports

Outcome:
Every later module has a standard way to prove correctness.

Create:

- `tests/fixtures/reference/manifest.json`
- `tests/fixtures/reference/README.md`
- `tests/fixtures/visual/README.md`
- `scripts/fixtures/extract-fixture.ts`
- `scripts/fixtures/capture-scene.ts`

Fixture structure:

- `tests/fixtures/reference/rng/`
- `tests/fixtures/reference/galaxy/`
- `tests/fixtures/reference/market/`
- `tests/fixtures/reference/text/`
- `tests/fixtures/reference/movement/`
- `tests/fixtures/reference/tactics/`
- `tests/fixtures/reference/commander/`
- `tests/fixtures/visual/screens/`
- `tests/fixtures/visual/scenes/`

Every fixture record should include:

- reference disc and model
- source routine or routines
- setup method: boot path, snapshot, input sequence, or memory patch
- exact input bytes or state values
- exact expected output bytes, values, or image
- capture script used to produce it

Gate:
No core subsystem is implemented without at least one extraction path and one
fixture location already defined.

## 5. Implement fixed-width arithmetic and bit semantics

Outcome:
The port can reproduce 8-bit, 16-bit, carry, overflow, shift, and
sign-magnitude behaviour without leaking JavaScript number semantics.

Create:

- `src/core/arithmetic/u8.ts`
- `src/core/arithmetic/u16.ts`
- `src/core/arithmetic/bitwise.ts`
- `src/core/arithmetic/sign-magnitude.ts`
- `src/core/arithmetic/fixed-width.ts`
- `tests/unit/arithmetic/*.test.ts`

Responsibilities:

- masking to 8 and 16 bits
- carry and borrow helpers
- signed and unsigned interpretations
- rotate and shift behaviour
- sign-magnitude helpers for coordinate storage
- small utility wrappers that keep simulation code readable

Read first:

- `docs/bbcelite-master/main/subroutine/dornd.md`
- `https://elite.bbcelite.com/terminology/`

Gate:
Arithmetic tests cover wraparound, carry, sign-magnitude decode/encode, and
any other byte-level behaviour depended on by RNG, movement, combat, and saves.

## 6. Model workspaces, ship blocks, and commander data explicitly

Outcome:
The port has typed state structures that preserve original semantics without
keeping opaque memory blobs as the main runtime API.

Create:

- `src/core/state/workspaces/zp.ts`
- `src/core/state/workspaces/wp.ts`
- `src/core/state/workspaces/up.ts`
- `src/core/state/ships/k-block.ts`
- `src/core/state/ships/inwk.ts`
- `src/core/state/commander/commander-state.ts`
- `src/core/state/serialization/`
- `tests/unit/state/*.test.ts`

Use these local documents as the mapping source:

- `docs/bbcelite-master/main/workspace/zp.md`
- `docs/bbcelite-master/main/workspace/wp.md`
- `docs/bbcelite-master/main/workspace/up.md`
- `docs/bbcelite-master/main/workspace/k_per_cent.md`
- `docs/bbcelite-master/indexes/variables.md`

Implementation rule:
State objects should expose named fields and typed records, but also support
lossless conversion to and from byte layouts where fixtures and save data
require it.

Gate:
State structures round-trip cleanly through serializer/deserializer tests and
can represent all bytes needed by the first ported subsystems.

## 7. Port static game data and lookup tables

Outcome:
Ship blueprints, tokens, market tables, fonts, and other gameplay data exist as
typed assets or data modules in the port.

Create:

- `src/core/data/ship-blueprints.ts`
- `src/core/data/markets.ts`
- `src/core/data/tokens.ts`
- `src/core/data/system-name-tokens.ts`
- `src/core/data/dashboard.ts`
- `src/core/data/lookup-tables.ts`
- `src/core/assets/`

Primary sources:

- `docs/elite-source-code-bbc-master/1-source-files/main-sources/elite-data.asm`
- `docs/bbcelite-master/game_data/`
- `docs/bbcelite-master/indexes/variables.md`

Important constraint:
Represent the data needed for behaviour and presentation, but do not copy the
annotated prose from the reference corpus into the runtime codebase.

Gate:
The runtime can load ship blueprint data, token tables, and market tables from
typed modules with tests that confirm the data matches the extracted reference
fixtures.

## 8. Port RNG and deterministic world generation

Outcome:
The port can reproduce galaxy seeds, system generation, names, and market
outputs exactly.

Create:

- `src/core/world/rng.ts`
- `src/core/world/galaxy/seed.ts`
- `src/core/world/galaxy/system-generation.ts`
- `src/core/world/galaxy/system-names.ts`
- `src/core/world/market/market-generation.ts`
- `tests/reference/rng.test.ts`
- `tests/reference/galaxy.test.ts`
- `tests/reference/market.test.ts`

Local docs:

- `docs/bbcelite-master/main/subroutine/dornd.md`
- `docs/bbcelite-master/main/workspace/zp.md#rand`
- `docs/bbcelite-master/main/subroutine/gvl.md`
- `docs/bbcelite-master/main/subroutine/tt151.md`
- `docs/bbcelite-master/main/variable/qq23.md`

Online deep dives to use when the local mirror is not enough:

- `https://elite.bbcelite.com/deep_dives/`:
  "Galaxy and system seeds"
- `https://elite.bbcelite.com/deep_dives/`:
  "Generating system data"
- `https://elite.bbcelite.com/deep_dives/`:
  "Generating system names"
- `https://elite.bbcelite.com/deep_dives/`:
  "Twisting the system seeds"
- `https://elite.bbcelite.com/deep_dives/`:
  "Market item prices and availability"

Gate:
Known galaxy seeds, system names, descriptions, prices, and availabilities
match reference fixtures exactly.

## 9. Port geometry, projection, and low-level presentation primitives

Outcome:
The port has deterministic math and drawing primitives that later ship,
planet, sun, scanner, and dashboard renderers can depend on.

Create:

- `src/core/presentation/vector.ts`
- `src/core/presentation/arctan.ts`
- `src/core/presentation/projection.ts`
- `src/core/presentation/line-rasterizer.ts`
- `src/core/presentation/pixel-plotter.ts`
- `src/core/presentation/circle.ts`
- `tests/reference/projection.test.ts`
- `tests/reference/rasterizer.test.ts`

Read first:

- `docs/bbcelite-master/main/subroutine/arctan.md`
- `docs/bbcelite-master/main/subroutine/proj.md`
- `docs/bbcelite-master/main/subroutine/loinq_part_1_of_7.md`
- `docs/bbcelite-master/main/subroutine/hloin.md`
- `docs/bbcelite-master/main/subroutine/pixel.md`
- `docs/bbcelite-master/main/subroutine/circle2.md`

Gate:
Projection helpers and drawing primitives match reference outputs in isolated
tests before any full-scene renderer is written.

## 10. Port local-universe data structures and ship movement

Outcome:
The runtime can maintain ships in the local bubble and move them with the same
core semantics as the reference game.

Create:

- `src/core/simulation/universe/universe-state.ts`
- `src/core/simulation/universe/local-bubble.ts`
- `src/core/simulation/ships/spawn.ts`
- `src/core/simulation/ships/move-ship.ts`
- `src/core/simulation/ships/orientation.ts`
- `src/core/simulation/scanner/scanner-state.ts`
- `tests/reference/movement.test.ts`
- `tests/reference/local-bubble.test.ts`

Read first:

- `docs/bbcelite-master/main/variable/univ.md`
- `docs/bbcelite-master/main/subroutine/nwshp.md`
- `docs/bbcelite-master/main/subroutine/nwsps.md`
- `docs/bbcelite-master/main/subroutine/mveit_part_1_of_9.md`
- `docs/bbcelite-master/main/subroutine/mveit_part_9_of_9.md`
- `docs/bbcelite-master/main/workspace/zp.md#inwk`
- `docs/bbcelite-master/main/workspace/k_per_cent.md`

Online deep dives:

- `https://elite.bbcelite.com/deep_dives/`:
  "The local bubble of universe"
- `https://elite.bbcelite.com/deep_dives/`:
  "Ship data blocks"
- `https://elite.bbcelite.com/deep_dives/`:
  "Program flow of the ship-moving routine"
- `https://elite.bbcelite.com/deep_dives/`:
  "Orientation vectors"
- `https://elite.bbcelite.com/deep_dives/`:
  "Tidying orthonormal vectors"

Gate:
Ship spawn, update, and orientation transforms match the reference harness for
captured scenarios.

## 11. Port tactics, combat, collision, docking, and scooping

Outcome:
The port reproduces the decision-making and immediate interaction rules that
make spaceflight feel like Elite rather than a loose approximation.

Create:

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

Read first:

- `docs/bbcelite-master/main/subroutine/tactics_part_1_of_7.md`
- `docs/bbcelite-master/main/subroutine/tactics_part_7_of_7.md`
- `docs/bbcelite-master/main/subroutine/hitch.md`
- `docs/bbcelite-master/main/subroutine/dockit.md`
- `docs/bbcelite-master/main/subroutine/main_flight_loop_part_7_of_16.md`
- `docs/bbcelite-master/main/subroutine/main_flight_loop_part_11_of_16.md`

Online deep dives:

- `https://elite.bbcelite.com/deep_dives/`:
  "Docking checks"
- `https://elite.bbcelite.com/deep_dives/`:
  "The docking computer"
- `https://elite.bbcelite.com/deep_dives/`:
  "Program flow of the tactics routine"
- `https://elite.bbcelite.com/deep_dives/`:
  "In the crosshairs"

Gate:
Captured combat and docking scenarios produce the same state transitions,
targeting outcomes, and collision results as the reference machine.

## 12. Port the main flight loop

Outcome:
A single TypeScript orchestration path replaces the reference flight loop while
still delegating detailed behaviour to the already-ported subsystems.

Create:

- `src/core/simulation/flight/main-flight-loop.ts`
- `src/core/simulation/flight/flight-context.ts`
- `src/core/simulation/flight/flight-step.ts`
- `tests/reference/main-flight-loop.test.ts`

Read first:

- `docs/bbcelite-master/main/subroutine/main_flight_loop_part_1_of_16.md`
- `docs/bbcelite-master/main/subroutine/main_flight_loop_part_16_of_16.md`
- `docs/bbcelite-master/articles/map_of_the_source_code.md`

Implementation rule:
The TypeScript flight loop should be readable and modular. Preserve ordering and
observable effects, but do not preserve the original control-flow shape if a
clearer structure produces identical behaviour.

Gate:
Stepped flight-loop captures from the port and `jsbeeb` agree for curated
scenarios covering combat, docking, planet approach, and ordinary travel.

## 13. Port the main game loop and docked flows

Outcome:
The port can transition between in-flight and docked modes, spawn game events,
and run the top-level gameplay loop.

Create:

- `src/core/simulation/game/main-game-loop.ts`
- `src/core/simulation/game/scheduler.ts`
- `src/core/simulation/game/spawn-rules.ts`
- `src/core/simulation/game/view-state.ts`
- `tests/reference/main-game-loop.test.ts`

Read first:

- `docs/bbcelite-master/main/subroutine/tt170.md`
- `docs/bbcelite-master/main/subroutine/main_game_loop_part_1_of_6.md`
- `docs/bbcelite-master/main/subroutine/main_game_loop_part_6_of_6.md`
- `docs/bbcelite-master/main/subroutine/title.md`
- `docs/bbcelite-master/main/subroutine/doentry.md`

Online deep dives:

- `https://elite.bbcelite.com/deep_dives/`:
  "Program flow of the main game loop"
- `https://elite.bbcelite.com/deep_dives/`:
  "Scheduling tasks with the main loop counter"

Gate:
The top-level loop can enter play, transition to docked flows, and continue
event scheduling in the same order as the reference build.

## 14. Port economy, equipment, hyperspace, missions, and commander state

Outcome:
The non-flight gameplay systems become fully playable.

Create:

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

Read first:

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

Gate:
The player can trade, equip, hyperspace, progress missions, and save/load a
commander while matching captured reference data.

## 15. Port text systems and UI-facing token expansion

Outcome:
All generated and scripted text is produced by data-driven TypeScript rather
than by screen-memory-era printing logic.

Create:

- `src/core/presentation/text/token-expansion.ts`
- `src/core/presentation/text/goat-soup.ts`
- `src/core/presentation/text/formatting.ts`
- `tests/reference/text.test.ts`

Read first:

- `docs/bbcelite-master/main/subroutine/detok.md`
- `docs/bbcelite-master/main/subroutine/detok2.md`
- `docs/bbcelite-master/main/subroutine/detok3.md`
- `docs/bbcelite-master/main/variable/jmtb.md`
- `docs/bbcelite-master/game_data/variable/tkn1.md`
- `docs/bbcelite-master/game_data/variable/rutok.md`

Online deep dives:

- `https://elite.bbcelite.com/deep_dives/`:
  "Printing text tokens"
- `https://elite.bbcelite.com/deep_dives/`:
  "Extended text tokens"
- `https://elite.bbcelite.com/deep_dives/`:
  "Extended system descriptions"

Gate:
System names, market text, mission text, and generated descriptions match the
reference outputs for a representative fixture set.

## 16. Implement rendering adapters and scene renderers

Outcome:
The port can draw the game in the browser without reusing the emulator's video
pipeline.

Create:

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

Read first:

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

Implementation rule:
Canvas2D is the first adapter. Keep the renderer boundary clean enough that a
future WebGL or WebGPU renderer can replace it.

Gate:
Bounded scenes produce stable screenshots that pass pixel or perceptual
regression thresholds against captured reference images.

## 17. Implement browser input, audio, storage, and shell integration

Outcome:
The browser build is actually playable.

Create:

- `src/platform/browser/input/keyboard.ts`
- `src/platform/browser/input/joystick.ts`
- `src/platform/browser/audio/audio-engine.ts`
- `src/platform/browser/storage/commander-storage.ts`
- `src/app/game-shell.ts`
- `src/ui/screens/*.ts`
- `src/ui/hud/*.ts`
- `tests/integration/browser-shell.test.ts`

Read first:

- `docs/bbcelite-master/main/subroutine/shift.md`
- `docs/bbcelite-master/main/subroutine/return.md`
- `docs/bbcelite-master/main/subroutine/djoy.md`
- `docs/bbcelite-master/main/subroutine/rdjoy.md`
- `docs/bbcelite-master/main/subroutine/boop.md`
- `docs/bbcelite-master/main/subroutine/beep.md`
- `docs/bbcelite-master/main/subroutine/noise.md`
- `docs/bbcelite-master/main/subroutine/soint.md`
- `docs/bbcelite-master/main/workspace/sound_variables.md`

Gate:
Keyboard control, optional joystick support, audio cues, save persistence, and
screen transitions all work in the browser build.

## 18. Integrate startup, title flow, and user-facing product behaviour

Outcome:
The experience behaves like a product rather than a set of disconnected
subsystems.

Create:

- `src/app/bootstrap.ts`
- `src/ui/screens/title-screen.ts`
- `src/ui/screens/loading-screen.ts`
- `src/ui/screens/flight-screen.ts`
- `src/ui/screens/docked-screen.ts`
- `tests/integration/startup-flow.test.ts`

Read first:

- `docs/bbcelite-master/main/subroutine/title.md`
- `docs/elite-source-code-bbc-master/1-source-files/main-sources/elite-loader.asm`
- `docs/bbcelite-master/main/subroutine/hanger.md`

Important rule:
Preserve title-screen and hangar feel, but do not reproduce disc-loader
choreography or copy-protection mechanisms that existed only for the original
delivery medium.

Gate:
The browser build boots to a usable title flow, enters gameplay, and returns to
docked/title paths cleanly.

## 19. Expand automated evidence and perform side-by-side playtesting

Outcome:
The port is proven both mechanically and experientially.

Create and maintain:

- `tests/reference/acceptance/`
- `tests/visual/acceptance/`
- `docs/tasks/playtest-checklist.md`
- `docs/tasks/known-fidelity-gaps.md`

Automated checks:

- deterministic fixtures for core simulation
- visual regression scenes for dashboard, scanner, ships, sun, planet, and
  title/loading screens
- save/load round trips
- end-to-end flow tests for launch, docking, trade, hyperspace, combat, death,
  and save/load

Human checks:

- input feel
- movement feel
- docking feel
- combat feel
- audio feel
- general pacing and readability

Gate:
Every known fidelity gap is either fixed, intentionally accepted, or recorded
with a clear follow-up.

## 20. Package, document, and ship the browser port

Outcome:
The project can be built, tested, and run by someone other than the original
implementer.

Create or update:

- root `README.md`
- `docs/tasks/release-checklist.md`
- browser build output configuration
- CI scripts for tests and builds

Release requirements:

- one command for local development
- one command for the full test suite
- one command for reference extraction refresh
- one command for production build
- documentation that explains the reference corpus, legal constraints, and how
  the verification harness works

Gate:
Another engineer can clone the repository, install dependencies, run the
browser build, run the tests, and understand the relationship between the port
and the reference corpus.

## Order of Execution

Use this exact macro-order:

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

Do not move rendering, UI, or browser shell work ahead of deterministic core
simulation unless a small amount of shell scaffolding is required to keep the
project runnable.

## Definition of Done

The implementation is done when all of the following are true:

- the browser build is playable from title screen through ordinary gameplay
- deterministic subsystems pass fixture-backed reference tests
- rendering passes bounded visual regression checks
- save/load is compatible with the chosen commander-state rules for the port
- human side-by-side playtesting says the game feels like the reference build
- the codebase is typed, modular, and understandable without reading assembly
