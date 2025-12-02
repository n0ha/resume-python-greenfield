# Implementation Plan: Resume Generation Backend

This document provides a detailed, incremental plan for developing the resume generation backend, following the hybrid architecture outlined in `backend-spec.md`. The project is broken down into five distinct phases, each with specific, actionable steps.

---

### Phase 1: Foundation & Project Setup

**Goal:** Establish the complete project infrastructure, from the cloud BaaS to the local development environment, ensuring all pieces can communicate.

*   **Step 1.1: Supabase Project Initialization**
    *   **Task:** Create a new project in the Supabase dashboard.
    *   **Deliverables:**
        *   Securely store the Project URL, `anon` key, `service_role` key, and the full Database Connection String.
        *   These credentials will be used in the FastAPI application's environment variables.

*   **Step 1.2: Database Schema Definition**
    *   **Task:** Use the Supabase SQL Editor to define the initial database schema.
    *   **Deliverables:**
        *   SQL scripts (`CREATE TABLE ...`) for `applications`, `job_offers`, `cvs`, `communications`, and `action_points`.
        *   Define all columns, data types, foreign key relationships, and user ID associations.
        *   Enable Row-Level Security (RLS) on all tables.
        *   Implement basic RLS policies to ensure users can only access their own data.

*   **Step 1.3: Local FastAPI Project Scaffolding**
    *   **Task:** Create the local project structure and initialize dependencies using `uv`.
    *   **Deliverables:**
        *   A new Git repository for the backend project.
        *   Directory structure as defined in the spec (`app/`, `app/core/`, `app/models/`, etc.).
        *   A `pyproject.toml` file with initial dependencies: `fastapi`, `uvicorn`, `sqlmodel`, `pydantic-settings`, `supabase-py`, and `psycopg2-binary`.
        *   A `.env` file template (`.env.example`) and a `app/core/config.py` module using `pydantic-settings` to load Supabase credentials.

*   **Step 1.4: Establish Database Connectivity**
    *   **Task:** Implement the code to connect the FastAPI application to the Supabase database.
    *   **Deliverables:**
        *   A `app/db/session.py` module to manage database sessions.
        *   A `/health` endpoint in `app/main.py` that performs a simple query to the database to confirm a successful connection.

---

### Phase 2: Data Models & Core Endpoint

**Goal:** Align the application's data structures with the database and create the main entry point for the core logic.

*   **Step 2.1: Implement SQLModel Definitions**
    *   **Task:** Create the Python representations of the database tables.
    *   **Deliverables:**
        *   In `app/models/`, create Python files for each table.
        *   Define `SQLModel` classes that precisely match the tables created in Step 1.2, including all fields and relationships.

*   **Step 2.2: Implement Core API Endpoint Skeleton**
    *   **Task:** Create the primary API endpoint that will trigger the CV generation workflow.
    *   **Deliverables:**
        *   An API router in `app/routers/applications.py`.
        *   A `POST /applications` endpoint.
        *   Pydantic models for the request body (e.g., `CreateApplicationRequest`) and response.
        *   Initially, this endpoint will only accept data and return a static success message.

---

### Phase 3: Agentic Workflow Implementation

**Goal:** Build the intelligent core of the application by implementing the agent-based system.

*   **Step 3.1: Integrate `mcp-agent`**
    *   **Task:** Add the `mcp-agent` library and set up the basic agent structure.
    *   **Deliverables:**
        *   `mcp-agent` added as a dependency in `pyproject.toml`.
        *   An `app/core/agents.py` module to house the agent definitions.

*   **Step 3.2: Develop the Job Offer Parsing Agent**
    *   **Task:** Create the first agent responsible for converting raw text into a structured `JobOffer`.
    *   **Deliverables:**
        *   A `JobOfferParsingAgent` that uses an LLM call as a "tool".
        *   The agent's prompt will guide the LLM to output JSON conforming to the `JobOffer` Pydantic model.
        *   This replaces the logic from the POC's `job_offer_converter.py`.

*   **Step 3.3: Develop the CV Customization Agent**
    *   **Task:** Create the agent that generates the CV edit plan.
    *   **Deliverables:**
        *   A `CVCustomizationAgent`.
        *   This agent will have access to tools for reading the base CV and the structured `JobOffer`.
        *   Its primary tool will be an LLM call that produces a `PlannedChanges` object, replacing the logic from `llm_interaction.py`.

*   **Step 3.4: Develop the CV Editor Agent**
    *   **Task:** Create the agent that applies the generated edits.
    *   **Deliverables:**
        *   A `CVEditorAgent`.
        *   A tool that wraps the `YamlEditor` logic from the POC, ensuring safe and schema-compliant edits.

---

### Phase 4: End-to-End Workflow Integration

**Goal:** Connect all the pieces to create a fully functional, end-to-end CV generation pipeline.

*   **Step 4.1: Create the Orchestration Service**
    *   **Task:** Write the service logic that calls the agents in the correct sequence.
    *   **Deliverables:**
        *   An `app/core/services.py` module.
        *   A function that manages the entire workflow: `raw_text` -> `JobOfferParsingAgent` -> `CVCustomizationAgent` -> `CVEditorAgent` -> `modified_cv_yaml`.
        *   Connect this service to the `POST /applications` endpoint.

*   **Step 4.2: Implement Database Persistence**
    *   **Task:** Save the results of the workflow to the database.
    *   **Deliverables:**
        *   The orchestration service will create and commit the `Application`, `JobOffer`, and `CV` records to the Supabase database at the end of a successful run.

*   **Step 4.3: Integrate PDF Rendering**
    *   **Task:** Add the `rendercv` logic to the workflow.
    *   **Deliverables:**
        *   `rendercv` added as a dependency.
        *   The service will use `rendercv` to generate a PDF from the `modified_cv_yaml` and save it to a temporary file.

*   **Step 4.4: Implement PDF Upload to Supabase Storage**
    *   **Task:** Store the final PDF artifact.
    *   **Deliverables:**
        *   The service will use the `supabase-py` client to upload the generated PDF to a Supabase Storage bucket.
        *   The `cvs` table record will be updated with the public or signed URL of the stored PDF.

---

### Phase 5: Production Readiness

**Goal:** Prepare the application for deployment and real-world use.

*   **Step 5.1: Implement API Security**
    *   **Task:** Secure the FastAPI endpoints.
    *   **Deliverables:**
        *   A FastAPI dependency that validates the Supabase JWT from the `Authorization` header.
        *   The user ID from the JWT is extracted and used to scope all database operations, enforcing RLS.

*   **Step 5.2: Comprehensive Testing**
    *   **Task:** Build a robust test suite.
    *   **Deliverables:**
        *   `pytest` setup.
        *   Unit tests for services and agent tools (mocking LLM calls).
        *   Integration tests for the `POST /applications` endpoint.

*   **Step 5.3: Containerize the Application**
    *   **Task:** Create a Docker image for the FastAPI service.
    *   **Deliverables:**
        *   A `Dockerfile` that creates a production-ready container for the application.

*   **Step 5.4: Deploy the Service**
    *   **Task:** Deploy the containerized application to a cloud provider.
    *   **Deliverables:**
        *   A chosen deployment platform (e.g., Fly.io, Render, Google Cloud Run).
        *   A CI/CD pipeline (e.g., using GitHub Actions) to automate testing and deployment.
