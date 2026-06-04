# Genesis Automaton: Version Project Workflow (version_workflow.py)

This README documents the project versioning workflow implemented in `src/core/version_workflow.py`. This workflow handles the creation of a new version, sprint, or milestone for an existing project.

Genesis Automaton is a webhook-driven system that streamlines the versioning of a software project. It integrates multiple third‑party APIs to reduce manual effort and accelerate project kick‑off.

## Project Overview

The core of the system is an asynchronous web server that listens for a webhook. When a `POST` request with valid project version data is received, it triggers a background workflow that orchestrates a series of actions across various platforms. This ensures that all foundational assets for a new project version are created consistently and immediately.

## Core Features (Version Workflow)

* **Webhook-Driven Automation**: A single `POST /version-update` kicks off the entire versioning flow.
* **Versioned Project Name Generation**: Generates a standardized project name based on the version type (Version, Sprint, Milestone) and variant.
* **Resource Reuse**: Reuses existing resources from the original project, such as the Microsoft Teams chat and HRMS project.
* **Google Drive Provisioning**: Creates a new project folder for the version and uploads any provided attachments.
* **ClickUp Task Creation**: Generates a new master task for the version, linking to the new Drive folder and the original project's resources.
* **Microsoft Teams & Email Notifications**: Renames the existing Teams group chat and sends notifications about the new version. A new email thread is also created.
* **Database Persistence**: Stores versioned project metadata for subsequent phases.
* **HRMS Project Update**: Updates the existing project record in the HRMS portal with the new version name.

## System Workflow

The version workflow is a sequential, automated pipeline triggered by `POST /version-update`.

```mermaid
graph TD
    A[Webhook POST /genesis/version with Project Version JSON] --> B{Genesis Automaton};
    B --> C[Workflow Started];
    C --> D{1. Get Original Project Data};
    D --> E{2. Generate Versioned Project Name};
    E --> F{3. Reuse Original Resources (Teams Chat, HRMS Project)};
    F --> G{4. Create Google Drive Folder};
    G --> H{5. Upload Attachments to Drive};
    H --> I{6. Update Google Sheet};
    I --> J{7. Create ClickUp Task};
    J --> K{8. Rename Teams Chat};
    K --> L{9. Create Email Thread};
    L --> M{10. Send Notifications};
    M --> N{11. Fill Database};
    N --> O{12. Update HRMS Project};
    O --> P[Workflow Complete];
```

Detailed Step Map:

1.  **Get Original Project Data**: Retrieves the original project's data from the database using the `clickup_task_id`.
2.  **Generate Versioned Project Name**: Creates a standardized name for the new version (e.g., "v1.1 - Project-X").
3.  **Reuse Original Resources**: Fetches the Teams chat ID and HRMS project ID from the original project data to be reused.
4.  **Create Google Drive Folder**: Creates a new folder in Google Drive for the versioned project.
5.  **Upload Attachments to Drive**: Uploads any attachments included in the request to the new Drive folder.
6.  **Update Google Sheet**: Appends the version project data to a Google Sheet for tracking.
7.  **Create ClickUp Task**: Creates a new task in ClickUp for the versioned project.
8.  **Rename Teams Chat**: Renames the existing Teams chat to reflect the new version name.
9.  **Create Email Thread**: Creates a new email thread for the version.
10. **Send Notifications**: Sends notifications to the Teams chat about the new version.
11. **Fill Database**: Inserts the new version's data into the project database.
12. **Update HRMS Project**: Updates the project in the HRMS system with the new version name.

All long‑running operations execute asynchronously relative to the HTTP response.

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
    GOOGLE_SHEETS_CREDENTIALS='{"your_json_creds": "here"}'
    GOOGLE_SHEET_ID="YOUR_GOOGLE_SHEET_ID"

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
    
    # HRMS
    HRMS_BASE_URL="YOUR_HRMS_BASE_URL"
    HRMS_EMAIL="YOUR_HRMS_EMAIL"
    HRMS_PASSWORD="YOUR_HRMS_PASSWORD"
    ```

## Running the Server

To start the webhook server, run the main application module:

```bash
uv run main.py
```

The server will start on `http://localhost:8000` by default.

## API Endpoint (Version Workflow Trigger)

### POST /genesis/version

Initiates the versioning workflow. Returns immediately after queuing background execution.

* **Payload**: JSON matching `src/models/version_project.py`.
* **Success (200)**:
    `{ "status": "accepted" }`
* **Error (400)**:
    `{ "status": "error", "message": "..." }`
