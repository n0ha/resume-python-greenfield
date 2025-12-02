# Greenfield Specification: Resume Generation Backend

**Objective:** This document provides a comprehensive technical specification for a greenfield reimplementation of the resume generation backend. It contains all necessary data models, algorithms, and logic extracted from the original proof-of-concept to ensure a successful build without access to the original source files.

---

## 1. Detailed Data Models

These models define the database schema. They will be implemented as `SQLModel` classes in Python and as tables in the Supabase Postgres database.

### 1.1. `Application`

Represents a single, overarching job application process.

| Column      | Type                 | Constraints              | Description                                         |
| :---------- | :------------------- | :----------------------- | :-------------------------------------------------- |
| `id`        | `Integer`            | Primary Key, Auto-inc    | Unique identifier for the application.              |
| `created_at`| `String` / `DateTime`| Not Null, Indexed        | Timestamp of creation (ISO 8601 format).            |
| `updated_at`| `String` / `DateTime`| Not Null, Indexed        | Timestamp of last update (ISO 8601 format).         |

### 1.2. `JobOffer`

Stores the structured details of a job offer, typically extracted from raw text.

| Column           | Type                 | Constraints              | Description                                         |
| :--------------- | :------------------- | :----------------------- | :-------------------------------------------------- |
| `id`             | `Integer`            | Primary Key, Auto-inc    | Unique identifier for the job offer.                |
| `application_id` | `Integer`            | Foreign Key (`application.id`) | Links to the parent application.                    |
| `company`        | `String`             | Not Null                 | Name of the company.                                |
| `position`       | `String`             | Not Null                 | The job title.                                      |
| `slugs`          | `JSON`               | Not Null                 | Path-safe slugs, e.g., `{"company": "acme-corp"}`. |
| `artefacts`      | `JSON`               | Not Null                 | Dictionary of file paths or identifiers.            |
| `url`            | `String`             | Nullable                 | URL to the original job posting.                    |
| `discovered`     | `String` / `DateTime`| Nullable                 | Date the job offer was found.                       |
| `subject`        | `String`             | Nullable                 | Subject line or brief title.                        |
| `salary`         | `String`             | Nullable                 | Salary information.                                 |
| `location`       | `String`             | Nullable                 | Job location.                                       |
| `job_type`       | `String`             | Nullable                 | e.g., "full-time", "contract".                    |
| `experience`     | `String`             | Nullable                 | Required experience level.                          |
| `description`    | `String`             | Nullable                 | The full job description text.                      |
| `technologies`   | `JSON` (list of str) | Nullable                 | List of technologies mentioned.                     |
| `created_at`     | `String` / `DateTime`| Not Null, Indexed        | Timestamp of creation.                              |
| `updated_at`     | `String` / `DateTime`| Not Null, Indexed        | Timestamp of last update.                           |

### 1.3. `CV`

Represents a CV document, which can be a base version or a customized one.

| Column           | Type                 | Constraints              | Description                                         |
| :--------------- | :------------------- | :----------------------- | :-------------------------------------------------- |
| `id`             | `Integer`            | Primary Key, Auto-inc    | Unique identifier for the CV.                       |
| `application_id` | `Integer`            | Foreign Key (`application.id`), Nullable | Links to an application if it's a custom CV.        |
| `parent_cv_id`   | `Integer`            | Foreign Key (`cv.id`), Nullable | Links to the base CV it was derived from.           |
| `content`        | `String` / `Text`    | Not Null                 | The full CV content in YAML format.                 |
| `created_at`     | `String` / `DateTime`| Not Null, Indexed        | Timestamp of creation.                              |
| `updated_at`     | `String` / `DateTime`| Not Null, Indexed        | Timestamp of last update.                           |

**Note:** A CV is considered a "base" CV if both `application_id` and `parent_cv_id` are `NULL`.

---

## 2. LLM Interaction Schemas

These Pydantic models are essential for ensuring structured and predictable communication with the LLM.

### 2.1. `EditOperationType` (Enum)

Defines the set of possible actions the LLM can request.

-   `UPDATE_VALUE`: Change the value of an existing key.
-   `ADD_TO_LIST`: Add a new item to a list.
-   `REMOVE_FROM_LIST`: Remove an existing item from a list.
-   `ADD_KEY_VALUE`: Add a new key-value pair to a dictionary.
-   `REMOVE_KEY`: Remove a key-value pair from a dictionary.

### 2.2. `EditOperation` (Pydantic Model)

Represents a single, atomic change to the CV data.

| Field                   | Type                           | Description                                                                                             |
| :---------------------- | :----------------------------- | :------------------------------------------------------------------------------------------------------ |
| `operation_type`        | `EditOperationType`            | The type of edit to perform.                                                                            |
| `path`                  | `List[Union[str, int]]`        | The JSON path to the element to modify (e.g., `["experience", 0, "highlights"]`).                     |
| `value`                 | `Any`                          | The new value for `UPDATE_VALUE` or the item to add for `ADD_TO_LIST`. Optional.                        |
| `key_for_add_or_remove` | `str`                          | The key to add/remove for `ADD_KEY_VALUE` or `REMOVE_KEY`. Optional.                                    |
| `description`           | `str`                          | The LLM's justification for this specific change.                                                       |

### 2.3. `PlannedChanges` (Pydantic Model)

The top-level object returned by the CV Customization Agent.

| Field                 | Type                  | Description                                                              |
| :-------------------- | :-------------------- | :----------------------------------------------------------------------- |
| `edits`               | `List[EditOperation]` | A list of all the edit operations to be applied.                         |
| `overall_explanation` | `str`                 | A high-level summary from the LLM explaining the strategy for the edits. |

---

## 3. Core Algorithms & Logic

This logic is critical for the system's function and must be replicated.

### 3.1. YAML Editor Logic

The `YamlEditor` is responsible for safely applying the `PlannedChanges` to the CV data. Its `apply_edits` method must perform the following steps for each `EditOperation`:

1.  **Deep Copy:** Work on a deep copy of the source data to avoid side effects.
2.  **Navigate Path:** Traverse the data structure using the `path` from the `EditOperation`.
3.  **Apply Operation:** Execute the change based on the `operation_type`.
4.  **Special Case for Experience Highlights:** When performing an `ADD_TO_LIST` operation on an `experience` entry's `highlights` list, the logic must be nuanced:
    *   If the new highlight to be added is a "Used: ..." summary of technologies, it should be placed at the *end* of the list.
    *   If a "Used: ..." highlight *already exists*, it should be replaced with the new one.
    *   Any other new highlight should be inserted *before* an existing "Used: ..." highlight, or at the end if one doesn't exist.
5.  **Schema Validation:** After each successful edit, the modified data structure must be validated against the `cv_schema.json` to ensure its integrity. If validation fails, the edit must be rolled back, and an error should be logged.

### 3.2. Slugification

A `slugify` function is required to create URL- and path-safe identifiers from free-form text (e.g., company and position names).

-   **Input:** A string (e.g., "Senior Software Engineer (Backend)").
-   **Logic:**
    1.  Convert the string to lower-case.
    2.  Replace all whitespace, underscores, and non-alphanumeric characters with a hyphen (`-`).
    3.  Collapse multiple consecutive hyphens into a single one.
    4.  Trim any leading or trailing hyphens.
    5.  Truncate to a maximum length (e.g., 50 characters).
-   **Output:** A slug (e.g., `senior-software-engineer-backend`).

---

## 4. Prompt Engineering Blueprints

Replicating the LLM's high-quality output depends entirely on providing it with the correct instructions. The following principles must be embedded in the system prompts for the relevant agents.

### 4.1. For the CV Customization Agent

**Core Principle:** You are an expert Professional Resume Writer. Your goal is to strategically edit a base CV to maximize its appeal for a specific job offer, while ensuring **100% of the factual information is sourced exclusively from the Base CV.**

**Key Directives:**

1.  **Truthfulness:** The Base CV is the *sole source of truth*. Do not invent, embellish, or infer skills or experiences not explicitly present in the Base CV.
2.  **Experience Integrity:** Retain all experience entries from the Base CV. The task is to rephrase, reorder, and highlight existing content, not to add or remove jobs.
3.  **Technology Sourcing:** When creating a "Used: ..." list for a specific job experience, the technologies listed *must* be derived *only* from the description of that same job experience in the Base CV.
4.  **Realistic Matching:** Use the job offer as a guide to identify which aspects of the Base CV are most relevant. The output must be a realistic representation of the candidate's existing profile.
5.  **Structured Output:** The final output must be a single JSON object conforming to the `PlannedChanges` schema.

### 4.2. For the Job Offer Parsing Agent

**Core Principle:** You are a data extraction tool. Your task is to analyze the provided text and populate a structured `JobOffer` model.

**Key Directives:**

1.  **Strict Extraction:** Extract information *only* from the provided text. Do not infer or assume any information.
2.  **Handle Missing Data:** If a piece of information for a field is not present, its value in the output JSON must be `null`.
3.  **Empty Lists:** If no technologies are mentioned, the `technologies` field should be an empty list (`[]`).
4.  **Dates:** If a `discovered` date is not present, it should be `null`.
