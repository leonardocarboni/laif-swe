This file provides guidance to the AI agent when working with code in this repository.

## Project Overview

This is a company-specific fork of Open SWE - an asynchronous coding agent built with LangGraph that autonomously understands codebases, plans solutions, and executes code changes. The agent is specialized for completing full development cycles from PRD to deployment for company web applications.

**Primary Mission**: Enable rapid prototyping and development of company web applications by autonomously completing development cycles from Product Requirements Documents (PRDs) to fully functional applications, saving hours of development time per application.

The agent is optimized to work within these technology constraints and understands common patterns used across company applications.

### Company Application Template

All company web applications follow a standardized template:
- **Backend**: Python (FastAPI/Django) with OpenAPI documentation
- **Frontend**: Next.js 15 with App Router, TypeScript, and Tailwind CSS
- **Database**: PostgreSQL with migrations
- **Infrastructure**: AWS deployment (ECS/Lambda + RDS + S3)
- **Architecture**: RESTful APIs with standardized patterns

## Agent Capabilities

This agent is designed with two core principles: self-alteration and continuous improvement. It operates on a fork of its own codebase, allowing it to modify its internal workings to adapt to new requirements or improve its performance on the company's specific technology stack.

- **Self-Altering**: The agent can read, understand, and modify its own source code. This allows it to complete tasks that may require changes to its own architecture or tools.
- **Auto-Update Capability**: Although this is a specialized fork, it is designed to pull in updates from the upstream Open SWE project. This ensures the agent benefits from the latest advancements and bug fixes from the core open-source project while maintaining its company-specific adaptations.
- **Custom Prompting**: To operate effectively within the private company's context, the agent requires a custom prompt. This prompt provides the necessary guidance on the company's development patterns, architectural standards, and specific project requirements.

## Architecture

### Monorepo Structure
This is a Yarn monorepo managed by Turbo with the following apps:
- `apps/open-swe/` - Core LangGraph agent with state graphs for manager, planner, and programmer workflows
- `apps/web/` - Next.js 15 frontend with shadcn/ui components and real-time thread management
- `apps/cli/` - Command-line interface built with Ink and React
- `apps/docs/` - Mintlify documentation site
- `packages/shared/` - Shared TypeScript types and utilities across all apps

### Core Agent Architecture (apps/open-swe)
The agent is built using LangGraph with three main state graphs:
- **Manager Graph** (`src/graphs/manager/`) - Handles GitHub issue initialization, message classification, and workflow routing
- **Planner Graph** (`src/graphs/planner/`) - Generates execution plans, determines context needs, and manages planning iterations
- **Programmer Graph** (`src/graphs/programmer/`) - Executes tasks, takes actions, handles errors, opens PRs, and includes reviewer subgraph

Key components:
- `src/tools/` - Various tools including shell execution, text editing, search, and GitHub integration
- `src/utils/` - Core utilities for GitHub API, LLM management, sandbox operations, and token handling
- `src/security/` - Authentication and GitHub security utilities
- `src/routes/` - HTTP routes and GitHub webhook handlers
<general_rules>
- Always use Yarn as the package manager - never use npm or other package managers
- Run all general commands (e.g. not for starting a server) from the repository root using Turbo orchestration (yarn build, yarn lint, yarn format)
- Before creating new utilities or shared functions, search in packages/shared/src to see if one already exists
- When importing from the shared package, use the @open-swe/shared namespace with specific module paths
- Follow strict TypeScript practices - the codebase uses strict mode across all packages
- Use ESLint and Prettier for code quality - run yarn lint:fix and yarn format before committing
- Console logging is prohibited in the open-swe app (ESLint error) - use the `createLogger` function to create a new logger instance instead
- Build the shared package first before other packages can consume it (yarn build from the root handles this automatically via turbo repo)
- Follow existing code patterns and maintain consistency with the established architecture
- Include as few inline comments as possible
</general_rules>

<repository_structure>
This is a Yarn workspace monorepo with Turbo build orchestration containing three main packages:

**apps/open-swe**: LangGraph agent application
- Core LangChain/LangGraph agent implementation with TypeScript
- Contains three graphs: programmer, planner, and manager (configured in langgraph.json)
- Uses strict ESLint rules including no-console errors

**apps/web**: Next.js 15 web interface
- React 19 frontend with Shadcn UI components (wrapped Radix UI) and Tailwind CSS
- Modern web stack with TypeScript, ESLint, and Prettier with Tailwind plugin
- Serves as the user interface for the LangGraph agent

**packages/shared**: Common utilities package
- Central workspace dependency providing shared types, constants, and utilities
- Exports modules via @open-swe/shared namespace (e.g., @open-swe/shared/open-swe/types)
- Must be built before other packages can import from it
- Contains crypto utilities, GraphState types, and open-swe specific modules

**Root Configuration**:
- turbo.json: Build orchestration with task dependencies and parallel execution
- .yarnrc.yml: Yarn 3.5.1 configuration with node-modules linker
- tsconfig.json: Base TypeScript configuration extended by all packages
</repository_structure>

<dependencies_and_installation>
**Package Manager**: Use Yarn exclusively (configured in .yarnrc.yml)

**Installation Process**:
- Run `yarn install` from the repository root - this handles all workspace dependencies automatically

**Key Dependencies**:
- LangChain ecosystem: @langchain/langgraph, @langchain/anthropic for agent functionality
- Next.js 15 with React 19 for web interface
- Shadcn UI (wrapped Radix UI) and Tailwind CSS for component library and styling
- TypeScript with strict mode across all packages
- Jest with ts-jest for testing framework

**Workspace Structure**: Dependencies are managed on a per-package basis, meaning dependencies should only be installed in their specific app/package. Individual packages reference the shared package via @open-swe/shared workspace dependency.
</dependencies_and_installation>

<testing_instructions>
**Testing Framework**: Jest with TypeScript support via ts-jest preset and ESM module handling

**Test Types**:
- Unit tests: *.test.ts files (e.g., take-action.test.ts in __tests__ directories)
- Integration tests: *.int.test.ts files (e.g., sandbox.int.test.ts)

**Running Tests**:
- `yarn test` - Run unit tests across all packages
- `yarn test:int` - Run integration tests (apps/open-swe only)
- `yarn test:single <file>` - Run a specific test file

**Test Configuration**:
- 20-second timeout for longer-running tests
- Environment variables loaded via dotenv integration
- ESM module support with .js extension mapping
- Pass-with-no-tests setting for CI/CD compatibility

**Writing Tests**: Focus on testing core business logic, utilities, and agent functionality. Integration tests should verify end-to-end workflows. Use the existing test patterns and maintain consistency with the established testing structure.
</testing_instructions>




