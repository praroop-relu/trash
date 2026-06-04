# Genesis Automaton: Codebase Deep Dive & Architecture Analysis

Welcome to the Genesis Automaton codebase! This document serves as your "brain dump" to understand the core architecture, data flows, and structural components of the system.

## 1. High-Level Architecture & Entry Point
The application is an asynchronous, event-driven orchestration engine built using `aiohttp`.

**Entry Point (`main.py` & `src/api/webhook_server.py`)**
- The application starts in `main.py` by initializing an `aiohttp.web.Application` and configuring CORS.
- It registers several POST/GET endpoints mapped to handlers in `src/api/webhook_server.py`.
- **How it works:** When a webhook hits an endpoint (e.g., `/genesis` for new projects, `/est-update` for estimations), the handler immediately validates the incoming JSON payload using Pydantic models (from `src/models/`). 
- **Async Execution:** To ensure the webhook caller (e.g., Google Forms, ClickUp) doesn't time out, the heavy lifting (workflow orchestration) is passed to a background thread using `asyncio.get_event_loop().run_in_executor()`. The handler then immediately returns a `{"status": "accepted"}` response.

## 2. Data Models (`src/models/`)
The system strictly enforces data contracts using `pydantic`. This ensures that all incoming webhooks have the exact required fields before any processing starts.

- **`new_project.py` (`Project`)**: Defines the payload for a brand new project. It includes fields like `client_name`, `project_description`, and even base64 encoded `attachments`. It also includes custom Pydantic validators (e.g., `parse_and_join_communication_platforms`) to sanitize data automatically.
- **`project_estimation.py`**: Captures developer estimations, hours, costs, and assignments.
- **`project_approval.py`**: Captures client/manager approval, internal/external delivery dates, and contract types.
- **`project_delivery.py` / `version_project.py` / `project_feedback.py`**: Capture respective lifecycle events.

## 3. Core Workflows (`src/core/`)
This is the "Brain" of the operation. Each lifecycle event has a dedicated workflow class that orchestrates external services.

**Example: `InitialWorkflow` (`src/core/initial_workflow.py`)**
When a `/genesis` webhook fires, the `Workflow.run()` method executes a sequence of operations:
1. **AI Generation (`_generate_project_name`, `_generate_project_brief`)**: Uses Google Gemini to generate an SEO-friendly name and a detailed markdown project brief.
2. **Google Drive Provisioning (`_create_google_drive_folder`, `_upload_attachments_to_drive`, `_upload_brief_to_drive`)**: Creates a folder, decodes base64 attachments, converts the markdown brief to a PDF, and uploads everything.
3. **GitHub Setup (`_create_github_repo`, `_add_github_collaborators`)**: Creates a private repository and adds configured developers.
4. **ClickUp Tracking (`_create_clickup_task`)**: Creates a master tracking task containing all the generated URLs (Drive, GitHub, Brief).
5. **Team Suggestion (`_suggest_team`)**: Queries the database for employee skills and asks Gemini to recommend the best managers and developers for the project.
6. **Communications (`_send_notifications`)**: Creates an MS Teams group chat via the MS Graph API and sends formatted notification messages.
7. **Database Persistence (`_fill_db`)**: Saves the project metadata, ClickUp ID, and Drive folder ID to local SQLite databases.

Other workflows (like `DeliveryWorkflow` or `EstimationWorkflow`) follow a similar pattern: they read data, update ClickUp statuses, send Teams/Outlook notifications, and update Google Sheets and the local SQLite database.

## 4. Service Integration Layer (`src/services/`)
These are isolated wrappers around third-party APIs. They abstract away the authentication and HTTP requests.
- **`ai_service.py`**: Wraps the `google-genai` SDK. Contains prompts for brief generation, name generation, and team recommendations.
- **`google_drive_service.py`**: Manages OAuth2 Service Account authentication and executes folder/file uploads.
- **`github_service.py`**: Authenticates as a GitHub App using a JWT and private key to manage repositories.
- **`clickup_service.py`**: Simple wrapper around the ClickUp REST API using API tokens.
- **`ms_user_service.py` / `ms_app_service.py`**: Interacts with the Microsoft Graph API. One uses delegated user permissions (for sending emails or creating chats), while the other uses application-level permissions (e.g., for background monitoring).
- **`hrms_service.py`**: Interacts with your custom HRMS portal for employee stats and project synchronization.

## 5. Database Layer (`src/database/`)
The application uses SQLite (`data/genesis_automaton.db`) for lightweight, serverless persistence.

- **`db_init.py`**: The schema definition script. Contains `CREATE TABLE` statements for `ProjectDetails`, `LookUpIDs`, `Threads`, `Chats`, `Monitored`, etc.
- **`project_db_utils.py` (`ProjectManager`)**: Handles CRUD operations for project metadata (e.g., inserting a new project, updating its status to "Delivered").
- **`ms_db_utils.py`**: Manages Microsoft Teams thread and chat tracking.
- **`integrated_db_utils.py`**: Performs cross-service queries (e.g., mapping a ClickUp task ID to a specific Microsoft Teams Thread ID).

## 6. Configuration & Secrets (`src/core/config.py`)
- The system heavily relies on environment variables defined in the `.env` file.
- `config.py` loads these variables, ensures all required keys are present, and exposes them as a global `settings` dictionary.
- Some credentials (like `GOOGLE_SHEETS_CREDENTIALS` or `GOOGLE_APPLICATION_CREDENTIALS`) point to JSON files stored securely in the `secrets/` directory to avoid exposing raw JSON strings in the environment.

## Summary of the Data Flow
1. **Trigger**: External system hits HTTP endpoint.
2. **Validation**: Pydantic validates the JSON payload.
3. **Dispatch**: Handler hands the validated model to an asynchronous background worker in `src/core/`.
4. **Execution**: The Core workflow calls multiple `src/services/` in sequence to provision resources.
5. **Persistence**: The Core workflow saves the final state to `src/database/` and logs the output.
