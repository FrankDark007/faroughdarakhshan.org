---
title: "aries-cloud-dashboard: a team board on Cloudflare Workers and D1"
description: "Farough Darakhshan's aries-cloud-dashboard: a Cloudflare Workers and D1 board that publishes an assistant's task list to a team and queues their actions."
url: "/aries-cloud-dashboard/"
---

Aries is the assistant that runs on its own Mac mini. Its task list used to be visible only on that machine. This repo, published by Farough (Frank) Darakhshan, puts a last-known copy of the list on Cloudflare so my team can read it and act on it, even when the Mac mini is offline.

Repo: [github.com/FrankDark007/aries-cloud-dashboard](https://github.com/FrankDark007/aries-cloud-dashboard). Language: JavaScript. Created 9/5/2026.

## What it does

- **Board.** Aries publishes a curated snapshot of its open items. Teammates see it in a browser and can mark items done, add a note, assign, or snooze.
- **Command queue.** A teammate's action does not change the board directly. It waits in a queue until Aries pulls it, applies it, and acknowledges it as applied or failed.
- **Shared to-do list.** A catalogue of tasks plus an append-only log of every change. The log is the authoritative record of what the team did.

## How it is built

One Cloudflare Worker and one D1 database. Two hostnames:

- The **people host** sits behind Cloudflare Access, and the Worker checks the signed-in email against an exact allowlist on every route.
- The **machine host** accepts only the sync routes, with a bearer token stored as a Wrangler secret.

Every other hostname returns 404. The `workers.dev` and preview URLs are off. A health route returns only `{"ok":true}`.

## Rules the Worker enforces

- A snapshot keeps ten known fields and drops everything else. It is rejected whole if an id repeats, an item is malformed, it holds more than 1,000 items, it exceeds 1.8 MB, or any item is marked personal.
- Commands are delivered at least once until acknowledged, so the consumer applies them by command id.
- File attachments are stored as metadata only. File bytes never enter the database.

## Running it

Copy the Wrangler template, fill in the D1 id and the allowlist, then run `npm install`, apply `schema.sql` to the remote database, set the publisher token as a secret, and deploy. Put Cloudflare Access on the people host with the same email list.

Back to [all public code](/).
