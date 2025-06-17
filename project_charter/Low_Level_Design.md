# Low-Level Design (LLD)

**Project Name:** Eldoraigne Automations & Chatbot
**Version:** 2.1
**Last Updated:** 26 June 2025

---

## 1. Functional Requirements

### 1.1. User Interaction Model
- **FR-5.1.1:** All menus presented to the user will be formatted as a numbered list within a single Telegram message.
- **FR-5.1.2:** The system will wait for the user to reply with a standard text message containing the number of their choice.
- **FR-5.1.3:** The `Master Switch` node in n8n will route the workflow based on the number received in `message.text`.
- **FR-5.1.4:** The concept of `callback_data` is deprecated and will be removed from the design.

### 1.2. Committee Member Menu Structure & Phased Rollout
- The system will present menus as defined in the tables below.
- Any `Target` that is not yet a defined workflow (e.g., `Start Workflow: Duty Swop`) will instead trigger a "Coming Soon" message and return the user to the previous menu.

**1.2.1. Main Menu (`main_menu_committee`)**
*   **Message Text:** `Welcome, Committee Member [User Name]! What would you like to do?`

| Button Text | Target |
| :--- | :--- |
| 1. Duty Roster | Display Sub-Menu: `sub_menu_duty_roster` |
| 2. Communications | Display Sub-Menu: `sub_menu_communications` |
| 3. Members | Display Sub-Menu: `sub_menu_members` |
| 4. Treasurer | Display Sub-Menu: `sub_menu_treasurer` |

**1.2.2. Sub-Menu: Duty Roster (`sub_menu_duty_roster`)**
*   **Message Text:** `Duty Roster Actions:`

| Button Text | Target |
| :--- | :--- |
| 1. Check when I am next on Duty | Display "Coming Soon" Message |
| 2. Check Schedule for next 8 weeks | Display "Coming Soon" Message |
| 3. Swop out my duty schedule | Display "Coming Soon" Message |
| 4. ⬅️ Back to Main Menu | Display Main Menu |

**1.2.3. Sub-Menu: Communications (`sub_menu_communications`)**
*   **Message Text:** `What type of communication would you like to create?`

| Button Text | Target |
| :--- | :--- |
| 1. Wednesday Social | Start Workflow: `Create Wednesday Social Comms` |
| 2. Saturday Social | Display "Coming Soon" Message |
| 3. Junior Program | Display "Coming Soon" Message |
| 4. Birthdays | Display "Coming Soon" Message |
| 5. Leagues | Display "Coming Soon" Message |
| 6. Club Champs | Display "Coming Soon" Message |
| 7. Special Club Event | Display "Coming Soon" Message |
| 8. Other | Display "Coming Soon" Message |
| 9. ⬅️ Back to Main Menu | Display Main Menu |

**1.2.4. Sub-Menu: Members (`sub_menu_members`)**
*   **Message Text:** `Member Management Actions:`

| Button Text | Target |
| :--- | :--- |
| 1. Query Member Contact | Display "Coming Soon" Message |
| 2. Update Member Contact | Display "Coming Soon" Message |
| 3. Provide Member Stats | Display "Coming Soon" Message |
| 4. ⬅️ Back to Main Menu | Display Main Menu |

**1.2.5. Sub-Menu: Treasurer (`sub_menu_treasurer`)**
*   **Message Text:** `Treasurer Actions:`

| Button Text | Target |
| :--- | :--- |
| 1. Membership Payment Stats | Display "Coming Soon" Message |
| 2. Outstanding membership | Display "Coming Soon" Message |
| 3. ⬅️ Back to Main Menu | Display Main Menu |

### 1.3. Message Generation Engine
- **FR-5.3.1:** Upon final approval in a workflow, the system will read the appropriate template from the `Message_Templates` Google Sheet.
- **FR-5.3.2:** The n8n workflow will loop through each row of the template.
    - If a row's `type` is `fixed`, it will append the `content` to the final message, replacing placeholders.
    - If a row's `type` is `ai_generated`, it will take the prompt from the `content` column, add relevant data, and execute a call to the LLM, appending the result.

---

## 2. Data & Configuration Schema

### 2.1. Configuration Hub (Google Sheets)
- **Sheet: `Member_List`** -> Columns: `chat_id`, `user_name`, `role`
- **Sheet: `Menu_Structure`** -> Columns: `menu_name`, `message_text`, `option_1_text`, `option_2_text`, `...`
- **Sheet: `Committee_List`** -> Columns: `full_name`
- **Sheet: `Search_Parameters`** -> Columns: `search_name`, `search_query`, `result_count`
- **Sheet: `Message_Templates`** -> Columns: `template_name`, `line_number`, `line_type` (`fixed` or `ai_generated`), `content` (fixed text or AI prompt)

### 2.2. Knowledge Store (Google Sheets)
- **Sheet: `Log_Wednesday_Social`** -> Columns: `Timestamp`, `GeneratedBy_UserName`, `EventDate`, `DutyMember`, `Braai`, `Poll`, `NewsHeadline_Used`, `Tagline_Selected`, `Final_WhatsApp_Message`, `Final_Website_Post`
- **(Other log sheets to be created as needed)**

### 2.3. State Database (JSON Structure)
- The state for each `chat_id` will be stored as a JSON object in the Postgres database.
- **Example Structure:**
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

## 3. Changelog
- **v1.0:** Document created. Migrated all detailed functional requirements and data schemas from the monolithic Project Charter v8.0.
- **v2.0:** Added standard document header. Corrected omissions from the initial migration.
- **v2.1 (Current):** Corrected Markdown formatting for nested JSON code block to resolve rendering issues.
