# Genesis Automaton: Initial Project Workflow (initial_workflow.py)

This README documents ONLY the initial project setup workflow implemented in `src/core/initial_workflow.py` (referred to as `Workflow`). Follow‑up / update flows (e.g. estimation updates) are intentionally out of scope here.

Genesis Automaton is a webhook-driven system that streamlines the earliest phase of a software project: turning a raw project submission into a fully provisioned starter environment (documents, repository, task, communications) plus persistence for next‑phase automation. It integrates multiple third‑party APIs and AI to reduce manual effort and accelerate project kick‑off.

## Project Overview

The core of the system is an asynchronous web server that listens for a webhook. When a `POST` request with valid project data is received, it triggers a background workflow that orchestrates a series of actions across various platforms. This ensures that all foundational assets for a new project are created consistently and immediately.

## Core Features (Initial Workflow)

* **Webhook-Driven Automation**: A single `POST /genesis` kicks off the entire provisioning flow.
* **Conditional AI Project Name Generation**: If no `project_name` is supplied, an AI‑optimized name is generated.
* **AI-Powered Brief Generation**: Uses Google Gemini to produce a structured Markdown project brief which is also rendered to PDF.
* **Google Drive Provisioning**: Creates a project folder; uploads the brief (PDF fallback to Markdown if needed) and any provided source attachments (file IDs copied).
* **GitHub Repository Creation**: Creates a private repository and (if configured) adds collaborators.
* **ClickUp Task Creation**: Generates a master task embedding links to Drive, Brief, and Repository.
* **Microsoft Teams & Email Notifications**: Creates a Teams group chat and distributes a rich summary via Teams + email.
* **Database Persistence**: Stores project + communication metadata for subsequent phases.
* **HRMS Project**: Creates the project record in the HRMS portal via its API.

> HRMS integration is not yet implemented in `initial_workflow.py`; it appears in the workflow definition so downstream consumers can plan for the new artifact.

## System Workflow

The initial workflow is a sequential, automated pipeline triggered by `POST /genesis`.

```mermaid
graph TD
    A[Webhook POST /genesis with Project JSON] --> B{Genesis Automaton};
    B --> C[Workflow Started];
    C --> D{1. If project_name is None: Generate AI Project Name}
    D --> E{2. Generate AI Brief};
    E --> F{3. Create Google Drive Folder};
    F --> G{4. Upload Brief and Attachments (if any) to Drive};
    G --> H{5. Create GitHub Repo};
    H --> I{6. Add Collaborators to Repo};
    I --> J{7. Create ClickUp Task};
    J --> K{8. Send MS Teams & Email Notifications};
    K --> L{9. Create HRMS project};
    L --> M{10. Insert data in the DB for next phase};
    M --> N[Workflow Complete];
```

Detailed Step Map (Current Implementation vs Planned):

1. (Conditional) **AI Project Name Generation**: Only executed if incoming payload omits or leaves `project_name` empty.
2. **AI Brief Generation**: Creates a structured brief (Markdown); sanitized and converted to PDF.
3. **Google Drive Folder Creation**: New folder under configured root.
4. **Brief & Attachment Upload**: PDF brief (fallback Markdown) uploaded; any provided attachment file IDs are copied into the folder.
5. **GitHub Repository Creation**: Private repo named after normalized project name.
6. **Collaborator Addition**: Adds configured usernames (skipped if none configured).
7. **ClickUp Task Creation**: Master task with resource links in description.
8. **Notifications (Teams + Email)**: Group chat created; summary sent to chat + email.
9. **HRMS Project (WIP)**: Placeholder; not yet implemented in code.
10. **Database Persistence**: Inserts project + chat metadata to support later phases (e.g. update workflows, monitoring).

All long‑running operations execute asynchronously relative to the HTTP response (workflow runs in a background executor so the API responds promptly with `accepted`).

## Technology Stack

| Category          | Technology / Library                                                                                             |
| ----------------- | ---------------------------------------------------------------------------------------------------------------- |
| **Web Framework**   | aiohttp                                                                    |
| **Data Validation** | Pydantic                                                                            |
| **Configuration**   | python-dotenv                                                       |
| **APIs & Services** | Google Gemini API, Google Drive API, GitHub API, ClickUp API, Microsoft Graph API (Teams & Outlook)                |
| **API Clients**     | `google-genai`, `google-api-python-client`, `PyGithub`, `requests`, `msal`                                 |

## Setup and Installation

1. **Clone the repository:**

    ```bash
    git clone https://github.com/Relu-Consultancy/genesis-automaton.git
    cd genesis-automaton
    ```

2. **Install dependencies:**

    ```bash
    uv sync
    ```

3. **Configure Environment Variables:**
    Create a `.env` file in the root directory of the project and populate it with the necessary credentials and IDs. See `src/core/config.py` for the full list of required variables.

    ```env
    # Google
    GEMINI_API_KEY="YOUR_GEMINI_API_KEY"
    GOOGLE_APPLICATION_CREDENTIALS="path/to/your/gcp-service-account.json"
    GOOGLE_DRIVE_ROOT_FOLDER_ID="YOUR_ROOT_FOLDER_ID"

    # GitHub
    GITHUB_APP_ID="YOUR_GITHUB_APP_ID"
    GITHUB_ORG="YOUR_GITHUB_ORGANIZATION"
    GITHUB_INSTALLATION_ID="YOUR_APP_INSTALLATION_ID"
    GITHUB_PRIVATE_KEY_PATH="path/to/your/github-app.pem"
    GITHUB_USERNAMES="user1,user2,user3"

    # ClickUp
    CLICKUP_API_KEY="YOUR_CLICKUP_API_KEY"
    CLICKUP_LIST_ID="YOUR_CLICKUP_LIST_ID"

    # Microsoft Teams / Graph API
    MS_USER_CLIENT_ID="YOUR_AZURE_APP_CLIENT_ID"
    MS_USER_TOKEN_CACHE="path/to/your/ms_user_token_cache.json"
    MS_USER_TENANT_ID="YOUR_AZURE_TENANT_ID"
    TEAMS_USER_EMAILS="email1@example.com,email2@example.com"
    ```

## Running the Server

To start the webhook server, run the main application module:

```bash
uv run main.py
```

The server will start on `http://localhost:8000` by default.

## API Endpoint (Initial Workflow Trigger)

### POST /genesis

Initiates the initial provisioning workflow. Returns immediately after queuing background execution.

* **Payload**: JSON matching `src/models/project.py` (fields include project/client metadata, optional attachments, optional client message).
* **Success (200)**:
    `{ "status": "accepted" }`
* **Error (400)**:
    `{ "status": "error", "message": "..." }`

> Subsequent workflow enhancements (e.g. HRMS integration) will not alter the synchronous response shape—only background side effects.
