---
layout: post
title: "FAIRy Notes #1: preflight before you submit"
date: 2025-12-16 13:07:03 -0700
categories: [fairy-notes, data, bioinformatics]
permalink: /bioinformatics/journey/:year/:month/:day/:title/
---

Most dataset submission pain isn't *hard* but annoying. Its often not the case that the science is wrong but that the data needs to be put into a certain format. That formatting for different receievers is the work that can be tedious when you are working with a dataset.

So from that idea, I kept coming back to, I decided to build a metadata manipulation tool. The idea is this:

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
These show up across domains — genomics, biodiversity, random CSV someone exported from a database...

### 1) IDs that aren’t stable
If an `id` column changes between exports, it breaks joins, annotations, deduping, and the prior fixes of the dataset from last run!

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

## Where FAIRy fits
FAIRy is my attempt to make this workflow easy: run a command locally, get a report that tells you what’s blocking, what’s risky, and what to fix next — without uploading raw data anywhere by default.

This is still early and evolving, but the *direction* is pretty stable: **turn validation into remediation.**

## Follow Along!

I’m figuring out my path in bioinformatics by exploring a mix of courses, hands-on projects, and challenges. Along the way, I’ve found some incredible resources that have helped shape my learning, and I plan to share them here along with my own projects and ideas.

If you're also navigating bioinformatics, machine learning, or AI, let’s learn and explore together! Feel free to reach out via [LinkedIn](https://www.linkedin.com/in/jenniferslotnick/), [GitHub](https://github.com/yuummmer), or my [contact page](/contact/). 🚀
