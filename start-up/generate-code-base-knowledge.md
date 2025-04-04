# Purpose
*   To analyze an unfamiliar or undocumented codebase and create/update the `docs/codebase_context.md` file.
*   This document serves as a high-level guide to the project's structure, key components, internal dependencies, entry points, and conventions for developers and AI agents.

# Process: Analyzing the Codebase
*   **Goal:** Understand the project's layout, main components, internal interactions, and how it operates. Assume limited prior documentation exists.
*   **Initial Scan:**
    *   List top-level directories and files. Infer their purpose based on names (e.g., `src`, `lib`, `app`, `tests`, `docs`, `scripts`, `config`, `data`, `.github`, `docker`).
    *   Identify the primary source code directory/directories.
    *   Locate dependency management file(s) (e.g., `pyproject.toml`, `requirements.txt`, `package.json`, etc.) and note key *external* frameworks or libraries.
    *   Identify the testing directory and conventions.
*   **Source Code Exploration:**
    *   Identify potential application entry points (main scripts, CLIs, server startups).
    *   Identify major modules/packages/namespaces within the source code root(s). Group them by apparent responsibility.
    *   **Dependency Mapping:** For each major module/component identified, examine its imports and function/class calls to determine which *other internal* modules/components it depends on. Note these relationships. (e.g., Does `feature_a` import from `core/database`? Does `api` call functions in `services`?)
    *   Identify configuration loading mechanisms.
    *   Identify how external services (databases, external APIs) are accessed.
*   **Synthesize Findings:** Consolidate observations into a structured understanding, including the inferred dependency graph between internal modules.

# Output: Structuring `docs/codebase_context.md`
*   Create or update `docs/codebase_context.md` using Markdown.
*   Organize the document with the following sections (adapt as needed based on findings):

    *   **1. Project Overview:**
        *   Briefly summarize the inferred purpose of the project.
        *   Mention the primary programming language(s).

    *   **2. Directory Structure Overview:**
        *   **List Key Directories:** List the most important top-level directories and briefly describe their inferred purpose.
        *   **Generate Tree View:** Provide a visual representation of the project's high-level directory structure using a tree format within a Markdown code block. Focus on key architectural directories/files.
              ```
              project-root/
              ├── config/
              ├── docs/
              ├── scripts/
              ├── src/              # Main source code
              │   ├── core/         # Core shared components
              │   │   ├── database/
              │   │   └── utils/
              │   ├── features/     # Feature-specific modules
              │   │   ├── feature_a/
              │   │   └── feature_b/
              │   └── main.py       # Example entry point
              ├── tests/
              ├── .gitignore
              ├── pyproject.toml    # Example dependency file
              └── README.md
              ```
        *   **Source Code Location:** State the primary source code directory/directories.
        *   **Other Locations:** Mention locations of tests, documentation, scripts, configuration.

    *   **3. Key Modules/Components & Internal Interactions:**
        *   List the major modules/packages/namespaces found within the source code (reflecting the tree structure).
        *   For *each* key module/component, provide:
            *   **Purpose:** A brief description of its inferred role or responsibility.
            *   **Internal Dependencies (Uses):** List other internal modules/components that this module *imports or calls*. (e.g., `src.core.database`, `src.core.utils`).
            *   **Internal Usage (Used By):** List other internal modules/components that *import or call this module*. (This might require searching the codebase for imports of this module; provide a best estimate, e.g., "Likely used by `src.features.feature_a`, `src.api`").
        *   **Example Entry:**
            ```markdown
            *   **`src/core/database/`**
                *   **Purpose:** Handles database connection, session management, ORM models, and CRUD operations.
                *   **Internal Dependencies (Uses):** None (or potentially `src.core.config` if DB URL is read directly).
                *   **Internal Usage (Used By):** `src.features.feature_a`, `src.features.feature_b`, potentially `scripts/db_init.py`.
            *   **`src/features/feature_a/`**
                *   **Purpose:** Implements the logic specific to Feature A.
                *   **Internal Dependencies (Uses):** `src.core.database`, `src.core.utils`.
                *   **Internal Usage (Used By):** `src.main.py` (or relevant API/CLI entry point).
            *   **`src/core/utils/`**
                *   **Purpose:** Provides common utility functions (e.g., logging setup, data formatting).
                *   **Internal Dependencies (Uses):** None.
                *   **Internal Usage (Used By):** Widely used across `src.features`, `src.core`, `scripts`.
            ```

    *   **4. Entry Points & Execution:**
        *   List identified ways to run the application (main scripts, CLIs, servers).
        *   Provide example commands.

    *   **5. Configuration:**
        *   Describe how configuration is managed.

    *   **6. External Dependencies & Libraries:**
        *   List prominent *external* frameworks/libraries.
        *   Mention the dependency management tool.

    *   **7. Testing:**
        *   Describe testing structure and frameworks.

    *   **8. Conventions & Patterns (Optional):**
        *   Note obvious coding conventions or architectural patterns.

# Continuous Improvement
*   This document is a living snapshot. Update it, especially Section 3, when refactoring occurs, dependencies change, or new modules are added.
*   Use code search tools (like `grep`, IDE search) to help identify "Internal Usage (Used By)" relationships when updating.