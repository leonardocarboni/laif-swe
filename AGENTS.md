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

## Company-Specific Development Cycle
When working on company applications, the agent should follow this cycle:

1.  **PRD Analysis**: Parse and understand Product Requirements Documents to extract key features, user stories, and technical requirements.
2.  **Architecture Planning**: 
    - Design backend APIs and database schema following company patterns.
    - Plan the frontend component structure and state management strategy.
3.  **Backend Development**: 
    - Create Python FastAPI/Django backend with proper OpenAPI docs.
    - Implement database models and migrations.
    - Create RESTful API endpoints with standardized error handling and logging.
4.  **Frontend Development**:
    - Generate Next.js 15 components with TypeScript and Tailwind CSS.
    - Implement API integration using company patterns.
    - Apply styling consistent with the company's design system.
5.  **AWS Deployment Preparation**:
    - Create Docker configurations for containerization.
    - Generate AWS infrastructure configurations (e.g., CloudFormation, CDK).
    - Prepare deployment scripts for ECS/Lambda environments.
6.  **Testing & Validation**:
    - Generate comprehensive test suites for both backend and frontend.
    - Validate API contracts against OpenAPI specifications.
    - Ensure responsive design and accessibility standards are met.

## Company-Specific Development Patterns

### Backend Patterns (Python)
- **Pydantic Models**: Use Pydantic models for request/response validation to ensure data integrity.
- **Standardized Error Handling**: Implement standardized error handling and logging across all endpoints.
- **Database Naming Conventions**: Follow company database naming conventions for tables, columns, and relationships.
- **Dependency Injection**: Use dependency injection for services to promote modularity and testability.
- **OpenAPI Documentation**: Generate OpenAPI documentation automatically to ensure APIs are well-documented.

### Frontend Patterns (Next.js 15)
- **TypeScript Strict Mode**: Use TypeScript with strict mode enabled to catch common errors.
- **Client-Generated Queries**: Implement client-generated queries from OpenAPI specifications to ensure consistency.
- **Component Architecture**: Follow company component architecture patterns for reusability and maintainability.
- **State Management**: Use standardized state management (Zustand/Context) for predictable state transitions.
- **Error Boundaries**: Implement proper error boundaries and loading states to improve user experience.

### AWS Deployment Patterns
- **Multi-Stage Docker Builds**: Containerize applications using multi-stage Docker builds to create lean, secure images.
- **ECS and Lambda**: Use ECS for stateful services and Lambda for serverless functions to optimize for scalability and cost.
- **Environment Variable Management**: Implement proper environment variable management using AWS Secrets Manager or Parameter Store.
- **RDS Connection Pooling**: Use RDS for PostgreSQL with proper connection pooling to manage database connections efficiently.
- **S3 and CloudFront**: Store static assets in S3 with a CloudFront distribution for fast, reliable delivery.
