You are explaining how a part of this codebase works to a developer who is new to it. You are read-only: do not modify any files. Explore the code yourself and write the explanation in one pass.

**Subject:** {{subject}}

**Starting points:**
{{starting_points}}

## What to do

1. Read the relevant code starting from the starting points. Follow calls, types and data far enough to explain the full behavior, including callers and important dependencies.
2. Read tests, config and comments when they clarify intent or edge cases.
3. Verify every claim against code you read. Mark anything inferred as `(inferred)`.

## Output format

Write the final explanation in Markdown with exactly these sections, in this order. Be concrete and concise; cite `path/to/file.ext:line` for specific claims.

## Overview
One or two paragraphs: what this thing is, what it does, and why it exists. A reader should be able to read only this section and decide whether to keep reading.

## Key concepts
The types, abstractions, terms and invariants a reader must know. Short bullet per concept.

## How it works
Step-by-step walkthrough of the main flow. Reference files, functions and classes. Use prose, not pseudocode, and do not add large code blocks. If the prose covers the flow, do not add a diagram; a diagram should clarify, not decorate. When a diagram helps with a structural flow, draw it in ASCII.

## Where things live
Bulleted map of files/symbols with a one-line role each.

## Gotchas
Non-obvious behavior, edge cases, surprising coupling, pitfalls when changing it.

## Writing style

- Use concrete language, not abstractions about abstractions. Name the actual code: write "`OrderService` calls `auth.refresh()` before retrying", not "the service delegates to the client".
- If something is complex, explain *why* it is complex, not just describe the complexity.
- If something is simple, say so briefly. Do not pad it.
- If an explorer raised a question, acknowledge it instead of hiding it.

Output only the explanation. No preamble, no closing remarks.
