# High-Level Design (HLD)

**Project Name:** Eldoraigne Automations & Chatbot
**Version:** 3.0
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
            BA[5. Backup Automation Workflow]
        end

        subgraph "Google Workspace"
            GSC[<i class='fa fa-table'></i> Google Sheets<br><i>Configuration Hub</i>]
            GSK[<i class='fa fa-table'></i> Google Sheets<br><i>Knowledge Store</i>]
            GDB[<i class='fa fa-folder'></i> Google Drive<br><i>File Backups</i>]
        end

        U <--> TA
        TA -- Webhook Call --> CE
        CE -- Reads/Writes State --> DB
        CE -- Reads Config --> GSC
        CE -- Calls AI for creative tasks --> LLM
        CE -- Writes final output --> GSK
        BA -- Reads Config --> GSC
        BA -- Writes Backup --> GDB
    ```

### 1.3. Technology Stack & Security Model
- **Workflow Automation:** n8n.io (Community Edition, Self-Hosted)
- **State Management:** Postgres SQL Database
- **Primary Chat Interface:** Telegram
- **LLM Provider:** Google AI (Gemini Pro)
- **Google Workspace Integration (Security Model):**
    - A dedicated, non-human **Google Service Account** will be used for all interactions with Google Workspace.
    - Access will be granted on a resource-by-resource basis by sharing specific Google Drive folders, Google Sheets, or Google Calendars with the Service Account's unique email address.
- **Credentials Management:** All sensitive credentials will be stored exclusively within n8n's encrypted Credentials store.

---

## 2. Infrastructure & Operations

### 2.1. Hosting & Portability
- **Current Hosting:** The n8n instance and Postgres database are currently hosted on Render.
- **Portability:** The architecture is designed to be portable. A migration to a different hosting provider (e.g., bare-metal or another cloud provider) would primarily involve migrating the Postgres database and re-configuring the n8n environment variables. The core workflow logic remains unchanged.

### 2.2. Backup & Disaster Recovery
- **Configuration Files (Google Sheets):** A dedicated, scheduled n8n workflow (`Backup Automation`) will run nightly. It will read each sheet from the Configuration Hub and save a timestamped `.csv` version to a dedicated "Backups" folder in Google Drive.
- **State Database (Postgres):** The primary backup mechanism for the state database will be the automated backup feature provided by the hosting platform (Render). This is the most reliable method for point-in-time recovery.

### 2.3. Future State: CI/CD Pipeline
- **Vision:** To enable automated deployment of n8n workflows from the GitHub repository to the n8n instance.
- **Proposed Technology:** This would be achieved using **GitHub Actions**.
- **Process:**
    1. A change is merged into the `main` branch of the repository.
    2. A GitHub Action is triggered.
    3. The Action uses a secure SSH connection to the n8n host server.
    4. On the server, it executes the `n8n-cli` (Command Line Interface) command to import the updated workflow `.json` file from the repository into the n8n instance, overwriting the old version.
- **Status:** This is a future requirement and is not in the scope of the initial build.

---

## 3. Development & Release Cycle

### 3.1. Code Repository & Documentation
- **Code & Artifact Repository:** GitHub.com is the central repository for all version-controlled project artifacts.
- **Operator's Manual:** The GitHub Wiki serves as the primary documentation for the system's day-to-day administrator.

### 3.2. Development Phase (Conversational)
New `Business Requirements` are provided by the Project Lead. The AI Solutions Architect will update the project documentation suite and propose technical solutions within the chat for rapid iteration.

### 3.3. Release Phase (Formal)
Upon the command **"This is approved. Prepare the release for commit,"** the AI will generate a complete "Release Package" containing all changed artifacts formatted for their destination. The Project Lead is responsible for committing these updates to the GitHub repository.

---

## 4. Changelog
- **v1.0 - v2.1:** Established core architecture, security model, and development processes.
- **v3.0 (Current):** Added sections for Infrastructure & Operations, including Hosting, Backup & Disaster Recovery, and a vision for a future CI/CD pipeline. Updated the component diagram to include the Backup Automation workflow.
