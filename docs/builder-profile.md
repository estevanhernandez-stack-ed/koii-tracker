# Builder Profile

> Generated 2026-05-09 by `/vibe-cartographer:onboard` in autonomous-run mode.
> Every value below is either pulled from `~/.claude/profiles/builder.json`
> (the unified cross-plugin profile) or defaulted from the project's existing
> artifacts (README, index.html, the just-completed Sanduhr theming plant).
> Defaulted values are marked `(default — confirm on next interactive run)`.

## Who They Are
Estevan ("Mr. Solo Dolo"). Builder and outsider, runs 626Labs out of Fort Worth, TX.
20+ years on PC/Windows. Vibe coder — architects and ships through AI agents
rather than writing code directly. Strong pattern recognition, troubleshooting,
and systems thinking. Has shipped ~10 deployed apps including a 7-app enterprise
suite under serious consideration for company deployment, three Microsoft Store
apps (as of April 2026), and the 626labs.dev portfolio site (launched 2026-04-24).
Active Vibe Cartographer plugin contributor — modified the plugin for company
use; LADDER (April 2026 hackathon) was the retooled-plugin stress test.

What brings him to this project: Koii Tracker started as a single-page Pet Sim 99
event tracker for the Koii clan — a personal-use shipping-fast tool. Today's
session pulled the Sanduhr theming system into it, so the project earns formal
ecosystem registration.

## Technical Experience
Level: **experienced**.

Languages: TypeScript, Python, JavaScript, Luau, C#, HTML/CSS, C++.

Frameworks / tools: React 19, Next.js, Vite, TailwindCSS, Firebase, FastAPI,
Flask, Express, .NET 8/9, Azure, Expo, React Native, Drizzle ORM, Playwright,
WPF, C++/WinRT, Windows App SDK / WinUI 3, MSIX / wapproj, Ollama, Gemma 4
(E4B + 26B-A4B).

AI coding agent experience: Deep. Built and shipped Claude Code plugins
(Vibe Cartographer, Vibe Doc, Vibe Test) to marketplace. Runs Claude Code as
an autonomous build system with structured checklists and subagent delegation
across 6+ Cart cycles (LADDER, Sanduhr, Vibe Test, RTClickPng, 626Labs Universe
Deep Dive, Lab Backbone Step 1).

## Mode
**Builder** (from profile — `plugins.vibe-cartographer.mode = "builder"`).
Streamlined flow, no extended explanations. Autonomy level on file is
`fully-autonomous` (promoted 2026-04-26 after 9 completed Cart cycles).

## Persona
**Architect** (from `shared.preferences.persona`). Big-picture, tradeoff-focused.
Surfaces long-term implications. The voice that fits Koii Tracker's role as a
member of a growing 626Labs ecosystem rather than a one-off scratch tool.

## Project Goals
Koii Tracker is a single-page Pet Sim 99 event-strategy + clan-standings
dashboard for the Koii clan. The shipping bar is *fast and useful, not perfect*:

1. **Live ammo for clan strategy** — leaderboard pulled from `ps99.biggamesapi.io`,
   per-clan lookup with contribution ranks, prize-tier projection.
2. **Per-event playbook** — strats, decision matrix, daily checklist, pitfalls,
   quick reference, all in one editable `EVENT_CONFIG` block at the top of
   `index.html`. Re-skinning the tracker for the next event is a config edit.
3. **Personal grind log** — local-first roll counters, combo PRs, persisted
   per-event in `localStorage`.
4. **Now: visual register switching** — 7-theme palette system ported from
   Sanduhr. Default Koii (light, warm) plus six dark themes (Obsidian, Aurora,
   Ember, Mint, Matrix, Blueprint, 626 Labs).

Success looks like: any future event drops, the tracker is config-edited for it
in <30 minutes, the clan uses it for the duration of the battle, and the theme
picker means the tracker fits whatever lighting context the player is in.

## Project Origin
**Extending an existing repo.** Koii Tracker was bootstrapped before this Cart
cycle as a working single-page app. The cycle is therefore retrofit-shaped —
`/spec` (if/when run) pulls from the existing inline-CSS-and-JS structure in
`index.html` rather than designing from scratch. `/build` integrates into the
existing single-file shape; no framework introduction.

## Design Direction
Two-layer:

1. **Default / "Koii" theme** — warm light palette. Cream background (`#fff7ef`),
   orange accent (`#ff6b35`), brown ink. The sub-brand identity for Koii clan.
   Lives in `EVENT_CONFIG.brand`, overridable per event.
2. **Sanduhr-ported dark themes** — Obsidian, Aurora, Ember, Mint, Matrix,
   Blueprint, 626 Labs. Activated via `[data-theme]` on `<html>` plus a `.dark`
   class for surface fixes. Persisted in `localStorage` under `koii.theme`.
   FOUC-guarded with an inline `<script>` in `<head>`.

Sensibility (from profile): *clean, functional, high-contrast. Polish but not
at the expense of shipping. Won't ship a broken-looking tile even if the rest
works.*

## Prior SDD Experience
**Deep.** Active Cart contributor with 9+ completed cycles on file. Has run
the full /onboard → /scope → /prd → /spec → /checklist → /build chain
multiple times. Reflective practice is the norm — `/reflect` and `/evolve`
both well-trod. SDD calibration: skip the explanations, expect the artifacts.

## Architecture Docs
**No project-local architecture docs provided.** Koii Tracker is a single-file
HTML app — the architecture *is* the file. The Sanduhr theming plant introduced
two new concerns:

- **Theme registry** — JS array at the top of the IIFE in `index.html`,
  `THEMES = [{id, name, mode}, ...]`. Mirrors Sanduhr's `THEMES` dict / Swift
  `ThemeRegistry`.
- **Theme contract** — see `~/.vibe-taker/library/sanduhr-theming/CONTRACT.md`
  for the canonical 15-field schema, optional fields, and Sanduhr↔Koii token
  mapping.

If/when this project grows past a single file, `/spec` defaults apply.

## Deployment Target
**Static / file-based — currently no formal deploy** `(default — confirm on next interactive run)`.
The repo's index.html runs straight from `file://` or any static host
(GitHub Pages eligible). CORS is open on `ps99.biggamesapi.io`, so origin
doesn't matter. If shipped via GitHub Pages, the `koii-tracker` repo on
`github.com/estevanhernandez-stack-ed` is already linked to the dashboard project.
