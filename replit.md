# Bibliophile Concierge

An enchanted, AI-powered library management system landing page built with pure HTML/CSS.

## Project Overview

This is a static HTML showcase/landing page for the "Bibliophile Concierge" project — a university project featuring 4 n8n automation workflows for library management.

## Architecture

- **Type**: Static HTML site (single `index.html`)
- **No build system** — pure HTML + CSS, no JavaScript framework
- **No backend** — fully client-side static content

## Key Files

- `index.html` — Main landing page (hero, workflow cards, architecture diagram, tech stack, team section)
- `01-acquisition-multilingual-librarian.json` — n8n workflow 1 export
- `02-inventory-loan-manager.json` — n8n workflow 2 export
- `03-engagement-semantic-search.json` — n8n workflow 3 export
- `04-whatsapp-team-hub.json` — n8n workflow 4 export
- `README.md` — n8n setup guide
- `architecture.html` — Architecture diagram page

## Running Locally

Served with Python's built-in HTTP server on port 5000:
```
python3 -m http.server 5000 --bind 0.0.0.0
```

## Deployment

Configured as a **static** deployment with `publicDir: "."`.

## Tech Stack Showcased

- n8n (self-hosted automation)
- Groq AI (Llama 3.3 70B)
- Airtable, Google Sheets, Gmail, Google Calendar, Google Drive
- Discord Webhooks, WhatsApp Cloud API
- OpenWeatherMap, GitHub REST API, Google Books API
