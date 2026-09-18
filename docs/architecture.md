# Architecture

## System Style

- Client-server architecture
- React Single-Page Application (SPA)
- REST API backend
- Presentation, application, and data tiers
- AdonisJS modular monolith
- No microservices and no server-side rendering

## High-Level Flow

```text
Browser
  |
  v
React SPA
  |
  | HTTPS / REST / JSON
  v
AdonisJS API
  |
  | Lucid ORM
  v
SQLite

AdonisJS
  |
  +--> Image storage
```

## Backend Request Flow

```text
HTTP Request
  -> Route
  -> Middleware
  -> Validator
  -> Controller
  -> Service/business logic when justified
  -> Lucid model
  -> SQLite
```

Do not create a service abstraction for trivial CRUD unless it improves reuse, testability, or clarity.

## Frontend Routes

```text
/login
/register
/
/users/:username
/settings/profile
```

The create-post interface should be a navigation modal/dialog or `/posts/create`; choose the simpler implementation.

## Frontend Organization

Prefer feature-oriented organization:

```text
src/
├── app/
│   ├── App.tsx
│   └── router.tsx
├── features/
│   ├── auth/
│   ├── posts/
│   ├── comments/
│   ├── users/
│   └── follows/
├── components/
│   └── shared/
├── pages/
├── lib/
│   └── apiClient.ts
└── main.tsx
```

- Keep feature-specific components in their feature folder.
- Put only genuinely generic components in `components/shared`.
- Use TypeScript interfaces or types for API-facing models.
- Prefer composition over oversized page components.

## State Management

Use React state for local UI state such as dialogs, form values, tabs, and transient state. Server-backed data includes posts, comments, profiles, likes, and follows. Prefer TanStack Query, or React state plus Fetch/Axios for very small flows. Do not add Redux by default.

## Image Handling

Do not store image binary data in SQLite. Store an image URL or path instead. Local filesystem storage is acceptable for development; production may later use object/image storage. The backend must validate file type, file size, and ownership where relevant.
