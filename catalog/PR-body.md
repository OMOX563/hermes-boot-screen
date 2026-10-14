# Catalog submission — `plugin-catalog/hermes-boot-screen.yaml`

Adds one entry to the Hermes plugin catalog:

```yaml
name: hermes-boot-screen
repo: https://github.com/OMOX563/hermes-boot-screen
sha: "4bdfb3c67cd8341c3018631214b8c42d7cf50cb7"
tier: community
```

## What the plugin does

A six-act boot animation for the Hermes desktop app — it takes over the first
screen before the workspace mounts:

1. **Act 1** — black screen, brand glyph reveal with spacing tighten + light sweep
2. **Bridge** — over-exposed whiteout (black → grey → warm white plate)
3. **Act 2** — a warm-white "technical dossier" plate: brand block wipe, progress bar
4. **Sequence** — access permission → identity verification → request received →
   read complete → verified → welcome `<identity>` (each step pairs a big headline
   with a mono subtitle)
5. **Controls** — Esc or the SKIP badge lifts the curtain at any time; steps 1–2
   wait for confirmation via button, Enter, Space or click
6. **Finale** — WELCOME TO / [HERMES DESKTOP] / glyph with a chromatic split

- **Desktop-only.** `desktop/plugin.js` inside the plugin SDK surface; the
  agent-side `__init__.py` is a no-op stub. Registers no tools, hooks,
  middleware or env vars.
- **Nothing leaves the machine.** No network calls at all — every asset (audio,
  glyph, stylesheet) is embedded in the single-file artifact. No shell, no
  background processes, no telemetry. Config (identity name, playback mode,
  speed, sound) lives in the plugin's own storage.
- **No self-updating code.** Static `desktop/plugin.js`; updates reach users only
  through a SHA-bump PR here.
- **Fully English** UI, README and build script.
- Confirm keys (Enter / numpad Enter / Space / Esc) are bound in the **capture**
  phase, so the chat composer — where Enter sends a message and stops
  propagation — cannot swallow them.

## Disclosure (rule 13)

- Network calls: **none**.
- Reads outside the plugin's own data: none.
- Shell commands / long-running background processes: none.
- Stored credentials: none — the plugin asks for no keys and holds no secrets.
- Telemetry / usage reporting: none.
- **Lineage:** English edition of `hermes-startup-screen`
  ([OMOX563/hermes-startup-screen](https://github.com/OMOX563/hermes-startup-screen)),
  which is itself adapted from
  [6shenhonghong9/dsh-startup-screen](https://github.com/6shenhonghong9/dsh-startup-screen)
  (MIT © 沈宏宏). Same engine, fully English UI; upstream credited in both READMEs
  and LICENSEs.

## Submission checklist

- [x] Owner-submitted (`OMOX563` owns the repo)
- [x] Public repository
- [x] Released (`v1.1.1` + GitHub release; `version` matches the pinned code)
- [x] Capabilities declared: none — registers no tools, hooks or middleware;
      `requires_env` absent
- [x] `hermes plugins validate` passes on a clean clone at the pinned SHA:
      manifest ✓ loadable ✓ capability probe ✓ **security scan safe** ✓
      no core override ✓ desktop surface ✓
- [x] Smoke suite green at the pinned SHA
- [x] No self-updating code; no core overrides; SDK-surface only

## Verify locally

```bash
hermes plugins install OMOX563/hermes-boot-screen --enable
hermes plugins validate ~/.hermes/plugins/hermes-boot-screen
```

Repo layout: `plugin.yaml` + agent-side `__init__.py` stub +
`desktop/plugin.js` (byte-identical to the released root `plugin.js`, emitted by
`build.py`).