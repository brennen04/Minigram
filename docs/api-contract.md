# API Contract and Application Rules

## API Style

Use REST with JSON for standard requests and `multipart/form-data` for image uploads.

### Authentication

```http
POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
GET  /api/auth/me
```

### Posts

```http
GET    /api/posts
POST   /api/posts
GET    /api/posts/:id
DELETE /api/posts/:id
```

### Likes

```http
POST   /api/posts/:id/likes
DELETE /api/posts/:id/likes
```

### Comments

```http
GET    /api/posts/:id/comments
POST   /api/posts/:id/comments
DELETE /api/comments/:id
```

### Users

```http
GET   /api/users/:username
PATCH /api/users/me
```

### Following

```http
POST   /api/users/:id/follow
DELETE /api/users/:id/follow
```

## Response Guidance

Return frontend-friendly resource shapes so the feed does not require unnecessary follow-up requests.

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

## Authentication

Choose the authentication mechanism before implementing authentication. For a browser SPA, prefer secure session/cookie authentication if frontend and backend deployment make it practical. AdonisJS access-token authentication is an acceptable alternative. Do not invent a custom JWT system unless required.

## Authorization Rules

The backend is the source of truth. Enforce server-side that users can edit only their own profile, delete only their own posts and comments, cannot follow themselves, and cannot create duplicate likes or follows. Frontend visibility rules are UX only; they must not replace backend authorization.

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

Choose exact numeric limits during implementation and document them.

## Error Handling

The backend must use appropriate HTTP status codes and consistent structured errors, without leaking stack traces or secrets in production. The frontend must show readable errors and handle loading, empty, validation, unauthorized/expired-authentication, network, and server-failure states.

## Testing Priorities

Prioritize high-value behavior over coverage percentage:

- Registration, login, and authenticated routes
- Creating a post and blocking unauthorized post deletion
- Like/unlike and duplicate-like prevention
- Follow/unfollow, duplicate-follow prevention, and self-follow prevention
- Profile ownership rules

Frontend tests are secondary to core backend behavior for the MVP. Manual end-to-end testing is acceptable before deployment.
