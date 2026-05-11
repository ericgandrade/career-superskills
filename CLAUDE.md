# CLAUDE.md

This file provides guidance to Claude Code when working in this repository.

## Project Overview

**career-superskills** is a focused AI skills package for job search and career development. Part of the Superskills family.

- **npm package**: `career-superskills` (v1.0.2) — `npx career-superskills` — **20 skills**
- **Claude Code plugin**: `claude --plugin-dir ./career-superskills`
- **GitHub**: `https://github.com/ericgandrade/career-superskills`

## Skills (20)

Resume skills, job search skills, portfolio & presence skills. See README.md for full list.

## Version Management

Current version: **v1.0.2**. Use `node scripts/release.js [patch|minor|major]` to bump.

## Publishing

Publishing is automated via GitHub Actions on `v*` tag pushes.

## Language

All content in this repository — code comments, documentation, commit messages, skill files, and AI instructions — must be written in **English**. No Portuguese or any other language.

## graphify

This project has a graphify knowledge graph at graphify-out/.

Rules:
- Before answering architecture or codebase questions, read graphify-out/GRAPH_REPORT.md for god nodes and community structure
- If graphify-out/wiki/index.md exists, navigate it instead of reading raw files
- After modifying code files in this session, run `graphify update .` to keep the graph current (AST-only, no API cost)
- **After any important change, run `/graphify --update` before ending the session.** Important changes include: adding or removing a skill, modifying SKILL.md or README.md of any skill, changing CLI logic in cli-installer/, bumping version, updating bundles.json, or any structural refactor. Do not skip this step — an outdated graph leads to stale answers in future sessions.
