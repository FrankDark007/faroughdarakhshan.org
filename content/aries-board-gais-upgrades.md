---
title: "Aries-Board--GAIS-upgrades: a redesigned operations board"
description: "Farough Darakhshan's Aries-Board--GAIS-upgrades: a redesigned interface for the Aries operations board and shared to-do list, with a local preview server."
url: "/aries-board-gais-upgrades/"
---

This repo is a redesign of the [Aries operations board](/aries-cloud-dashboard/), the page my team uses to see and act on my assistant's task list. Farough (Frank) Darakhshan published it on 10/16/2026 as a repo separate from the board in use.

Repo: [github.com/FrankDark007/Aries-Board--GAIS-upgrades](https://github.com/FrankDark007/Aries-Board--GAIS-upgrades). Languages: JavaScript and TypeScript. There is no README yet; this page is the documentation.

## What changed from the original board

- **A workspace-style interface** in React, laid out like an issue tracker, with claim groups, cards and a mission view.
- **More teammate actions.** Beyond done, note, assign and snooze, the queue accepts reopen, reorder, delete, nudge, prep, urgent, and "useful" or "not useful" feedback on an item.
- **Safer delivery.** Each queued command carries a lease, an attempt count and a next-attempt time, so a command that fails is retried on a schedule instead of being lost or applied twice.

The Cloudflare Worker and D1 schema stay the backend.

## Running it locally

`npm install`, then `npm run dev`. The dev server runs the Worker code against an in-memory stand-in for D1, seeded with sample items, and serves the interface through Vite. Nothing reaches Cloudflare.

- `npm test` runs the Node test suite in `test/`.
- `npm run lint` type-checks with `tsc --noEmit`.
- `npm run build` produces the static bundle.

Back to [all public code](/).
