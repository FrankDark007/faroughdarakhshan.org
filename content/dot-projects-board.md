---
title: "Dot-Projects-Board: a mission control dashboard that shows only verified state"
description: "Farough Darakhshan's Dot-Projects-Board: a TypeScript mission control dashboard for live projects that never shows demo data as real and never fakes a connection."
url: "/dot-projects-board/"
---

Dot-Projects-Board is a mission control screen for live projects: which workers are running, what needs attention, and which milestones are done. Farough (Frank) Darakhshan published it on 10/15/2026.

A dashboard can show a green light because a page loaded, not because the work happened. The rule this one is built around: never show something as true unless it was proven.

Repo: [github.com/FrankDark007/Dot-Projects-Board](https://github.com/FrankDark007/Dot-Projects-Board). Language: TypeScript. The integration guide is `INTEGRATION_README.md`.

## How it stays honest

- **Demo data says it is demo data.** Every synthetic screen carries a "SYNTHETIC DEMO DATA" label.
- **Live mode starts disconnected.** If a poll fails, times out or returns bad data, the screen says so. It never falls back to sample data.
- **Three kinds of freshness, checked separately.** Did the server answer? Is the snapshot it sent recent? Is each worker's own report recent? A server that keeps serving an old cached snapshot is marked stale even though it answers.
- **"Verified Complete" needs evidence.** A milestone counts as done only when every required gate has passed with a timestamp that is not in the future and a linked piece of evidence marked verified.
- **Writes are never guessed.** Operator actions go out in one idempotent envelope. If a write times out and the outcome is unknown, the screen does not retry it on its own.

It was built and tested in a sandbox. The guide says plainly that the live connection to my own machines is not yet verified.

## Running it

`npm install`, then `npm run dev`. Checks:

- `npx tsx src/tests/runTests.ts`: 18 tests, 154 assertions
- `npm run lint` and `npm run build`
- `npx tsx src/tests/verifyLayoutBrowser.ts`: 37 layout states from 320 to 1920 px wide and at 200% zoom, in headless Chromium

Back to [all public code](/).
