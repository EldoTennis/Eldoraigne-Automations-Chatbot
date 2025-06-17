# Eldoraigne Automations & Chatbot: Project Charter

**Version:** 4.0
**Last Updated:** 26 June 2025

---

## 1. Project Vision & Purpose

### 1.1. My Purpose (The AI's Mandate)
My purpose is to act as a Senior Solutions Architect and AI Developer. I am tasked with assisting the project lead (the user) in designing, building, and refining a robust, stateful conversational AI engine for the **Eldoraigne Automations & Chatbot** project. My primary responsibilities are to:
- Maintain and update this Project Charter as the single source of truth.
- Translate formal "Business Requirements" into updated "Functional Requirements," "Architecture," and "Data Schemas."
- Propose stable, scalable, and maintainable architectural patterns that align with the project's goals and constraints.
- Provide complete, unabridged artifacts, such as n8n JSON workflows, Mermaid diagrams, and explicit configuration schemas.
- Document all decisions, failures, and learnings in the Project History & Changelog.

### 1.2. Project Goal
To create an automated, conversational interface for the Eldoraigne Tennis Club that provides distinct experiences for three user groups: Committee Members, Club Members, and the Public (Non-Members). The system will leverage n8n.io for workflow automation, Google Workspace for data and configuration, and a Large Language Model (LLM) for generative tasks, ultimately aiming to reduce administrative overhead and improve engagement.

### 1.3. Success Criteria
- The system can successfully identify a user's role and present them with the correct, role-specific menu.
- The system can guide a committee member through the entire multi-step process of creating a social media communication post.
- The system's configuration can be updated by editing a Google Sheet, without modifying the core n8n workflow.
- The architecture is modular, allowing for future integration of new platforms (like WhatsApp).
- All project artifacts, including this charter, n8n JSON, and architecture diagrams, are version-controlled in a central GitHub repository.

---

## 2. Business Requirements

This section acts as the formal input from the project owner.
- **BR-001: Core Mission:** The system shall provide automated and conversational capabilities for the Eldoraigne Tennis Club to serve its members and the public.
- **BR-002: User Segmentation & Access Control:** The system must identify users and segment them into at least three distinct roles: `committee`, `member`, and `unknown`. Each role must be presented with a unique, dynamic menu of options relevant to their role.
- **BR-003: Core Feature - Social Comms Generation:** Committee members must have a feature to generate communications for social events.
- **BR-004: System Administration & Maintainability:** The system must be designed to be administered by a non-coder. All user-facing text, menu options, and user roles should be manageable via Google Sheets.
- **BR-005: Platform Strategy:** The initial user interface will be a Telegram chatbot. The architecture must support future integration of other platforms.
- **BR-006: Technology Stack:** The system will be built using a self-hosted n8n.io Community Edition instance, a Postgres SQL database, and the club's existing Google Workspace subscription.
- **BR-007: Version Control:** All project artifacts must be stored and version-controlled in a central GitHub repository.

---

## 3. Core System Architecture

### 3.1. Architectural Pattern: The "Platform-Agnostic Adapter" Model
The system is composed of two distinct types of n8n workflows:
- **Adapters:** Lightweight workflows for specific chat platforms (e.g., Telegram). They translate messages between the platform and the Core Engine.
- **Core Engine:** A single, platform-agnostic workflow that contains all business logic, state management, and connections to data sources.

### 3.2. High-Level Design (Component Diagram)
The canonical version of this diagram is maintained as a Mermaid `.mmd` file in the GitHub repository under `/architecture_diagrams/`.
```mermaid
graph TD
    subgraph "User Environment"
        U[<i class='fa fa-user'></i> Telegram User]
    end

    subgraph "n8n.io (Self-Hosted)"
        TA[1. Telegram Adapter Workflow]
        CE[2. Core Logic Engine Workflow]
        LLM[3. LLM/AI Node]
        DB[(4. Postgres DB<br><i>State Management</i>)]
    end

    subgraph "Google Workspace"
        GSC[<i class='fa fa-table'></i> Google Sheets<br><i>Configuration Hub</i>]
        GSK[<i class='fa fa-table'></i> Google Sheets<br><i>Knowledge Store</i>]
    end

    U <--> TA
    TA -- Webhook Call --> CE
    CE -- Reads/Writes State --> DB
    CE -- Reads Config --> GSC
    CE -- Calls AI for creative tasks --> LLM
    CE -- Writes final output --> GSK
