# Business Requirement Specification (BRS)

**Project Name:** Eldoraigne Automations & Chatbot  
**Version:** 4.1  
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

---

## 3. Changelog  
- **v1.0:** Document created as part of monolithic charter.
- **v2.0:** No changes.
- **v3.0:** Added new business requirements for documentation, disaster recovery, and hosting.
- **v3.1:** Re-structured the Business Requirements section into four distinct categories for better organization.
- **v4.0:** Version number updated to align with the new multi-document suite release (v8.0 of the AI Manual).
- **v4.1 (Current):** Version number updated to align with the new documentation suite release (v13.1 of the AI Manual). No other content changes.
