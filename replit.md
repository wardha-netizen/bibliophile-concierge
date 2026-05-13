🌿 Bibliophile Concierge
An enchanted, AI-powered library management system landing page built with pure HTML/CSS.

Project Overview
This is a static HTML showcase and technical documentation portal for the Bibliophile Concierge system—a university project featuring four complex n8n automation workflows designed to modernize library management and community engagement.

System Architecture
Frontend: Static documentation suite (HTML5 / CSS3) featuring responsive architecture diagrams.

Automation Engine: Four integrated n8n workflows handling data ingestion, multilingual processing, and automated communication.

Persistence Layer: Dual-database strategy using Airtable as the primary vault and Google Sheets for real-time redundancy.

Key Files
index.html — Main landing page featuring the hero section, workflow logic cards, and team credits.

architecture.html — Detailed visual breakdown of the 15+ integrated services.

01-acquisition-multilingual-librarian.json — Workflow for book intake and metadata enrichment.

02-inventory-loan-manager.json — Logic for managing community loans and tracking.

03-engagement-semantic-search.json — AI-driven search and discovery workflow.

04-whatsapp-team-hub.json — Automated communication hub for Discord and WhatsApp.

Core Logic & Features
Multilingual Detection: Custom JavaScript nodes for tri-lingual detection (Arabic, Urdu, English) to route metadata correctly.

Metadata Enrichment: Synchronous API calls to Google Books and Open Library for automated data filling.

Omni-channel Alerts: Real-time notification system using WhatsApp Cloud API and Discord Webhooks.

Viewing Locally
To view the technical showcase locally, you can use any static web server:

Bash
# Example using Python
python3 -m http.server 5000
Deployment
The system is designed to be hosted as a static documentation portal, with the automation backend currently configured for self-hosted n8n environments or cloud providers like Railway.

Tech Stack Showcased
Automation: n8n (Self-hosted)

Intelligence: Groq AI (Llama 3.3 70B)

Productivity: Airtable, Google Sheets, Gmail, Google Calendar, Google Drive

Communication: Discord Webhooks, WhatsApp Cloud API

External APIs: OpenWeatherMap, GitHub REST API, Google Books API
