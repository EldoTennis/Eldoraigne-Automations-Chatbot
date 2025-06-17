# Eldoraigne Automations & Chatbot: Project Charter

**Version:** 7.0
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
- The architecture is modular, allowing for future integration of new platforms (like WhatsApp).
- All project artifacts are version-controlled in the central GitHub repository.

---

## 2. Business Requirements

This section acts as the formal input from the project owner.
- **BR-001: Core Mission:** The system shall provide automated and conversational capabilities for the Eldoraigne Tennis Club.
- **BR-002: User Segmentation & Access Control:** The system must identify users and segment them into `committee`, `member`, and `unknown` roles.
- **BR-003: Role-Based Menus:** Each user role must be presented with a unique, dynamic menu of options relevant to their role.
- **BR-004: Committee Member Features:** Committee members shall have access to four main feature categories: "Duty Roster," "Communications," "Members," and "Treasurer," each with a corresponding sub-menu of actions.
- **BR-005: System Administration & Maintainability:** The system must be designed to be administered by a non-coder via Google Sheets.
- **BR-006: Platform Strategy:** The initial user interface will be a Telegram chatbot, with a modular architecture to support future platforms.
- **BR-007: Technology Stack:** The system will use a self-hosted n8n.io instance, a Postgres SQL database, and the club's existing Google Workspace subscription.
- **BR-008: Version Control:** All project artifacts must be stored and version-controlled in a central GitHub repository.
- **BR-009: Secure Google Workspace Integration:** The system must interact with Google Workspace services securely, without using personal user accounts.

---

## 3. Core System Architecture

### 3.1. Architectural Pattern: The "Platform-Agnostic Adapter" Model
The system is composed of two distinct types of n8n workflows:
- **Adapters:** Lightweight workflows for specific chat platforms (e.g., Telegram). They translate messages between the platform and the Core Engine.
- **Core Engine:** A single, platform-agnostic workflow that contains all business logic, state management, and connections to data sources.

### 3.2. High-Level Design (Component Diagram)
The canonical version of this diagram is maintained as a Mermaid `.mmd` file in the GitHub repository under `/architecture_diagrams/`.

### 3.3. Code Repository & Documentation
- **Code & Artifact Repository:** GitHub.com is the central repository for all version-controlled project artifacts.
- **Operator's Manual:** The GitHub Wiki serves as the primary documentation for the system's day-to-day administrator.

### 3.4. Technology Stack & Security Model
- **Workflow Automation:** n8n.io (Community Edition, Self-Hosted)
- **State Management:** Postgres SQL Database
- **Primary Chat Interface:** Telegram
- **LLM Provider:** Google AI (Gemini Pro)
- **Google Workspace Integration (Security Model):**
    - A dedicated, non-human **Google Service Account** will be used for all interactions with Google Workspace.
    - Access will be granted on a resource-by-resource basis by sharing specific Google Drive folders, Google Sheets, or Google Calendars with the Service Account's unique email address.
- **Credentials Management:** All sensitive credentials will be stored exclusively within n8n's encrypted Credentials store.

---

## 4. Development & Release Cycle

### 4.1. Development Phase (Conversational)
New `Business Requirements` are provided by the Project Lead. The AI Solutions Architect will update the Project Charter and propose technical solutions within the chat for rapid iteration.

### 4.2. Release Phase (Formal)
Upon the command **"This is approved. Prepare the release for commit,"** the AI will generate a complete "Release Package" containing all changed artifacts formatted for their destination. The Project Lead is responsible for committing these updates to the GitHub repository.

---

## 5. Functional Requirements

### 5.1. User Role Identification
- **FR-5.1.1:** Upon receiving a message, the system must use the user's `chat.id` to look up their `role` in the `Member_List` Google Sheet.
- **FR-5.1.2:** Based on the `role` returned, the system will look up and display the corresponding main menu from the `Menu_Structure` configuration sheet.

### 5.2. Committee Member Menu Structure
The following tables define the menus and sub-menus that will be presented to users with the `committee` role.

**5.2.1. Main Menu (`main_menu_committee`)**
*   **Message Text:** `Welcome, Committee Member [User Name]! What would you like to do?`

| Button Text | `callback_data` | Target |
| :--- | :--- | :--- |
| 1. Duty Roster | `menu_duty_roster` | Display Sub-Menu: `sub_menu_duty_roster` |
| 2. Communications | `menu_communications` | Display Sub-Menu: `sub_menu_communications` |
| 3. Members | `menu_members` | Display Sub-Menu: `sub_menu_members` |
| 4. Treasurer | `menu_treasurer` | Display Sub-Menu: `sub_menu_treasurer` |

**5.2.2. Sub-Menu: Duty Roster (`sub_menu_duty_roster`)**
*   **Message Text:** `Duty Roster Actions:`

| Button Text | `callback_data` | Target |
| :--- | :--- | :--- |
| 1. Check when I am next on Duty | `action_check_my_duty` | Start Workflow: `Check Personal Duty` |
| 2. Check Schedule for next 8 weeks | `action_view_8_week_roster` | Start Workflow: `View Roster Schedule` |
| 3. Swop out my duty schedule | `action_initiate_swop` | Start Workflow: `Duty Swop` |
| ⬅️ Back to Main Menu | `menu_main_committee` | Display Main Menu |

**5.2.3. Sub-Menu: Communications (`sub_menu_communications`)**
*   **Message Text:** `What type of communication would you like to create?`

| Button Text | `callback_data` | Target |
| :--- | :--- | :--- |
| 1. Wednesday Social | `action_comms_wednesday` | Start Workflow: `Create Social Comms` |
| 2. Saturday Social | `action_comms_saturday` | Start Workflow: `Create Social Comms` |
| 3. Junior Program | `action_comms_junior` | Start Workflow: `Create Generic Comms` |
| 4. Birthdays | `action_comms_birthdays` | Start Workflow: `Create Birthday Comms` |
| 5. Leagues | `action_comms_leagues` | Start Workflow: `Create Generic Comms` |
| 6. Club Champs | `action_comms_champs` | Start Workflow: `Create Generic Comms` |
| 7. Special Club Event | `action_comms_special` | Start Workflow: `Create Generic Comms` |
| 8. Other | `action_comms_other` | Start Workflow: `Create Generic Comms` |
| ⬅️ Back to Main Menu | `menu_main_committee` | Display Main Menu |

**5.2.4. Sub-Menu: Members (`sub_menu_members`)**
*   **Message Text:** `Member Management Actions:`

| Button Text | `callback_data` | Target |
| :--- | :--- | :--- |
| 1. Query Member Contact | `action_query_member` | Start Workflow: `Query Member Details` |
| 2. Update Member Contact | `action_update_member` | Start Workflow: `Update Member Details` |
| 3. Provide Member Stats | `action_member_stats` | Start Workflow: `Generate Member Stats` |
| ⬅️ Back to Main Menu | `menu_main_committee` | Display Main Menu |

**5.2.5. Sub-Menu: Treasurer (`sub_menu_treasurer`)**
*   **Message Text:** `Treasurer Actions:`

| Button Text | `callback_data` | Target |
| :--- | :--- | :--- |
| 1. Membership Payment Stats | `action_payment_stats` | Start Workflow: `Generate Payment Stats` |
| 2. Outstanding membership | `action_outstanding_payments` | Start Workflow: `Generate Outstanding List` |
| ⬅️ Back to Main Menu | `menu_main_committee` | Display Main Menu |

---

## 6. Data & Configuration Schema

### 6.1. Configuration Hub (Google Sheets)
*   **Sheet: `Member_List`** -> Columns: `chat_id`, `user_name`, `role`
*   **Sheet: `Menu_Structure`** -> Columns: `menu_name`, `message_text`, `button_1_text`, `button_1_callback`, `button_2_text`, `button_2_callback`, ...
*   **Sheet: `Step_Messages`** -> Columns: `step_name`, `message_text`, `button_1_text`, `button_1_callback`, ...

### 6.2. State Database (JSON Structure)
*   The state for each `chat.id` will be stored as a JSON object in the Postgres database.
*   **Example Structure:**
    ```json
    {
      "currentStep": "awaiting_main_menu_choice",
      "userName": "John Doe",
      "userRole": "committee",
      "activeWorkflow": "none",
      "workflowData": {}
    }
    ```

---

## 7. The Constitution (My Core Operating Principles)

This is the rulebook I, the AI assistant, must follow.
- **Principle 1: The Requirement Translation Mandate:** My primary function is to translate `Business Requirements` into updates across all relevant sections of this charter.
- **Principle 2: The Development Cycle Mandate:** I must adhere to the "Development & Release Cycle" defined in Section 4. I will only produce a full "Release Package" for GitHub upon the user's explicit approval command.
- **Principle 3: The "Dumb Robot" Core Model:** I will design the bot to be a stateful robot that executes a script.
- **Principle 4: The "One-Way Street" Architecture:** The workflow must be linear and sequential.
- **Principle 5: The "Ask-Then-Act" Interaction Pattern:** The bot must present simple, static menus first, and only perform complex actions *after* a user has made their selection.
- **Principle 6: Maintainability First:** Designs must prioritize management via Google Sheets for a non-coder.
- **Principle 7: No Omissions, No Summaries:** All provided artifacts, especially within a formal Release Package, must be complete and unabridged.

---

## 8. Project History & Changelog

- **v1.0 - v2.0:** Established the core charter (originally "The Genesis Project"), defined the "Platform-Agnostic Adapter" model, and formalized the Business Requirements section.
- **v3.0:** Formalized the use of a GitHub repository and established Mermaid.js as the standard for architecture diagrams.
- **v4.0:** Project formally renamed to "Eldoraigne Automations & Chatbot".
- **v5.0:** Formalized the "Development & Release Cycle" and established the GitHub Wiki as the official "Operator's Manual".
- **v6.0:** Defined the full menu structure for Committee Members. Formalized the use of a Google Service Account as the official security model.
- **v7.0 (Current):** Re-consolidated the AI Constitution and Project Changelog into this main Project Charter for easier maintenance, reverting the file structure change from v6.0.
