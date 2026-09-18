# Web Routes and Application Rules

## Routing Style

AdonisJS owns every application route. Controllers render React pages with Inertia props or redirect after successful mutations. Do not create `/api` endpoints or a frontend API client for normal MiniGram page flows.

Use named routes so Inertia `Link`, `Form`, and router helpers can navigate and submit data without hardcoded URLs. Standard HTTP methods still express intent, and image uploads use `multipart/form-data`.

## Page Routes

```http
GET /                       feed.index
GET /register               auth.register.create
GET /login                  auth.login.create
GET /posts/:id              posts.show
GET /users/:username        users.show
GET /settings/profile       profile.edit
```

Page controllers call `inertia.render(page, props)`. The initial request returns the HTML shell and page data; subsequent Inertia visits return an Inertia page response and update the React view without a full reload.

## Action Routes

### Authentication

```http
POST /register              auth.register.store
POST /login                 auth.login.store
POST /logout                auth.logout
```

### Posts

```http
POST   /posts               posts.store
DELETE /posts/:id           posts.destroy
```

### Likes

```http
POST   /posts/:id/likes     likes.store
DELETE /posts/:id/likes     likes.destroy
```

### Comments

```http
POST   /posts/:id/comments  comments.store
DELETE /comments/:id        comments.destroy
```

### Profile and Following

```http
PATCH  /settings/profile    profile.update
POST   /users/:id/follow    follows.store
DELETE /users/:id/follow    follows.destroy
```

Successful action routes normally redirect to an appropriate page or back to the current page. Validation errors and flash messages should be returned through the Inertia workflow rather than a custom JSON error envelope.

## Page-Prop Guidance

Provide each page with the data required for its first useful render. Avoid making the React page issue follow-up requests for basic content. Use transformers or explicit serialization to control which model fields reach the browser and to preserve end-to-end TypeScript types.

Example feed post prop:

```json
{
  "id": 42,
  "imageUrl": "/uploads/example.jpg",
  "caption": "Hello",
  "createdAt": "2026-09-18T08:32:00Z",
  "author": {
    "id": 8,
    "username": "brennen",
    "avatarUrl": "/uploads/avatar.jpg"
  },
  "likesCount": 17,
  "commentsCount": 3,
  "isLikedByCurrentUser": true
}
```

Share only genuinely global props, such as the authenticated user and one-time flash messages. Keep page-specific data in the corresponding controller response.

## Authentication

Use AdonisJS session/cookie authentication. This is the default architectural decision for the Inertia application because pages and actions are served by the same AdonisJS application and origin. Do not add access tokens or a custom JWT system for browser authentication.

Guest middleware protects registration and login pages. Auth middleware protects logout, settings, post mutations, likes, comments, and follows. The authenticated user may be exposed to React as a typed shared Inertia prop.

## Authorization Rules

The backend is the source of truth. Enforce server-side that users can edit only their own profile, delete only their own posts and comments, cannot follow themselves, and cannot create duplicate likes or follows. React visibility rules are UX only; they must not replace backend authorization.

## Validation Rules

### Registration

- Valid and unique email
- Unique username with a length constraint
- Minimum password length

### Posts

- Required image
- Supported image MIME type and upload-size limit
- Caption maximum length

### Comments

- Non-empty content with a maximum length

### Profile

- Bio maximum length
- Valid image avatar

Choose exact numeric limits during implementation and document them. Use AdonisJS validators and surface validation messages through Inertia forms.

## Error Handling

Use appropriate HTTP status codes and redirects without leaking stack traces or secrets in production. React pages must present readable validation, authorization, expired-session, and server-failure states. Use Inertia error and flash-message mechanisms before inventing a custom error format.

## Testing Priorities

Prioritize high-value server behavior over coverage percentage:

- Registration, login, logout, and authenticated page protection
- Correct Inertia page components and essential props
- Creating a post and blocking unauthorized post deletion
- Like/unlike and duplicate-like prevention
- Follow/unfollow, duplicate-follow prevention, and self-follow prevention
- Profile ownership rules

Frontend component tests are secondary to core AdonisJS route and authorization behavior for the MVP. Manual end-to-end testing is acceptable before deployment.
