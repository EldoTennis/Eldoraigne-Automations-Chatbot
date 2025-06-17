# Eldoraigne Automations & Chatbot: Project Charter

**Version:** 6.0
**Last Updated:** 26 June 2025

---

## 1. Project Vision & Purpose

### 1.1. My Purpose (The AI's Mandate)
My purpose is to act as a Senior Solutions Architect and AI Developer, operating as an instance of Google's Gemini Pro model. I am tasked with assisting the project lead (the user) in designing, building, and refining a robust, stateful conversational AI engine for the **Eldoraigne Automations & Chatbot** project. My primary responsibilities are to:
- Maintain and update this Project Charter as the single source of truth.
- Translate formal "Business Requirements" into updated "Functional Requirements," "Architecture," and "Data Schemas."
- Propose stable, scalable, and maintainable architectural patterns that align with the project's goals and constraints.
- Provide complete, unabridged artifacts, such as n8n JSON workflows, Mermaid diagrams, and explicit configuration schemas.
- Document all decisions, failures, and learnings in the Project History & Changelog.

### 1.2. Project Goal
To create an automated, conversational interface for the Eldoraigne Tennis Club that provides distinct experiences for three user groups: Committee Members, Club Members, and the Public (Non-Members). The system will leverage n8n.io for workflow automation, Google Workspace for data and configuration, and a Large Language Model (LLM) for generative tasks, ultimately aiming to reduce administrative overhead and improve engagement.

### 1.3. Success Criteria
- The system can successfully identify a user's role and present them with the correct, role-specific menu.
- The system's configuration can be updated by editing a Google Sheet, without modifying the core n8n workflow.
- All project artifacts are version-controlled in the central GitHub repository.

---

## 2. Business Requirements

- **BR-001: Core Mission:** The system shall provide automated and conversational capabilities for the Eldoraigne Tennis Club.
- **BR-002: User Segmentation:** The system must identify and segment users into `committee`, `member`, and `unknown` roles with role-specific menus.
- **BR-003: Core Feature:** Committee members must have a feature to generate communications for social events.
- **BR-004: Maintainability:** The system must be administered by a non-coder via Google Sheets.
- **BR-005: Platform Strategy:** The initial interface is a Telegram chatbot, but the architecture must be modular to support future platforms.
- **BR-006: Technology Stack:** The system will use a self-hosted n8n.io instance, a Postgres SQL database, and Google Workspace.
- **BR-007: Version Control:** All project artifacts must be stored and version-controlled in a central GitHub repository.

---

## 3. Core System Architecture

### 3.1. Architectural Pattern: The "Platform-Agnostic Adapter" Model
The system uses two types of n8n workflows: **Adapters** (platform-specific) and a **Core Engine** (platform-agnostic logic).

### 3.2. High-Level Design (Component Diagram)
The canonical version of this diagram is maintained as a Mermaid `.mmd` file in the GitHub repository under `/architecture_diagrams/`.

### 3.3. Code Repository & Documentation
- **Code & Artifact Repository:** GitHub.com is the central repository for all version-controlled project artifacts.
- **Operator's Manual:** The GitHub Wiki serves as the primary documentation for the system's day-to-day administrator.
- **AI Operating Principles:** The `AI_Constitution.md` file defines the rules the AI developer must follow.
- **Project History:** The `Changelog.md` file documents the project's evolution.

### 3.4. Technology Stack & Credentials Store
- **Workflow Automation:** n8n.io (Community Edition, Self-Hosted)
- **State Management:** Postgres SQL Database
- **Configuration & Knowledge:** Google Sheets / Google Docs
- **LLM Provider:** Google AI (Gemini Pro)
- **Primary Chat Interface:** Telegram
- **Credentials Management:** Sensitive credentials will be stored exclusively within n8n's encrypted Credentials store.

---

## 4. Development & Release Cycle

### 4.1. Development Phase (Conversational)
New `Business Requirements` are provided by the Project Lead. The AI Solutions Architect will update the Project Charter and propose technical solutions within the chat for rapid iteration.

### 4.2. Release Phase (Formal)
Upon the command **"This is approved. Prepare the release for commit,"** the AI will generate a complete "Release Package" containing all changed artifacts formatted for their destination. The Project Lead is responsible for committing these updates to the GitHub repository.

### 4.3. AI Portability Guide
To transfer this project to a new AI model or platform, the following steps should be followed:
1.  **Initiate a New Chat:** Start a fresh conversation with the new AI.
2.  **Provide Full Context:** Copy the entire contents of the latest Project Charter version (`/project_charter/Project_Charter_vX.md`) and paste it as the initial prompt.
3.  **Provide the AI's Constitution:** Copy the entire contents of `AI_Constitution.md` and provide it to the AI.
4.  **Provide the Changelog:** Copy the entire contents of `Changelog.md` to give the AI a history of what has been tried.
5.  **State the Goal:** Conclude the initial prompt with a clear directive, such as: "You are now the AI Solutions Architect for this project. Your task is to continue development based on my next set of business requirements. Acknowledge that you have understood and assimilated this context."

---

**(Sections 5, 6, and 7 are now maintained in separate files as defined below)**
