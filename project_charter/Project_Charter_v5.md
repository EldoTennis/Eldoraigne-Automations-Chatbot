# Eldoraigne Automations & Chatbot: Project Charter

**Version:** 5.0
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

### 3.3. Code Repository & Documentation
- **Code & Artifact Repository:** GitHub.com is the central repository for all version-controlled project artifacts, including this charter, n8n workflow JSON, and diagrams.
- **Operator's Manual:** The GitHub Wiki serves as the primary documentation for the system's day-to-day administrator. It contains user guides, "how-to" articles for maintenance, and troubleshooting steps.

### 3.4. Technology Stack & Credentials Store
- **Workflow Automation:** n8n.io (Community Edition, Self-Hosted)
- **State Management:** Postgres SQL Database
- **Configuration Hub & Knowledge Store:** Google Sheets / Google Docs
- **LLM Provider:** Google AI (Gemini)
- **Primary Chat Interface:** Telegram
- **Credentials Management:** All sensitive credentials (API keys, tokens) will be stored exclusively within n8n's encrypted Credentials store. They will **not** be committed to the GitHub repository.

---

## 4. Development & Release Cycle

### 4.1. Development Phase (Conversational)
- New `Business Requirements` are provided by the Project Lead (the user) in this chat.
- The AI Solutions Architect (the AI) will update the Project Charter and propose technical solutions and artifacts (JSON, diagrams, etc.) within the chat.
- This phase allows for rapid testing and iteration without formally committing changes.

### 4.2. Release Phase (Formal)
- When the Project Lead is satisfied with the developed changes, they will issue the command: **"This is approved. Prepare the release for commit."**
- The AI will then generate a complete "Release Package" containing all changed artifacts (charter, JSON, Wiki content) formatted for their final destination.
- The Project Lead is responsible for taking this package and manually updating the corresponding files in the GitHub repository, which constitutes a formal version commit.

---

## 5. Functional Requirements

### 4.1. User Role Identification
- Upon receiving a message, the system must use the user's `chat.id` to look up their role in the `User_Roles` Google Sheet.
- If a role is found, the system must display the corresponding role-specific main menu as defined in the `Menu_Structure` Google Sheet.
- If no role is found, the user is assigned the `unknown` role and shown the public-facing menu.

### 4.2. Core Features (by Role)
- **Committee Member:** Can initiate the "Create Social Comms" workflow.
- **Club Member:** (Future Requirement)
- **Unknown User:** (Future Requirement)

### 4.3. "Create Social Comms" Workflow
- A multi-step conversational process that guides the user through data collection.
- Utilizes automated lookups against the `ETC Duty Roster 2025` Google Sheet.
- Utilizes Google Search via an LLM to find news and weather data.
- Utilizes an LLM for creative tasks.
- Upon user approval, saves the final output to a "Knowledge Store" Google Sheet for future reference.

---

## 6. Data & Configuration Schema

### 5.1. Configuration Hub (Google Sheets)
- **Sheet: `User_Roles`** -> Columns: `chat_id`, `user_name`, `role`
- **Sheet: `Menu_Structure`** -> Columns: `menu_name`, `message_text`, `button_1_text`, `button_1_callback`, `button_2_text`, `button_2_callback`, ...
- **Sheet: `Step_Messages`** -> Columns: `step_name`, `message_text`, `button_1_text`, `button_1_callback`, ...

### 5.2. State Database (JSON Structure)
- The state for each `chat.id` will be stored as a JSON object in the Postgres database.
- **Example Structure:**
  ```json
  {
    "currentStep": "awaiting_step_4_answer",
    "userName": "John Doe",
    "userRole": "committee",
    "answers": {
      "channels": "whatsapp_website",
      "date_display": "Wednesday, 25 June 2025",
      "date_lookup": "25-Jun-2025",
      "location": "Eldoraigne Ext 3, Centurion"
    }
  }
