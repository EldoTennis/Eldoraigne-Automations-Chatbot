# Business Requirement Specification (BRS)

**Project Name:** Eldoraigne Automations & Chatbot
**Version:** 4.0
**Last Updated:** 26 June 2025

---

## 1. Project Vision & Purpose

### 1.1. Project Goal
To create an automated, conversational interface for the Eldoraigne Tennis Club that provides distinct experiences for three user groups: Committee Members, Club Members, and the Public (Non-Members). The system will leverage n8n.io for workflow automation, Google Workspace for data and configuration, and a Large Language Model (LLM) for generative tasks, ultimately aiming to reduce administrative overhead and improve engagement.

### 1.2. Success Criteria
- The system can successfully identify a user's role and present them with the correct, role-specific menu.
- The system's configuration can be updated by editing a Google Sheet, without modifying the core n8n workflow.
- The architecture is modular and scalable to include both interactive and backend automations.
- All project artifacts are version-controlled in the central GitHub repository.

---

## 2. Business Requirements

### 2.1. System-Wide & Non-Functional Requirements
- **BR-S01: Core Mission:** The system shall provide automated and conversational capabilities for the Eldoraigne Tennis Club.
- **BR-S02: System Administration & Maintainability:** The system must be designed to be administered by a non-coder via Google Sheets.
- **BR-S03: Platform Strategy:** The initial user interface will be a Telegram chatbot, but the architecture must be modular to support future platforms.
- **BR-S04: Technology Stack:** The system will use a self-hosted n8n.io instance, a Postgres SQL database, and Google Workspace.
- **BR-S05: Version Control:** All project artifacts must be stored and version-controlled in a central GitHub repository.
- **BR-S06: Secure Google Workspace Integration:** The system must interact with Google Workspace services securely via a dedicated Google Service Account.
- **BR-S07: Comprehensive Documentation:** The system must maintain a structured, multi-page GitHub Wiki to serve as an "Operator's Manual," and a central `REFERENCES.md` file.
- **BR-S08: Hosting Portability:** The system architecture must be designed to be portable across different hosting providers.
- **BR-S09: Future CI/CD Exploration:** The possibility of automating workflow deployments from GitHub shall be considered in the architectural design.

### 2.2. Conversational Interface (Chatbot) Requirements
- **BR-C01: User Segmentation & Access Control:** The system must identify users via their chat ID and segment them into `committee`, `member`, and `unknown` roles.
- **BR-C02: Role-Based Menus:** Each user role must be presented with a unique, dynamic menu of options.
- **BR-C03: Committee Member Menu Features:** Committee members shall have access to four main feature categories: "Duty Roster," "Communications," "Members," and "Treasurer."
- **BR-C04: Communications Log & Feedback Loop:** A log of all generated communications must be kept in a Google Sheet for human review and as a knowledge source for the LLM.
- **BR-C05: Configurable Search Queries:** All search queries used by the LLM for conversational enrichment must be sourced from a dedicated Google Sheet.
- **BR-C06: Graceful Failure & Maintainable Fallbacks:** The system must handle non-critical data lookup failures gracefully. Manual fallback lists must be sourced from a Google Sheet.
- **BR-C07: Number-Based Menu Interaction:** User interaction with menus must be based on the user replying with a corresponding number.
- **BR-C08: Externalized & Hybrid Message Templates:** All final output messages must be constructed from templates defined in a Google Sheet, supporting a mix of `fixed` text and `ai_generated` sections.

### 2.3. Process Automation (Backend) Requirements
- **BR-A01: Automated Backups:** The system must include an automated, scheduled workflow to back up all critical configuration files from Google Sheets to a versioned archive in Google Drive.
- **(This section will be expanded as new non-interactive automations are defined)**

### 2.4. Phased Rollout & Prioritization
- **BR-P01: Initial Focus:** The initial development focus will be on the "Communications -> Wednesday Social" conversational workflow for the `committee` user role.
- **BR-P02: Scaffolding:** All other defined menu paths will be scaffolded with a "Coming Soon" message until their development is prioritized.

---

## 3. Changelog
- **v1.0 - v3.0:** Initial document creation and addition of new requirements.
- **v3.1:** Re-structured the Business Requirements section into four distinct categories for better organization.
- **v4.0 (Current):** Version number updated to align with the new multi-document suite release (v6.0 of the AI Manual). No other content changes.
