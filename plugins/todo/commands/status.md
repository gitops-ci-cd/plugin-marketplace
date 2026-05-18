---
description: Get a quick read on where things stand — what's done, what's next, what's stale
disable-model-invocation: true
---

Give me a snapshot of my current workload:

1. Fetch all tasks
2. Display them in three groups:
   - **Next up** — incomplete tasks, most recently touched first (limit 5)
   - **Done today** — tasks completed in the last 24h
   - **Going stale** — incomplete tasks untouched for 7+ days (if any)
3. End with: "X of Y tasks completed. [one-sentence observation about workload health]"
