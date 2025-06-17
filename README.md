# Eldoraigne Automations & Chatbot Project

This repository contains all the project artifacts for the Eldoraigne Tennis Club's automation and chatbot system, managed via n8n.io.

## Quick Links

- **[Project Charter](./project_charter/Project_Charter_v6.md):** The single source of truth for the project's vision, architecture, and requirements.
- **[The AI's Constitution](./project_charter/AI_Constitution.md):** The core operating principles that govern the AI developer.
- **[Project Changelog](./project_charter/Changelog.md):** A running log of all version changes and key decisions.
- **[Architecture Diagrams](./architecture_diagrams/):** Visual representations of the system architecture.
- **[n8n Workflows](./n8n_workflows/):** Exported JSON files for the n8n workflows.

## Overview

This system uses a platform-agnostic adapter model. A lightweight **Telegram Adapter** workflow translates user messages into a standard format, which is then processed by a central **Core Logic Engine** workflow. This design allows for easy maintenance and future expansion to other platforms like WhatsApp.

- **State Management:** Handled by a Postgres SQL database.
- **Configuration:** Managed via Google Sheets for easy updates by non-coders.
- **LLM Provider:** Google AI (Gemini).

---

*This project is being developed in collaboration with an AI Solutions Architect (Google's Gemini Pro model).*
