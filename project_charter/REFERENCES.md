# References & External Knowledge

This document serves as a central repository of links to external documentation, articles, and tutorials that have informed the design and architecture of the Eldoraigne Automations & Chatbot project.

---

## 1. n8n.io Core Concepts

*   **Telegram Trigger Node Documentation**
    *   **Link:** `https://n8n.io/integrations/telegram-trigger/`
    *   **Relevance:** This is the official documentation for the entry point of our Telegram Adapter. It defines how we receive messages, `chat_id`, and `callback_query` data from the Telegram API.
    *   **Status:** Reviewed and incorporated into the adapter design.

*   **State Management in n8n**
    *   **Link:** `https://n8n.io/blog/state-management-in-n8n/`
    *   **Relevance:** This article provides the foundational concepts for building stateful, multi-step conversational bots in a stateless environment like n8n. Our use of a Postgres database as an external "memory" is based on the principles described here.
    *   **Status:** Reviewed and incorporated into the Core Logic Engine design.

*   **Using the n8n CLI (Command Line Interface)**
    *   **Link:** `https://docs.n8n.io/hosting/cli-commands/`
    *   **Relevance:** This documentation will be critical for implementing our "Future State: CI/CD Pipeline." The `n8n import:workflow` command is the key to automating deployments from our GitHub repository.
    *   **Status:** Not yet implemented. For future reference.

---

## 2. Google Workspace Integration

*   **Using Google Service Accounts**
    *   **Link:** `https://cloud.google.com/iam/docs/service-accounts`
    *   **Relevance:** The official Google Cloud documentation defining what a Service Account is, how it works, and how to create one. This is the foundation of our entire security model for interacting with Google Sheets and other Google APIs.
    *   **Status:** Reviewed and incorporated into the HLD security model.

---

## 3. GitHub Documentation

*   **GitHub Wiki Documentation**
    *   **Link:** `https://docs.github.com/en/communities/documenting-your-project-with-wikis`
    *   **Relevance:** Official documentation on how to create, edit, and link pages within the GitHub Wiki, which serves as our "Operator's Manual."
    *   **Status:** Reviewed and incorporated into our documentation strategy.

*   **Mastering Markdown (Mermaid Syntax)**
    *   **Link:** `https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-diagrams`
    *   **Relevance:** Official documentation on how to embed Mermaid diagrams into Markdown files on GitHub. This is crucial for maintaining our architecture diagrams as code.
    *   **Status:** Reviewed and incorporated into our documentation strategy.
