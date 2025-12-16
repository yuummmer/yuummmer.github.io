---
layout: post
title: "FAIRy Notes #1: preflight before you submit"
date: 2025-12-16 13:07:03 -0700
categories: [fairy-notes, data, bioinformatics]
permalink: /bioinformatics/journey/:year/:month/:day/:title/
---

> *“My philosophy on technology is that people shouldn’t have to understand it in order to be able to use it.”*
> — *Radia Perlman*

Most dataset submission pain isn't *hard*, it's just annoying. Its usually not that the science is wrong. It's that the same dataset has to be shaped into a slightly different format for each receiver: a repository, a curator, a collaborator, a tool. And when you’re already deep in research mode, that “just put it in the right format” work can feel like needless friction.

I kept coming back to the same idea, so I decided to build a small metadata manipulation tool. The idea is this:

**Run a tiny preflight locally before you submit.**  
Not to make your data perfect — just to catch the preventable problems early and make fixes less miserable.

## What I mean by “preflight”
A preflight is a quick pass over your dataset (usually the metadata table) that produces:

- **Pass / Warn / Fail** results (so you know what’s blocking vs. what’s “fix when you can”)
- A **human-readable report** you can share (or keep as a record)
- **Remediation hints** (what broke + what to do next)
- Optionally: a small “bundle” of artifacts (reports, manifests, provenance notes) so you can reproduce what you checked later

I’m building FAIRy around this  model: *validation is only useful if it helps you fix things.*

## The 5 boring issues that cause most chaos
These show up across domains — genomics, biodiversity, and every random CSV someone exported from a database.

### 1) IDs that aren’t stable
If an `id` column changes between exports, it breaks joins, annotations, deduping, and even your own “what did I fix last time?” notes.

**Preflight check:** ID is present, unique, and doesn’t contain obvious formatting weirdness (like spaces).

**Fix:** choose a stable key strategy, or generate one once and persist it.

### 2) Dates that are “valid” but wrong
A date can parse fine and still be suspicious (defaults like `2025-01-01`, weird placeholders, or mixed formats).

**Preflight check:** parseable date and optional heuristics (e.g., warn on implausible ranges or on obvious defaults).

**Fix:** normalize to one format and document what the field means (collection date vs upload date vs observation date).

### 3) URLs that aren’t URLs
Classic: `www.example.com` without a scheme, or “URL-looking strings” that are actually notes.

**Preflight check:** if a field is supposed to be a URL, require a real URL shape (`http://` or `https://`) and flag the rest.

**Fix:** add the scheme, or move the text into a notes field.

### 4) Numeric fields that drift out of reality
Latitude and longitude swapped, out-of-range coordinates, negative mass, impossible dimensions.

**Preflight check:** type and range checks (and warn if something looks swapped).

**Fix:** normalize units, confirm coordinate order, add constraints closer to the source system.

### 5) “Row number” confusion
One of the most annoying little bugs: reporting row numbers that don’t line up because of headers (or 0 vs 1 indexing).

**Preflight check:** not a data rule, just a UX rule: make error locations match what users see in Excel/Sheets.

**Fix:** always be explicit about whether row numbers include headers.

## How someone else could use this idea (even without FAIRy)
If you’re a…

- **Researcher:** run preflight before you upload, so you don’t learn about basic issues from a rejection email.
- **Curator/reviewer:** share a preflight checklist (or rulepack) so feedback is consistent and less bespoke.
- **Tool builder:** treat rulepacks like “lint rules” for datasets — something the community can version and improve.

## The vision
I want dataset submission to feel more like running tests on code: quick, local, and obvious about what to fix.
The point is cutting down preventable back-and-forth so curators and researchers spend their time on the work they’re actually trained to do.
FAIRy runs locally, applies a shared set of requirements (“rulepacks”), and produces something you can act on: a clear fix list.
Over time, I want these rulepacks to be reusable and updated to community standards.

## Where FAIRy fits
FAIRy is my attempt to make this workflow easy: run a command locally, get a report that tells you what’s blocking, what’s risky, and what to fix next — without uploading raw data anywhere by default.

This is still early and evolving, but the *direction* is pretty stable: **turn validation into remediation.**

## Follow Along

I’m sharing these **FAIRy Notes** as I build a local-first “preflight” for research datasets — the boring checks that save time later.

If you want to check it out:
- FAIRy (code + issues): [github.com/yuummmer/fairy-core](https://github.com/yuummmer/fairy-core)
- More on the project (and the institutional side) lives at: [datadabra.com](https://datadabra.com)

And if you’re working with messy metadata (genomics, biodiversity, anything tabular) and have a “this always breaks” example, I’d love to hear it. You can reach me on [LinkedIn](https://www.linkedin.com/in/jenniferslotnick/) or [GitHub](https://github.com/yuummmer).
