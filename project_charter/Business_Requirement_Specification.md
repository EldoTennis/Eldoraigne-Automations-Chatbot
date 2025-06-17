# Business Requirement Specification (BRS)

**Project Name:** Eldoraigne Automations & Chatbot
**Version:** 2.0
**Last Updated:** 26 June 2025

---

## 1. Project Vision & Purpose

### 1.1. Project Goal
To create an automated, conversational interface for the Eldoraigne Tennis Club that provides distinct experiences for three user groups: Committee Members, Club Members, and the Public (Non-Members). The system will leverage n8n.io for workflow automation, Google Workspace for data and configuration, and a Large Language Model (LLM) for generative tasks, ultimately aiming to reduce administrative overhead and improve engagement.

### 1.2. Success Criteria
- The system can successfully identify a user's role and present them with the correct, role-specific menu.
- The system's configuration can be updated by editing a Google Sheet, without modifying the core n8n workflow.
- The architecture is modular, allowing for future integration of new platforms (like WhatsApp).
- All project artifacts are version-controlled in the central GitHub repository.

---

## 2. Business Requirements

This section acts as the formal input from the project owner.
- **BR-001: Core Mission:** The system shall provide automated and conversational capabilities for the Eldoraigne Tennis Club.
- **BR-002: User Segmentation & Access Control:** The system must identify users and segment them into `committee`, `member`, and `unknown` roles.
- **BR-003: Role-Based Menus:** Each user role must be presented with a unique, dynamic menu of options.
- **BR-004: Committee Member Features:** Committee members shall have access to four main feature categories: "Duty Roster," "Communications," "Members," and "Treasurer."
- **BR-005: Phased Rollout:** The initial development focus will be on the "Communications -> Wednesday Social" feature. All other menu paths will be scaffolded with a "Coming Soon" message.
- **BR-006: System Administration & Maintainability:** The system must be designed to be administered by a non-coder via Google Sheets.
- **BR-007: Platform Strategy:** The initial user interface will be a Telegram chatbot, with a modular architecture to support future platforms.
- **BR-008: Technology Stack:** The system will use a self-hosted n8n.io instance, a Postgres SQL database, and Google Workspace.
- **BR-009: Version Control:** All project artifacts must be stored and version-controlled in a central GitHub repository.
- **BR-010: Secure Google Workspace Integration:** The system must interact with Google Workspace services securely via a dedicated Google Service Account.
- **BR-011: Communications Log & Feedback Loop:** A log of all generated communications must be kept in a Google Sheet for human review and as a knowledge source for the LLM to prevent duplication.
- **BR-012: Configurable Search Queries:** All search queries used by the LLM must be sourced from a dedicated Google Sheet.
- **BR-013: Graceful Failure & Maintainable Fallbacks:** The system must handle non-critical data lookup failures gracefully. Manual fallback lists must be sourced from a Google Sheet.
- **BR-014: Number-Based Menu Interaction:** User interaction with menus must be based on the user replying with a corresponding number.
- **BR-015: Externalized & Hybrid Message Templates:** All final output messages must be constructed from templates defined in a Google Sheet, supporting a mix of `fixed` text and `ai_generated` sections with configurable prompts.

---

## 3. Changelog
- **v1.0:** Document created. Migrated and consolidated all business-facing requirements from the monolithic Project Charter v8.0.
- **v2.0 (Current):** No changes in this version.
