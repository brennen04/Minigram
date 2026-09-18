# Database Schema

SQLite is the local-development and MVP database. Keep persistence logic database-agnostic enough to support a later PostgreSQL migration. Prefer migrations over manual database changes, and use database constraints for invariants that belong in the database.

## `users`

```text
id
username
email
password
bio
avatar_url
created_at
updated_at
```

Constraints: unique `email`; unique `username`.

## `posts`

```text
id
user_id
image_url
caption
created_at
updated_at
```

Foreign key: `user_id -> users.id`.

## `comments`

```text
id
user_id
post_id
content
created_at
updated_at
```

Foreign keys: `user_id -> users.id`; `post_id -> posts.id`.

## `likes`

```text
user_id
post_id
created_at
```

Constraint: unique `(user_id, post_id)`.

## `follows`

```text
follower_id
following_id
created_at
```

Constraints: unique `(follower_id, following_id)`; application rule `follower_id != following_id`.
