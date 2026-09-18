# AGENTS.md

## Project

**Name:** MiniGram  
**Type:** Small full-stack portfolio web app  
**Goal:** Build a lightweight Instagram clone that demonstrates solid software-engineering fundamentals without unnecessary complexity. MiniGram is also a learning project for a developer gaining web-development experience.

## Required Tech Stack

### Frontend

- React and TypeScript
- React Router
- Tailwind CSS and shadcn/ui

### Backend

- AdonisJS and TypeScript
- Lucid ORM
- AdonisJS authentication and validation

### Database and Tooling

- SQLite for local development and the MVP
- Git and GitHub
- ESLint and Prettier
- `.env` configuration

Do not change the required stack without explicit approval.

## Required Reference Documents

Read the document relevant to the work before making a change:

- [Product scope](docs/product-scope.md): MVP features, non-goals, milestones, definition of done, and open decisions.
- [Architecture](docs/architecture.md): client-server design, request flow, routes, frontend organization, state, and image handling.
- [API contract and application rules](docs/api-contract.md): endpoints, response shapes, authentication, authorization, validation, errors, and test priorities.
- [Database schema](docs/database-schema.md): data model, foreign keys, and database constraints.

## Non-Negotiable Engineering Rules

- Use a React SPA, REST API, and AdonisJS modular monolith. Do not add microservices or server-side rendering.
- Keep images outside SQLite; persist an image URL or path instead.
- Use migrations instead of manual database changes.
- Keep secrets out of source control and update `.env.example` when configuration changes.
- Use backend authorization for ownership and social-relationship rules; frontend checks are only UX.
- Preserve TypeScript type safety and avoid dependencies without a concrete reason.
- Do not add Redux by default or introduce speculative infrastructure.
- Prefer framework-native AdonisJS and React patterns, simple designs, and focused changes.

## Learning-First Development

Do not complete an entire milestone in one pass. For each milestone:

1. Explain the relevant web-development concept before coding.
2. Break the milestone into small, logical implementation steps.
3. Implement only one logical, testable step at a time.
4. Explain important code, including how data moves between the browser, React, the API, and the database when relevant.
5. Give the developer a concrete manual test to perform.
6. Stop and wait for explicit approval before continuing.

Prioritize understanding over speed. Explain decisions, alternatives, and tradeoffs in plain language. Avoid unexplained framework magic and large code dumps.

## Agent Operating Instructions

Before a substantial change:

1. Identify the milestone and inspect the existing implementation.
2. Read the relevant reference documents above.
3. Preserve the existing architecture and make the smallest coherent change.
4. Add or update tests when behavior changes.
5. Document a decision when it becomes final.

When uncertain, prefer the simpler implementation. Ask only when the ambiguity would materially change architecture or product scope. Do not rewrite working areas solely for style.
