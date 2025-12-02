# Technical Specification: Resume Generation Backend (Hybrid Architecture)

This document outlines the technical specification for the backend of the resume generation application. It details a hybrid architecture that leverages a Backend-as-a-Service (BaaS) for core functionalities and a custom microservice for complex, agent-based logic.

## 1. Project Vision & Goals

The core goal is to create a robust, API-driven service for generating job-specific CVs. This moves away from a local CLI tool towards a backend that can power various frontends (e.g., a web app, browser extension).

Key goals include:
-   **Scalability:** Handle multiple users and requests concurrently.
-   **Maintainability:** A clean, well-structured codebase that is easy to extend.
-   **Flexibility:** An agentic architecture to easily adapt to new LLMs, tools, and workflows.
-   **Rapid Development:** Use a BaaS to handle boilerplate functionality, focusing development time on the unique value proposition.

## 2. Architecture & Technology Stack

The project will be built using a **hybrid architecture**, combining the power of a managed BaaS with a custom Python service for specialized tasks.

### 2.1. Supabase: The BaaS Layer

Supabase will serve as the foundational backend, providing a suite of managed services out-of-the-box.

-   **Role:** Handles the database, user authentication, file storage, and provides an instant, secure CRUD API.
-   **Components:**
    -   **Database:** A managed **PostgreSQL** database. This will be the single source of truth for all data.
    -   **Auto-generated API:** **PostgREST** generates a full RESTful API directly from the database schema. The frontend will use this for simple data operations (e.g., fetching a list of applications).
    -   **Authentication:** Manages user sign-up, login, and row-level security (RLS) to ensure users can only access their own data.
    -   **Storage:** Provides a simple and scalable solution for storing generated PDF CVs and other assets.

### 2.2. FastAPI Service: The Custom Logic Layer

A dedicated FastAPI application will be responsible for the complex business logic that cannot be handled by the BaaS.

-   **Role:** Executes the multi-step, agentic workflow for CV generation. It acts as a secure "webhook" or microservice that is called for specific, intensive tasks.
-   **Responsibilities:**
    -   Receiving requests from the frontend to initiate a new CV generation.
    -   Connecting to the Supabase database to read base data and write results.
    -   Orchestrating the `mcp-agent` workflow to parse job offers and customize CVs.
    -   Using `rendercv` to generate the final PDF.
    -   Uploading the generated PDF to Supabase Storage.

### 2.3. Core Technologies

-   **Backend-as-a-Service (BaaS):** **Supabase**
-   **Custom Logic Server:** **FastAPI**
-   **ASGI Server:** **Uvicorn**
-   **Agent Framework:** **`mcp-agent`**
-   **Database ORM:** **SQLModel** (connecting to the Supabase Postgres DB)
-   **PDF Generation:** **`rendercv`**
-   **Dependency Management:** **`uv`**
-   **Configuration:** **`pydantic-settings`**
-   **Supabase Client:** **`supabase-py`** for interacting with Supabase services (Storage, Auth) from within the FastAPI app.

## 3. Data Models

The `SQLModel` entities defined in our Python code will be kept in sync with the Supabase Postgres database schema. This gives us the benefit of Python-native data validation while using a managed, scalable database. The core models remain: `Application`, `JobOffer`, `CV`, `Communication`, `ActionPoint`.

## 4. Agent-Based Workflow

The core CV customization logic remains the same as previously specified, but it will be entirely encapsulated within the FastAPI service. This service will be called by the frontend to trigger the workflow.

## 5. API Design

The API is now split into two parts: the auto-generated Supabase API and the custom FastAPI service API.

### 5.1. Supabase API (Auto-Generated via PostgREST)

The frontend will directly and securely interact with these endpoints for all standard CRUD operations. Supabase's Row-Level Security will ensure data privacy.

-   `GET /rest/v1/applications`: Fetches applications for the logged-in user.
-   `POST /rest/v1/communications`: Adds a new communication record.
-   ... and so on for all tables in the database.

### 5.2. FastAPI Service API

This service exposes a minimal set of high-level endpoints for the complex workflows.

-   `POST /applications`: The primary endpoint.
    -   **Request:** Contains the base CV content and the job description text.
    -   **Workflow:**
        1.  Authenticates the request (e.g., using the Supabase JWT).
        2.  Initiates the full agent-based workflow.
        3.  Writes all resulting records (`Application`, `JobOffer`, `CV`) to the database.
        4.  Uploads the rendered PDF to Supabase Storage.
    -   **Response:** The ID of the newly created application record.
-   `GET /health`: A simple health check endpoint to confirm the service is running.

## 6. Project Structure (FastAPI Service)

The internal structure of the FastAPI service remains the same:

```
/backend
├── app/
│   ├── __init__.py
│   ├── main.py             # FastAPI app instantiation
│   ├── core/               # Core logic, agents, services
│   ├── models/             # Pydantic & SQLModel definitions
│   ├── routers/            # API endpoints (e.g., for /applications)
│   └── db/                 # Database session management
├── pyproject.toml
├── uv.lock
└── README.md
```

## 7. Next Steps

1.  **Set up Supabase Project:** Create a new project on the Supabase platform.
2.  **Define DB Schema:** Design the tables and relationships directly in the Supabase dashboard or via SQL scripts.
3.  **Sync Models:** Create the corresponding `SQLModel` classes in the FastAPI project.
4.  **Develop FastAPI Skeleton:** Set up the basic FastAPI application, including database connection to Supabase.
5.  **Implement Core Endpoint:** Build the `POST /applications` endpoint and integrate the `mcp-agent` workflow.
6.  **Frontend Integration:** The frontend can then be built to interact with both the Supabase API for data and the FastAPI service for the core CV generation functionality.