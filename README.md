# JP Assist

JP Assist is a multi-game Japanese learning platform. It combines a shared
account and FSRS review service, reusable vocabulary identity, public game
courses, and optional in-game study adapters for supported recompilations.

## Repository layout

- `apps/learning-platform/` — website, account API, catalog, FSRS, and Anki interoperability.
- `packages/study-mod-sdk/` — versioned C ABI implemented by compatible game hosts.
- `packages/study-core/` — portable Study Mode state and account synchronization.
- `packages/game-module-contract/` — discoverable game-module schema.
- `games/ocarina-of-time/` — OoT catalog, course, pipeline metadata, tools, and Shipwright plugin.
- `docs/` — architecture, deployment, security, content, and interoperability guides.

Shipwright and libultraship remain separate upstream-tracking forks. They are
integration targets, not owners of the learning platform.

## Run the local site

```bash
python3 -m venv .venv
. .venv/bin/activate
pip install -r apps/learning-platform/requirements.txt
PYTHONPATH=apps/learning-platform \
  uvicorn learning_platform.main:app --reload --port 8766
```

Open <http://127.0.0.1:8766>.

## Build the portable core and OoT plugin

```bash
cmake -S . -B build
cmake --build build --target jpassist_plugin_package jp_assist_study_core_tests
ctest --test-dir build --output-on-failure
```

The plugin package is written to
`build/games/ocarina-of-time/adapters/shipwright/plugin/jp-assist.o2r`.
It requires a Shipwright build implementing the Study Mod host API.

## Add a game

Create `games/<game-id>/module.json` following the schema in
`packages/game-module-contract`. Catalog discovery is data-driven; adding a
module does not require hard-coded website or API registration.

Extracted dialogue and ROM-derived assets are local build inputs and must not
be committed or distributed.
