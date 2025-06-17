# High-Level Design (HLD)

**Project Name:** Eldoraigne Automations & Chatbot
**Version:** 2.1
**Last Updated:** 26 June 2025

---

## 1. Core System Architecture

### 1.1. Architectural Pattern: The "Platform-Agnostic Adapter" Model
The system is composed of two distinct types of n8n workflows:
- **Adapters:** Lightweight workflows for specific chat platforms (e.g., Telegram). They translate messages between the platform and the Core Engine.
- **Core Engine:** A single, platform-agnostic workflow that contains all business logic, state management, and connections to data sources.

### 1.2. High-Level Design (Component Diagram)
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
    ```

### 1.3. Code Repository & Documentation
- **Code & Artifact Repository:** GitHub.com is the central repository for all version-controlled project artifacts.
- **Operator's Manual:** The GitHub Wiki serves as the primary documentation for the system's day-to-day administrator.

### 1.4. Technology Stack & Security Model
- **Workflow Automation:** n8n.io (Community Edition, Self-Hosted)
- **State Management:** Postgres SQL Database
- **Primary Chat Interface:** Telegram
- **LLM Provider:** Google AI (Gemini Pro)
- **Google Workspace Integration (Security Model):**
    - A dedicated, non-human **Google Service Account** will be used for all interactions with Google Workspace.
    - Access will be granted on a resource-by-resource basis by sharing specific Google Drive folders, Google Sheets, or Google Calendars with the Service Account's unique email address.
- **Credentials Management:** All sensitive credentials will be stored exclusively within n8n's encrypted Credentials store.

---

## 2. Development & Release Cycle

### 2.1. Development Phase (Conversational)
New `Business Requirements` are provided by the Project Lead. The AI Solutions Architect will update the project documentation suite and propose technical solutions within the chat for rapid iteration.

### 2.2. Release Phase (Formal)
Upon the command **"This is approved. Prepare the release for commit,"** the AI will generate a complete "Release Package" containing all changed artifacts formatted for their destination. The Project Lead is responsible for committing these updates to the GitHub repository.

---

## 3. Changelog
- **v1.0:** Document created. Migrated all architectural, security, and process definitions from the monolithic Project Charter v8.0.
- **v2.0:** Re-organized sections to move "Technology Stack," "Code Repository," and "Development Cycle" into this document from the BRS, as they are architectural concerns.
- **v2.1 (Current):** Corrected Markdown formatting for nested Mermaid diagram to resolve rendering issues.
