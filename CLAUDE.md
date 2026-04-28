# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static single-page website for Italian high school students preparing the *Esame di Stato* in **Sistemi e Reti** (ITIS — ITIA / ITTL articulation). No build step, no framework, no dependencies.

## Structure

```
index.html          # Entire site: HTML + CSS + JS in one file
prove-esame/        # 15 official exam PDFs (local, committed to repo)
```

## Development

Open `index.html` directly in a browser — no server needed for local development. To simulate a real server (required for PDF downloads to work correctly in some browsers):

```bash
python3 -m http.server 8080
# then open http://localhost:8080
```

## Architecture of index.html

All CSS, JS, and HTML are inlined in a single file. Key sections:

- **CSS variables** (`:root`) at the top — all colours and sizing use these; edit here to retheme.
- **Sidebar** (`#sidebar`) — sticky nav with scroll-spy driven by JS. Nav links mirror section `id` attributes.
- **Search** (`#search`) — filters `.topic-item` elements and `.section-title` text live; auto-opens matching sections; supports `/` shortcut to focus, `Esc` to clear.
- **Accordion sections** (`.section`) — toggled via `toggleSection()`. Each has a `.section-header` and `.section-body`.
- **Frequency table** (`#freq-tbody`) — built from the `freqData` array in JS at page load.
- **Checklist** (`#checklist-container`) — built from `checklistData` array; state persisted in `localStorage` under key `ser_checklist`.
- **PDF download section** — links point to `prove-esame/<filename>.pdf` (relative path). The "Scarica tutti" button loops through `.pdf-link` anchors with a 400 ms delay between clicks.
- **Resource cards** (`.res-card`, `.video-card`) — static HTML; all external URLs were manually verified as reachable.

## PDF files

PDFs in `prove-esame/` are official MIM/MIUR exam papers for *Sistemi e Reti* (sessions 2015–2024). When adding new exams, also add the corresponding row in the PDF download section of `index.html`.

## Deployment

Deployed on Vercel from the `main` branch of `github.com/lightoshadow/guida-sistemi-e-reti`. Push to `main` triggers an automatic redeploy. No `vercel.json` is needed — Vercel serves the root as a static site.
