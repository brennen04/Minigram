# Architecture

## System Style

- AdonisJS full-stack modular monolith
- React page components connected through Inertia.js
- Server-driven routing with SPA-like navigation after the initial page load
- Presentation, application, and data tiers
- No separate frontend application or public REST API for normal page flows
- No React Router, microservices, or server-side rendering

Inertia is the bridge between AdonisJS and React. AdonisJS owns routes, authentication, validation, database access, and business rules. React renders pages and manages local interaction. Controllers render named React pages with serializable props instead of exposing a separate JSON API.

## Current Workspace Transition

The existing standalone `frontend/` Vite scaffold was created before the Inertia decision. It is not the target architecture and must not receive further feature work. In the next explicitly approved implementation step, replace it with a single AdonisJS React/Inertia application after explaining the starter structure and migration impact.

## High-Level Flow

```text
Browser
  |
  | HTTP request or Inertia visit
  v
AdonisJS route -> middleware -> controller
  |
  +--> Lucid ORM -> SQLite
  +--> Image storage
  |
  | inertia.render(page, props)
  v
React page component
  |
  v
Browser UI
```

The first visit returns an HTML shell containing the Inertia page data. Later Inertia navigation and form submissions use background HTTP requests and swap React page components without a full browser reload. This is not a separately designed REST API even though Inertia uses HTTP and JSON internally.

## Request Flow

```text
HTTP Request
  -> AdonisJS route
  -> Middleware
  -> Validator
  -> Controller
  -> Service/business logic when justified
  -> Lucid model
  -> SQLite
  -> inertia.render(page, props) or redirect
  -> React page
```

Do not create a service abstraction for trivial CRUD unless it improves reuse, testability, or clarity.

## Application Routes

These URLs are registered in AdonisJS. Inertia renders the corresponding React page; React Router is not used.

```text
/login
/register
/
/posts/:id
/users/:username
/settings/profile
```

The create-post interface should be a navigation modal/dialog or `/posts/create`; choose the simpler implementation.

## Project Organization

The React code lives inside the AdonisJS application under `inertia/`. Prefer page components plus feature-oriented supporting code:

```text
app/
├── controllers/
├── middleware/
├── models/
└── validators/
database/
├── migrations/
└── seeders/
inertia/
├── app.tsx
├── components/
│   └── shared/
├── features/
│   ├── auth/
│   ├── posts/
│   ├── comments/
│   ├── users/
│   └── follows/
├── pages/
│   ├── auth/
│   ├── posts/
│   ├── users/
│   └── settings/
└── types.ts
resources/views/
└── inertia_layout.edge
start/
└── routes.ts
```

- Keep feature-specific components in their feature folder.
- Put only genuinely generic components in `components/shared`.
- Keep routable screens in `inertia/pages`.
- Use typed Inertia props or generated transformer types for server data.
- Prefer composition over oversized page components.

## State Management

Use React state for local UI state such as dialogs, temporary input state, tabs, and other transient interactions. AdonisJS remains the source of truth for posts, comments, profiles, likes, follows, and authentication. Deliver server state through Inertia page props, shared props, redirects, and partial reloads.

Use Inertia `Form`, `Link`, and router helpers for standard navigation and mutations. Do not add React Router, Redux, TanStack Query, Fetch, or Axios for ordinary Inertia page flows without a concrete need.

## Image Handling

Do not store image binary data in SQLite. Store an image URL or path instead. Local filesystem storage is acceptable for development; production may later use object/image storage. The backend must validate file type, file size, and ownership where relevant. Inertia forms should submit images using `multipart/form-data`.
