# GitHub Copilot - Core Architectural Instructions

**Core Guiding Principle for Copilot:** Adhere to the architectural vision. This document provides core architectural directives. More detailed general coding principles and language-specific conventions are provided in other instruction files that the IDE makes available to you based on the current file context or task.

---

## Core Architecture & Key Technologies

- **Clean Architecture Mandate**:
  - Strict layer separation: `Domain` -> `Application` -> `Infrastructure` / `Presentation (API, Clients)`.
  - Dependency Rule: Dependencies flow inwards ONLY.
  - Boundaries are defined with interfaces in inner layers, implemented by outer layers.
- **CQRS & Event Sourcing (ES) - Architectural Blueprint**:
  - **CQRS Philosophy**: Operations that change state (Commands) are segregated from operations that read state (Queries).
  - **Event Sourcing Approach**: Core domain entities employ event sourcing; domain events are the source of truth for their state changes.
  - **Persistence Strategy**: Marten (on PostgreSQL) is the designated technology for event storage and read model projections.
- **Key Technologies & Strategic Roles**:
  - **.NET (C#)**: Primary backend technology stack.
  - **ASP.NET Core**: Framework for `Foundation.Api`.
  - **SignalR**: Real-time communication for client-server updates and collaborative features.
  - **PostgreSQL (via Marten)**: Primary data persistence solution.
  - **Redis**: Strategic caching solution.
  - **Semantic Kernel**: Core of the AI layer (`Foundation.AI`).
  - **Docker**: Containerization for consistent development and deployment environments.
- **Configuration Management**:
  - Environment-based configuration using .env files and configuration providers.
  - No hardcoded environment-specific values.
- **Data-Oriented Programming (DOP) - Architectural Stance**:
  - The architecture favors clear, immutable data structures and transformations.
- **Error Handling & Optionality - Architectural Stance**:
  - The architecture mandates explicit signaling for fallible operations using FluentResults.
  - Use `Result<T>` for operations that can fail, and `Result<T, TError>` for specific error types.
  - Avoid nullable returns where FluentResults can provide better error context.
- **Architectural Quality: Testability**:
  - All system components MUST be designed with testability as a primary concern, influencing choices towards modularity, clear interfaces, and dependency injection.
  - All production code must have corresponding tests in the appropriate test project.
- **Architectural Process: Decision Records**:
  - Significant architectural decisions impacting the overall system design or choices of core technologies are formally documented. (The practice of writing these records is detailed in general coding guidelines).

## 2. Critical "Must Avoids" (Global Rules for Copilot)

- **Clean Architecture Violations**: No direct references from inner to outer layers.
- **Service Locator Pattern / Direct Instantiation of Dependencies**: Use constructor-based Dependency Injection with abstractions.
- **Hard-Coding Configuration**: All configuration MUST be sourced externally.
- **Environment-Specific Code**: No hardcoded environment-specific values; use configuration providers.
- **Implicit Error Handling / Nulls for Optionality**: Adhere to the explicit error handling and optionality patterns established architecturally.
- **Mutable Static State**: Avoid due to testability and concurrency issues.
- **Unmanaged `DateTime.Now` / `DateTime.UtcNow`**: Use an `IDateTimeProvider` abstraction for testable time-dependent logic.
- **Untested Code**: All production code must have corresponding tests in the appropriate test project.

## 3. Understanding the Hierarchy & Context of Instructions

- **This file (`copilot-instructions.md`) provides foundational architectural directives for Foundation.** It is always in context for you.
- **Contextual Instructions**: You will automatically receive more detailed, context-relevant guidance from other files based on the file you are working on (e.g., from `.github/instructions/general-coding.instructions.md` for general principles, or `.github/instructions/csharp.instructions.md` for C# specifics).
- **Task-Specific Instructions**: When a specific Copilot feature is invoked (e.g., via IDE context menus for code generation, test generation, commit message generation, etc.), the IDE settings link that feature to a dedicated `copilot-[task].md` file. This file provides focused instructions for that operation, drawing upon the architectural and contextual guidelines.
- **Precedence**: More specific instructions (e.g., in `csharp.instructions.md` for C# code, or in a `copilot-[task].md` file for that task) take precedence over more general instructions in this file if an apparent conflict arises for that specific context or task.
- **Primary Goal**: Generate clear, maintainable, robust code and content that strictly adheres to the established patterns and architectural vision. If guidance seems incomplete or conflicting for a specific, nuanced scenario, state this and ask for developer clarification.

## 4. Development Workflow & Practices

- **Code Reviews**: Follow guidelines in `.github/copilot-code-review.md`
- **Pull Requests**: Adhere to PR template and `.github/copilot-pr-title-and-description.md`
- **Testing**: All features must have tests; see `.github/copilot-test-generation.md`
- **Commit Messages**: Follow Conventional Commits as specified in `.github/copilot-commit-message-generation.md`
- **Project Structure**: Refer to `plan.md` for detailed project organization and folder structure
- **Solution Organization**:
  - Source code in `src/` directory with individual projects
  - Tests in `tests/` directory mirroring the source structure
  - Documentation in `docs/` directory
  - Media assets in `media/` directory

## 5. Community & Support

- **Code of Conduct**: All contributions must adhere to `CODE_OF_CONDUCT.md`
- **Security**: Report security issues as outlined in `SECURITY.md`
- **Support**: See `SUPPORT.md` for getting help and community resources
