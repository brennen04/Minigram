# Product Scope

## MVP Features

### Authentication

- Register
- Login
- Logout
- Persist authenticated session across refreshes

### Profiles

- View profile
- Edit own profile
- Set avatar
- Set short bio
- View user's posts
- View follower/following counts

### Posts

- Create post
- Upload exactly one image
- Optional caption
- View posts
- Delete own post

### Feed

- Reverse chronological order
- Initial MVP may show all posts
- No recommendation algorithm

### Likes

- Like post
- Unlike post
- Show like count
- Prevent duplicate likes

### Comments

- Add comment
- View comments
- Delete own comment

### Following

- Follow user
- Unfollow user
- Show follower/following counts
- Prevent self-follow
- Prevent duplicate follows

## Explicit Non-Goals

Do not implement the following unless the project scope is explicitly expanded:

- Stories, reels, direct messages, live chat, or video upload
- Push or email notifications
- Recommendation engine, hashtags, mentions, saved posts, or advanced search
- Private accounts, social login, content moderation, admin dashboard, or multi-image posts
- Microservices, server-side rendering, distributed queues, event-driven architecture, Kubernetes, or Redis without a concrete need
- Redux unless application complexity justifies it

## Milestones

### M1 - Foundation

- Initialize frontend and backend
- Configure TypeScript, SQLite, React Router, Tailwind CSS, shadcn/ui, ESLint, and Prettier
- Set repository structure and configure CORS

### M2 - Authentication

- Create users table and model
- Choose authentication mechanism
- Implement register, login, logout, and `/auth/me`
- Add protected frontend routes

### M3 - Posts

- Create posts table and model
- Implement image upload, post creation, retrieval, and own-post deletion

### M4 - Feed

- Define feed response contract and pagination
- Build `PostCard` and feed page
- Add loading and empty states

### M5 - Social Interaction

- Implement likes and comments

### M6 - Profiles

- Implement profile API and profile page
- Implement own-profile editing and avatars

### M7 - Following

- Create follows table and model
- Implement follow/unfollow API, counts, and frontend integration

### M8 - Quality

- Complete validation, error handling, responsive design, accessibility pass, and core tests

### M9 - Deployment

- Choose hosting and persistent database strategy
- Choose production image storage
- Deploy frontend and backend with HTTPS and environment variables

### M10 - Portfolio Polish

- Complete README, architecture diagram, screenshots, demo account, deployed URL, technical decisions, and known limitations

## Definition of Done

The MVP is complete when a new user can:

1. Register and log in.
2. Remain authenticated after refresh.
3. Edit their profile and upload an image post.
4. Browse the feed, like or unlike posts, and comment.
5. View another user's profile and follow or unfollow them.
6. Delete their own post and log out.

It must also block unauthorized resource modification, handle validation failures cleanly, work at desktop and mobile sizes, use SPA navigation and REST communication, store images outside SQLite, include setup instructions, and be demonstrable from a clean account.

## Decisions Still Open

Resolve these only when implementation requires them. Choose the simplest option that satisfies the MVP, document the decision, and avoid unrelated architecture changes.

- Session versus access-token authentication
- TanStack Query versus simpler API state handling
- Production image-storage provider
- Deployment provider
- Pagination and API error formats
- Exact validation and upload limits
- Public-profile visibility for unauthenticated users
- Whether the feed stays global or becomes followed-users only after the MVP
