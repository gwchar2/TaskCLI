# TaskCLINamespace
A CLI based Task Manager


taskcli/                                # Project root (Git repo top-level)
├─ CMakeLists.txt                       # Single CMake build script: declares project name, C++ standard, sources, and links SQLite (system-provided for now)
├─ include/                             # Public headers (visible to src/ and future tests/tools)
│  └─ taskcli/                          # Namespace folder to avoid header name clashes
│     ├─ cli.hpp                        # Declarations for parsing argv into a structured Command (command name + args), no implementation details
│     ├─ core.hpp                       # Declarations for Task data model and the service API (add/list/complete) that main() calls
│     └─ storage.hpp                    # Declarations for storage façade (init DB, add/list/complete) without exposing SQLite specifics
├─ src/                                 # All C++ implementation files (compiled into the taskcli executable)
│  ├─ main.cpp                          # Program entry: init storage (db path), call CLI parse, dispatch to core service, print results; zero business logic here
│  ├─ cli.cpp                           # Implementation of argument parsing (maps argv → Command), handles unknown/missing commands and usage text
│  ├─ core.cpp                          # Implementation of service functions: validates inputs, enforces simple rules, and delegates to storage
│  └─ storage_sqlite.cpp                # SQLite-backed implementation of storage.hpp (open DB, ensure schema, run SQL for add/list/complete)
├─ data/                                # Static data shipped with the app (read-only at runtime)
│  └─ schema.sql                        # DDL to create required tables on first run; versioned here for easy updates and review
├─ tests/                               # Unit/integration tests live here (can stay empty until you add a test framework)
│  └─ (empty for now)                   # Placeholder so the directory exists and CI can hook in later
├─ .github/                             # GitHub-specific automation and configuration
│  └─ workflows/                        # CI pipelines
│     └─ ci.yml                         # Windows build/test workflow: checks out repo, configures with CMake, builds Debug (extend later for tests)
├─ .vscode/                             # VS Code project settings (safe to remove if you don’t use VS Code)
│  ├─ settings.json                     # Editor/compiler settings: set C++20, enable format-on-save, and point CMake to the build folder
│  ├─ launch.json                       # Debug configuration: how F5 runs the built taskcli.exe (working directory, default args like “list”)
│  └─ tasks.json                        # One-click commands to configure and build via CMake from within VS Code (no terminal typing needed)
├─ .clang                               # Formatting rules for consistent C/C++ style across the repo (used by IDEs or clang-format CLI)
├─ .editorconfig                        # Editor-agnostic basics (indent size, trailing whitespace, newline policy, charset) applied across file types
