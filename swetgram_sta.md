# Swetgram Technical Architecture (STA)

**Document version:** 1.0  
**Product:** Swetgram  
**Document type:** Company-level Technical Architecture  
**Status:** Draft / Implementation-ready baseline  
**Last updated:** 2026-07-07  
**Owner:** Swetgram Product & Engineering  

---

## 0. Executive Summary

Swetgram is a messenger available on Android and iOS as two independent, fully native applications. The product combines fast private messaging, group communities, and a lightweight public text-based discovery surface called **Discover**.

The technical architecture is optimized for:

- fast MVP delivery;
- low infrastructure cost;
- Android + iOS support from v1;
- realtime messaging;
- simple operational maintenance;
- future migration from home server / staging to a production Hetzner EU VPS;
- privacy-conscious account creation using email + username instead of phone numbers.

The chosen architecture is a **modular monolith backend** written in Go, with PostgreSQL as the source of truth, Centrifugo for realtime delivery, and Cloudflare R2 for media. The mobile client is split into two fully separate, fully native applications: **Android (Kotlin + Jetpack Compose)** and **iOS (Swift + SwiftUI)**, sharing no code. Both clients consume the same REST API and Centrifugo realtime contract, and both implement the Swetgram Design System independently.

---

## 1. Architecture Decisions

### 1.1 Final Decisions

| Area | Decision |
|---|---|
| Backend language | Go |
| HTTP framework | Fiber |
| API style | REST API |
| Backend architecture | Modular monolith |
| Database | PostgreSQL 16+ |
| SQL layer | sqlc |
| Realtime | Centrifugo |
| Client strategy | Two fully separate native apps, no shared code |
| Android client | Kotlin + Jetpack Compose |
| iOS client | Swift + SwiftUI |
| Android networking | Retrofit + OkHttp |
| iOS networking | URLSession (Alamofire optional) |
| Android local storage | Room |
| iOS local storage | SwiftData (Core Data fallback) |
| Android serialization | kotlinx.serialization / Moshi |
| iOS serialization | Swift `Codable` |
| Android DI | Hilt |
| iOS DI | Swift native (constructor injection / lightweight container) |
| Auth | JWT access token + rotating refresh token |
| IDs | UUID v7 generated in backend application layer |
| Media storage | Cloudflare R2 |
| Push | FCM + APNs |
| Search v1 | PostgreSQL search |
| Analytics | Internal event pipeline first; PostHog-compatible later |
| Logs/monitoring | Structured logs + health checks + metrics |
| Development infrastructure | Home server |
| Production infrastructure | Hetzner VPS in EU before production launch |
| Deployment | Docker Compose |
| Reverse proxy | Caddy |
| Delete policy | Hard delete for user content, minimal audit/report records retained |

---

## 2. Product Context

### 2.1 Core Product Modules

Swetgram v1 contains four primary user-facing modules:

1. **Chats**  
   Private 1:1 conversations and group chats.

2. **Discover**  
   Chronological public text posts, comments, likes, sharing, and reporting.

3. **Groups**  
   Communities with their own chat, internal Discover, and member list.

4. **Profile**  
   Identity, settings, privacy controls, QR/profile link, and account management.

### 2.2 Non-Goals for v1

The following are intentionally excluded from v1:

- phone-number registration;
- channels;
- bots;
- stories;
- reels/video feed;
- marketplace;
- voice calls;
- video calls;
- voice messages;
- end-to-end encrypted secret chats;
- desktop app;
- web app;
- advanced algorithmic recommendation feed.

---

## 3. High-Level System Architecture

```text
Android App / iOS App
        |
        | REST API + Centrifugo WebSocket
        v
Caddy Reverse Proxy / TLS
        |
        +-------------------+
        |                   |
        v                   v
Go Fiber API          Centrifugo
        |                   ^
        | publish events    |
        v                   |
PostgreSQL 16+  <----------+
        |
        +--> Cloudflare R2 for media
        |
        +--> FCM / APNs for push
        |
        +--> Admin tools / moderation
```

### 3.1 Data Flow Principles

- PostgreSQL is the source of truth.
- Durable writes always go through the Go backend.
- Clients do not write directly to Centrifugo for persistent entities.
- The backend writes to PostgreSQL first, then publishes realtime events through Centrifugo.
- Offline users receive push notifications through FCM/APNs.
- Each client uses its own native local database (Room on Android, SwiftData on iOS) as a cache for fast UI and offline-friendly reads.

---

## 4. Client Architecture

### 4.0 Separate Native Clients Decision

Swetgram ships as **two fully independent, fully native applications** — one for Android, one for iOS. There is no shared module, no shared UI layer, and no shared business-logic layer between the two. Each platform team owns its full stack end to end.

Reason:

- native performance and platform-idiomatic UX on both OSes;
- avoids Kotlin Multiplatform / Compose Multiplatform iOS-tooling risk (previously flagged as the top architecture risk);
- each platform can use its ecosystem's mature, well-documented tooling instead of a cross-platform abstraction layer;
- easier to hire for (native Android and native iOS engineers are a larger, more standard talent pool than KMP specialists);
- platform teams can move at their own pace without one platform blocking the other.

Trade-off accepted:

- feature work is implemented twice (once per platform);
- design and business-logic consistency must be enforced through documentation (this STA + the SDS) and process, not shared code;
- two codebases to test, release, and maintain long-term.

Both clients talk to the same REST API and the same Centrifugo realtime contract, so backend work is not duplicated — only client implementation is.

### 4.1 Android Client Stack

| Layer | Technology |
|---|---|
| Language | Kotlin |
| UI | Jetpack Compose |
| HTTP | Retrofit + OkHttp |
| WebSocket / realtime client | Centrifugo Android SDK (or OkHttp WebSocket) |
| JSON | kotlinx.serialization or Moshi |
| Local DB | Room |
| Async | Coroutines + Flow |
| DI | Hilt |
| State management | MVVM + StateFlow |
| Navigation | Navigation Compose |

### 4.2 iOS Client Stack

| Layer | Technology |
|---|---|
| Language | Swift |
| UI | SwiftUI |
| HTTP | URLSession (Alamofire optional) |
| WebSocket / realtime client | Centrifugo iOS SDK (or URLSessionWebSocketTask) |
| JSON | Swift `Codable` |
| Local DB | SwiftData (Core Data as fallback) |
| Async | Swift Concurrency (async/await, `Task`) |
| DI | Lightweight native container / constructor injection |
| State management | MVVM + `@Observable` / `ObservableObject` |
| Navigation | `NavigationStack` |

### 4.3 Platform-Specific Concerns

Each client independently implements:

- push notifications (FCM on Android, APNs on iOS);
- permissions handling;
- image/file picker;
- secure token storage (Android Keystore / iOS Keychain);
- system sharing;
- app lifecycle integrations;
- OS-specific settings shortcuts.

There is no cross-platform abstraction for these — each is written natively per platform.

### 4.4 Suggested Android Module Structure

```text
swetgram-android/

  app/
    src/main/java/com/swetgram/

      core/
        network/
        realtime/
        database/
        auth/
        storage/
        analytics/
        logging/
        errors/
        time/
        permissions/

      design/
        colors/
        typography/
        spacing/
        icons/
        components/
        theme/
        motion/

      feature/
        auth/
        chats/
        chat/
        discover/
        groups/
        profile/
        settings/
        search/
        notifications/
        moderation/
```

### 4.5 Suggested iOS Module Structure

```text
swetgram-ios/

  SwetgramApp/
    Sources/

      Core/
        Network/
        Realtime/
        Database/
        Auth/
        Storage/
        Analytics/
        Logging/
        Errors/
        Time/
        Permissions/

      Design/
        Colors/
        Typography/
        Spacing/
        Icons/
        Components/
        Theme/
        Motion/

      Feature/
        Auth/
        Chats/
        Chat/
        Discover/
        Groups/
        Profile/
        Settings/
        Search/
        Notifications/
        Moderation/

  SwetgramApp.xcodeproj
```

Both repositories mirror the same top-level grouping (`core`/`design`/`feature`) so that parity between platforms is easy to audit even without shared code.

### 4.6 Client Layering

Each feature, on both platforms, should follow the same conceptual structure — only the syntax differs.

Android example:

```text
feature/chats/
  data/
    ChatsApi.kt
    ChatsLocalDataSource.kt
    ChatsRepositoryImpl.kt
  domain/
    model/
    repository/
    usecase/
  presentation/
    ChatsViewModel.kt
    ChatsUiState.kt
    ChatsScreen.kt
    components/
```

iOS example:

```text
Feature/Chats/
  Data/
    ChatsAPI.swift
    ChatsLocalDataSource.swift
    ChatsRepositoryImpl.swift
  Domain/
    Model/
    Repository/
    UseCase/
  Presentation/
    ChatsViewModel.swift
    ChatsUiState.swift
    ChatsScreen.swift
    Components/
```

Rules (apply to both platforms):

- UI never calls the API directly.
- ViewModel talks to use cases or repositories.
- Repository decides whether data comes from network or local DB.
- Local DB is used for chat list, messages, user profile cache, group cache, and draft state.
- API DTOs are separate from domain models.

### 4.7 Client State Model

Each screen uses a single immutable UI state on both platforms.

Android (Kotlin):

```kotlin
data class ChatsUiState(
    val isLoading: Boolean = false,
    val searchQuery: String = "",
    val pinnedChats: List<ChatListItem> = emptyList(),
    val chats: List<ChatListItem> = emptyList(),
    val archivedCount: Int = 0,
    val error: UiError? = null
)
```

iOS (Swift):

```swift
struct ChatsUiState {
    var isLoading: Bool = false
    var searchQuery: String = ""
    var pinnedChats: [ChatListItem] = []
    var chats: [ChatListItem] = []
    var archivedCount: Int = 0
    var error: UiError? = nil
}
```

### 4.8 Offline Behavior

v1 should support basic offline behavior on both platforms:

- show cached chat list;
- show cached messages;
- show cached profile;
- allow typing drafts locally;
- queue outgoing messages only if feasible; otherwise show clear failed-send state.

For MVP, offline sending is optional. Failed messages should remain visible with retry action. Offline behavior parity between Android and iOS should be checked explicitly in QA, since there is no shared implementation to guarantee it automatically.

---

## 5. Backend Architecture

### 5.1 Backend Style

Swetgram backend is a **modular monolith**.

This means:

- one deployable Go application;
- one database;
- clear internal modules;
- no microservices in v1;
- modules communicate through internal interfaces, not HTTP.

Why modular monolith:

- fastest to build;
- easier to debug;
- lower infrastructure cost;
- simpler local development;
- easier transactions;
- can be split into services later if needed.

### 5.2 Backend Module Structure

```text
swetgram-api/

  cmd/api/
    main.go

  internal/
    app/
      server.go
      router.go
      config.go

    platform/
      postgres/
      redis/
      centrifugo/
      r2/
      mailer/
      push/
      logger/
      metrics/

    auth/
      handler.go
      service.go
      repository.go
      tokens.go
      password.go
      email_verification.go

    users/
      handler.go
      service.go
      repository.go
      privacy.go

    chats/
      handler.go
      service.go
      repository.go
      events.go

    messages/
      handler.go
      service.go
      repository.go
      delivery.go

    groups/
      handler.go
      service.go
      repository.go
      invites.go
      roles.go

    discover/
      handler.go
      service.go
      repository.go
      comments.go
      likes.go

    media/
      handler.go
      service.go
      presign.go
      validation.go

    notifications/
      service.go
      push.go
      preferences.go

    search/
      handler.go
      service.go
      repository.go

    moderation/
      handler.go
      service.go
      reports.go
      admin.go

    analytics/
      service.go
      events.go

    admin/
      handler.go
      middleware.go

  db/
    migrations/
    queries/
    sqlc.yaml

  docker/
    Dockerfile
    docker-compose.yml
```

### 5.3 Backend Rules

- All handlers validate request input.
- All handlers require authentication unless explicitly public.
- All SQL goes through sqlc-generated queries.
- Database transactions are handled in service layer.
- Business logic does not live in handlers.
- Realtime publishing happens after successful database commit.
- Admin APIs require separate admin role and stricter audit logging.

---

## 6. Authentication & Authorization

### 6.1 Account Model

Users register with:

- email;
- username;
- display name;
- password.

Phone number is not required.

### 6.2 Token Model

Swetgram uses:

- short-lived JWT access token;
- rotating refresh token stored server-side.

Recommended durations:

| Token | Duration |
|---|---:|
| Access token | 10–15 minutes |
| Refresh token | 30 days |
| Email verification token | 30–60 minutes |
| Password reset token | 15–30 minutes |

### 6.3 Refresh Token Rotation

On refresh:

1. Client sends refresh token.
2. Server validates token hash in database.
3. Server invalidates old refresh token.
4. Server issues a new access token and new refresh token.
5. Suspicious reuse of an already-rotated token invalidates the full session family.

Refresh tokens are never stored in plaintext. Store only a hash.

### 6.4 Email Verification Policy

Users may enter the app before verifying email.

Until email is verified, restrict:

- sending messages to users who have not already contacted them;
- creating groups;
- creating Discover posts;
- commenting in Discover;
- changing username frequently.

Allowed before verification:

- browse basic app;
- edit profile;
- resend verification email;
- receive messages;
- message official onboarding/system chat if implemented.

### 6.5 Roles

Global roles:

- user;
- moderator;
- admin.

Group roles:

- member;
- admin;
- owner.

---

## 7. Database Architecture

### 7.1 Database Principles

- PostgreSQL is the primary source of truth.
- Use UUID v7 generated in Go application layer and stored as `uuid`.
- Use foreign keys for core relationships.
- Use timestamps with timezone: `timestamptz`.
- Prefer explicit status fields instead of hidden implicit state.
- Use hard delete for user content by default.
- Keep minimal audit/report records where necessary for abuse prevention and operational safety.

### 7.2 Core Tables

#### users

```sql
CREATE TABLE users (
    id uuid PRIMARY KEY,
    email text NOT NULL UNIQUE,
    username text NOT NULL UNIQUE,
    password_hash text NOT NULL,
    display_name text NOT NULL,
    avatar_url text,
    bio text,
    email_verified boolean NOT NULL DEFAULT false,
    role text NOT NULL DEFAULT 'user',
    created_at timestamptz NOT NULL DEFAULT now(),
    updated_at timestamptz NOT NULL DEFAULT now(),
    last_seen_at timestamptz
);
```

#### user_privacy_settings

```sql
CREATE TABLE user_privacy_settings (
    user_id uuid PRIMARY KEY REFERENCES users(id) ON DELETE CASCADE,
    show_online text NOT NULL DEFAULT 'everyone',
    show_last_seen text NOT NULL DEFAULT 'everyone',
    allow_messages_from text NOT NULL DEFAULT 'everyone',
    allow_profile_link boolean NOT NULL DEFAULT true,
    updated_at timestamptz NOT NULL DEFAULT now()
);
```

#### refresh_tokens

```sql
CREATE TABLE refresh_tokens (
    id uuid PRIMARY KEY,
    user_id uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    token_hash text NOT NULL UNIQUE,
    session_id uuid NOT NULL,
    family_id uuid NOT NULL,
    user_agent text,
    ip_address inet,
    expires_at timestamptz NOT NULL,
    revoked_at timestamptz,
    created_at timestamptz NOT NULL DEFAULT now()
);
```

#### push_tokens

```sql
CREATE TABLE push_tokens (
    id uuid PRIMARY KEY,
    user_id uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    token text NOT NULL,
    platform text NOT NULL CHECK (platform IN ('android', 'ios')),
    device_id text,
    created_at timestamptz NOT NULL DEFAULT now(),
    updated_at timestamptz NOT NULL DEFAULT now(),
    UNIQUE(user_id, token)
);
```

---

## 8. Chats & Messages

### 8.1 Conversation Model

A conversation can be:

- direct;
- group.

Direct conversations have exactly two members. Group conversations can have many members.

#### conversations

```sql
CREATE TABLE conversations (
    id uuid PRIMARY KEY,
    type text NOT NULL CHECK (type IN ('direct', 'group')),
    title text,
    avatar_url text,
    created_by uuid REFERENCES users(id) ON DELETE SET NULL,
    created_at timestamptz NOT NULL DEFAULT now(),
    updated_at timestamptz NOT NULL DEFAULT now()
);
```

#### conversation_members

```sql
CREATE TABLE conversation_members (
    conversation_id uuid NOT NULL REFERENCES conversations(id) ON DELETE CASCADE,
    user_id uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    role text NOT NULL DEFAULT 'member',
    muted boolean NOT NULL DEFAULT false,
    archived boolean NOT NULL DEFAULT false,
    pinned_at timestamptz,
    last_read_message_id uuid,
    joined_at timestamptz NOT NULL DEFAULT now(),
    PRIMARY KEY (conversation_id, user_id)
);
```

Pinned rule:

- max 5 pinned conversations per user;
- enforce in service layer;
- optionally enforce with database trigger later.

#### messages

```sql
CREATE TABLE messages (
    id uuid PRIMARY KEY,
    conversation_id uuid NOT NULL REFERENCES conversations(id) ON DELETE CASCADE,
    sender_id uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    type text NOT NULL CHECK (type IN ('text', 'image', 'system')),
    content text,
    media_url text,
    reply_to_message_id uuid REFERENCES messages(id) ON DELETE SET NULL,
    created_at timestamptz NOT NULL DEFAULT now(),
    edited_at timestamptz
);

CREATE INDEX idx_messages_conversation_created
ON messages(conversation_id, created_at DESC);
```

### 8.2 Message Length

Maximum text message length:

```text
4096 characters
```

### 8.3 Message Delete Policy

Swetgram v1 uses hard delete:

- deleting a message removes it from `messages`;
- related media objects are scheduled for deletion from R2;
- clients receive `message_deleted` realtime event;
- if a message had been reported, the report remains with minimal metadata and a content hash, not the full message body by default.

### 8.4 Read Status

Read state is stored per conversation member:

- `last_read_message_id`;
- optionally derive unread count by comparing message order/timestamps.

For performance at scale, add:

- `conversation_member_counters` table;
- denormalized unread counts;
- background reconciliation.

Not required for early MVP.

---

## 9. Groups

### 9.1 Group Concept

A Swetgram group is more than a chat. Each group contains:

```text
Group
  ├── Chat
  ├── Discover
  └── Members
```

A group maps to a group conversation plus group metadata.

#### groups

```sql
CREATE TABLE groups (
    id uuid PRIMARY KEY,
    conversation_id uuid NOT NULL UNIQUE REFERENCES conversations(id) ON DELETE CASCADE,
    slug text UNIQUE,
    description text,
    invite_token text UNIQUE,
    visibility text NOT NULL DEFAULT 'private' CHECK (visibility IN ('private', 'public')),
    created_by uuid REFERENCES users(id) ON DELETE SET NULL,
    created_at timestamptz NOT NULL DEFAULT now(),
    updated_at timestamptz NOT NULL DEFAULT now()
);
```

### 9.2 Group Invite Links

Invite links use random secure tokens.

Format:

```text
https://swetgram.app/join/{invite_token}
```

Rules:

- owner/admin can rotate invite token;
- invite token can be disabled;
- future: expiring invite links.

### 9.3 Group Discover

Group Discover posts are stored in the same `discover_posts` table with `scope = 'group'` and `group_id` set.

---

## 10. Discover

### 10.1 Discover Scope

Discover has two scopes:

1. **Global Discover**  
   Visible to all users.

2. **Group Discover**  
   Visible only to members of a group.

### 10.2 Feed Ranking

v1 uses chronological ordering.

No algorithmic feed in v1.

Default sort:

```sql
ORDER BY created_at DESC
```

### 10.3 Discover Tables

#### discover_posts

```sql
CREATE TABLE discover_posts (
    id uuid PRIMARY KEY,
    author_id uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    scope text NOT NULL CHECK (scope IN ('global', 'group')),
    group_id uuid REFERENCES groups(id) ON DELETE CASCADE,
    content text NOT NULL,
    created_at timestamptz NOT NULL DEFAULT now(),
    updated_at timestamptz NOT NULL DEFAULT now(),
    CHECK (
        (scope = 'global' AND group_id IS NULL) OR
        (scope = 'group' AND group_id IS NOT NULL)
    )
);

CREATE INDEX idx_discover_posts_global_created
ON discover_posts(created_at DESC)
WHERE scope = 'global';

CREATE INDEX idx_discover_posts_group_created
ON discover_posts(group_id, created_at DESC)
WHERE scope = 'group';
```

#### discover_comments

```sql
CREATE TABLE discover_comments (
    id uuid PRIMARY KEY,
    post_id uuid NOT NULL REFERENCES discover_posts(id) ON DELETE CASCADE,
    author_id uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    content text NOT NULL,
    created_at timestamptz NOT NULL DEFAULT now(),
    updated_at timestamptz NOT NULL DEFAULT now()
);

CREATE INDEX idx_discover_comments_post_created
ON discover_comments(post_id, created_at ASC);
```

Only one-level comments are supported in v1.

#### discover_likes

```sql
CREATE TABLE discover_likes (
    post_id uuid NOT NULL REFERENCES discover_posts(id) ON DELETE CASCADE,
    user_id uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    created_at timestamptz NOT NULL DEFAULT now(),
    PRIMARY KEY (post_id, user_id)
);
```

### 10.4 Discover Content Rules

v1 supports:

- text posts;
- comments;
- likes;
- share to chat;
- message author;
- report.

v1 does not support:

- post images;
- video;
- nested comments;
- hashtags;
- algorithmic ranking.

### 10.5 Discover Character Limits

Recommended:

| Entity | Limit |
|---|---:|
| Discover post | 1000 characters |
| Discover comment | 500 characters |

---

## 11. Search

### 11.1 Search Methods

Users can discover each other through:

- username;
- display name;
- QR code;
- profile link.

Email is not searchable by default.

### 11.2 v1 Search Implementation

Use PostgreSQL search:

- exact username search;
- prefix username search;
- display name ILIKE search;
- group title search;
- Discover post text search.

Recommended indexes:

```sql
CREATE INDEX idx_users_username_lower ON users (lower(username));
CREATE INDEX idx_users_display_name_lower ON users (lower(display_name));
CREATE INDEX idx_conversations_title_lower ON conversations (lower(title));
```

Optional improvement:

- PostgreSQL trigram extension for fuzzy search.

Future:

- Meilisearch / Typesense if search becomes central and PostgreSQL search is not enough.

---

## 12. Realtime Architecture

### 12.1 Centrifugo Role

Centrifugo handles:

- realtime message delivery;
- chat events;
- typing events;
- online/presence where appropriate;
- Discover live updates where needed;
- user-specific counters and notifications.

### 12.2 Channel Model

Recommended channels:

```text
user:{user_id}
chat:{conversation_id}
group:{group_id}:discover
post:{post_id}:comments
```

### 12.3 Subscription Rules

| Channel | Who can subscribe |
|---|---|
| `user:{user_id}` | only that user |
| `chat:{conversation_id}` | conversation members |
| `group:{group_id}:discover` | group members |
| `post:{post_id}:comments` | users allowed to view the post |

The backend issues Centrifugo connection tokens and subscription permissions.

### 12.4 Message Send Flow

```text
Client
  |
  | POST /conversations/:id/messages
  v
Go API
  |
  | validate membership
  | validate message
  | write message to PostgreSQL
  v
PostgreSQL commit
  |
  | publish message.created event
  v
Centrifugo chat:{conversation_id}
  |
  +--> online clients receive event
  |
  +--> offline recipients receive push via FCM/APNs
```

### 12.5 Durable vs Ephemeral Events

Durable events:

- message created;
- message deleted;
- conversation created;
- group created;
- Discover post created;
- Discover comment created;
- like count changed.

Ephemeral events:

- typing;
- presence;
- currently viewing.

Durable events must be written to PostgreSQL first. Ephemeral events can be published without persistence.

### 12.6 Event Envelope

All realtime events use a consistent envelope:

```json
{
  "event_id": "uuid-v7",
  "type": "message.created",
  "version": 1,
  "occurred_at": "2026-07-07T12:00:00Z",
  "payload": {}
}
```

---

## 13. REST API Design

### 13.1 API Versioning

All endpoints are prefixed:

```text
/api/v1
```

### 13.2 Auth Endpoints

```http
POST /api/v1/auth/register
POST /api/v1/auth/login
POST /api/v1/auth/refresh
POST /api/v1/auth/logout
POST /api/v1/auth/verify-email
POST /api/v1/auth/resend-verification
POST /api/v1/auth/forgot-password
POST /api/v1/auth/reset-password
```

### 13.3 User/Profile Endpoints

```http
GET    /api/v1/users/me
PATCH  /api/v1/users/me
GET    /api/v1/users/:username
GET    /api/v1/users/:id/card
POST   /api/v1/users/:id/block
DELETE /api/v1/users/:id/block
GET    /api/v1/users/me/qr
```

### 13.4 Chats Endpoints

```http
GET    /api/v1/conversations
POST   /api/v1/conversations/direct
POST   /api/v1/conversations/group
GET    /api/v1/conversations/:id
PATCH  /api/v1/conversations/:id
DELETE /api/v1/conversations/:id
POST   /api/v1/conversations/:id/archive
DELETE /api/v1/conversations/:id/archive
POST   /api/v1/conversations/:id/pin
DELETE /api/v1/conversations/:id/pin
GET    /api/v1/conversations/:id/messages
POST   /api/v1/conversations/:id/messages
DELETE /api/v1/messages/:id
POST   /api/v1/conversations/:id/read
POST   /api/v1/conversations/:id/typing
```

### 13.5 Groups Endpoints

```http
GET    /api/v1/groups
POST   /api/v1/groups
GET    /api/v1/groups/:id
PATCH  /api/v1/groups/:id
DELETE /api/v1/groups/:id
GET    /api/v1/groups/:id/members
POST   /api/v1/groups/:id/invite-link
POST   /api/v1/join/:invite_token
POST   /api/v1/groups/:id/members/:user_id/promote
POST   /api/v1/groups/:id/members/:user_id/remove
```

### 13.6 Discover Endpoints

```http
GET    /api/v1/discover/posts
POST   /api/v1/discover/posts
GET    /api/v1/discover/posts/:id
DELETE /api/v1/discover/posts/:id
POST   /api/v1/discover/posts/:id/like
DELETE /api/v1/discover/posts/:id/like
GET    /api/v1/discover/posts/:id/comments
POST   /api/v1/discover/posts/:id/comments
DELETE /api/v1/discover/comments/:id
POST   /api/v1/discover/posts/:id/share
POST   /api/v1/discover/posts/:id/message-author

GET    /api/v1/groups/:id/discover/posts
POST   /api/v1/groups/:id/discover/posts
```

### 13.7 Search Endpoints

```http
GET /api/v1/search?q=
GET /api/v1/search/users?q=
GET /api/v1/search/groups?q=
GET /api/v1/search/discover?q=
```

### 13.8 Media Endpoints

```http
POST /api/v1/uploads/presign
POST /api/v1/uploads/complete
DELETE /api/v1/uploads/:id
```

### 13.9 Moderation Endpoints

```http
POST /api/v1/reports
GET  /api/v1/admin/reports
GET  /api/v1/admin/reports/:id
POST /api/v1/admin/reports/:id/resolve
POST /api/v1/admin/users/:id/block
POST /api/v1/admin/users/:id/unblock
DELETE /api/v1/admin/discover/posts/:id
```

---

## 14. Media Architecture

### 14.1 v1 Media Support

v1 supports:

- avatars;
- chat images;
- group avatars.

v1 does not support:

- Discover images;
- videos;
- general file attachments.

### 14.2 Upload Flow

```text
Client
  |
  | POST /uploads/presign
  v
API validates auth + file metadata
  |
  | returns presigned R2 URL
  v
Client uploads directly to R2
  |
  | POST /uploads/complete
  v
API stores media metadata
```

### 14.3 Media Validation

Validate:

- MIME type;
- file size;
- image dimensions if possible;
- ownership;
- upload expiration.

Recommended v1 limits:

| Type | Max size |
|---|---:|
| Avatar | 5 MB |
| Chat image | 10 MB |
| Group avatar | 5 MB |

---

## 15. Push Notifications

### 15.1 Platforms

Push is required for both:

- Android via FCM;
- iOS via APNs.

### 15.2 Notification Categories

- Messages;
- Groups;
- Discover;
- Mentions;
- Security.

### 15.3 Push Rules

Send push when:

- recipient is offline;
- app is backgrounded;
- user has not muted the conversation;
- notification category is enabled.

Do not send push when:

- user is actively viewing the conversation;
- conversation is muted;
- sender is blocked;
- user disabled category notifications.

---

## 16. Moderation & Admin Panel

### 16.1 v1 Admin Panel Scope

Admin panel is intentionally small.

Must support:

- view reports;
- view reported user/post/message metadata;
- delete Discover posts;
- block users;
- resolve reports.

Can be built as:

- simple protected admin web page;
- or internal API + minimal frontend;
- or temporary tool using server-rendered Go templates.

Do not build a complex admin product in v1.

### 16.2 Reports Table

```sql
CREATE TABLE reports (
    id uuid PRIMARY KEY,
    reporter_id uuid REFERENCES users(id) ON DELETE SET NULL,
    target_type text NOT NULL CHECK (target_type IN ('user', 'message', 'discover_post', 'discover_comment', 'group')),
    target_id uuid NOT NULL,
    reason text NOT NULL,
    details text,
    content_hash text,
    status text NOT NULL DEFAULT 'open' CHECK (status IN ('open', 'reviewing', 'resolved', 'rejected')),
    assigned_to uuid REFERENCES users(id) ON DELETE SET NULL,
    created_at timestamptz NOT NULL DEFAULT now(),
    resolved_at timestamptz
);
```

### 16.3 Hard Delete + Reports

Because Swetgram uses hard delete, reports must not depend on content existing forever.

Policy:

- user-facing content is hard deleted;
- report record remains;
- report stores target id, target type, reporter id, reason, created_at, and optional content hash;
- full deleted content is not retained by default;
- if legal or safety requirements later require retention, this must be a separate policy decision.

---

## 17. Blocking & Privacy

### 17.1 Blocks Table

```sql
CREATE TABLE blocks (
    blocker_id uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    blocked_id uuid NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    created_at timestamptz NOT NULL DEFAULT now(),
    PRIMARY KEY (blocker_id, blocked_id)
);
```

### 17.2 Blocking Rules

If A blocks B:

- B cannot start a new direct conversation with A;
- B cannot see A's online status;
- B cannot see A's last seen;
- B's messages to A should be rejected or hidden depending on product decision;
- A should not receive push notifications from B;
- B can still appear in shared groups unless future policy changes.

---

## 18. Analytics

### 18.1 Analytics Goals

Analytics should answer:

- Are people registering?
- Are people sending messages?
- Are users returning?
- Are groups being created?
- Is Discover being used?
- Where do users drop off?

### 18.2 Event Table

```sql
CREATE TABLE analytics_events (
    id uuid PRIMARY KEY,
    user_id uuid REFERENCES users(id) ON DELETE SET NULL,
    event_name text NOT NULL,
    properties jsonb NOT NULL DEFAULT '{}',
    created_at timestamptz NOT NULL DEFAULT now()
);
```

### 18.3 Recommended v1 Events

```text
user_registered
email_verified
user_logged_in
message_sent
conversation_created
group_created
group_joined
discover_post_created
discover_comment_created
discover_post_liked
profile_opened
search_performed
report_created
```

### 18.4 Privacy Rule

Analytics must not store:

- message content;
- password data;
- refresh tokens;
- private email content;
- full IP histories unless needed for security logs.

---

## 19. Observability

### 19.1 Logging

Use structured JSON logs.

Every log entry should include where applicable:

- request id;
- user id;
- route;
- method;
- status code;
- latency;
- error code;
- correlation id.

Do not log:

- passwords;
- access tokens;
- refresh tokens;
- full message content;
- private media URLs.

### 19.2 Health Checks

Endpoints:

```http
GET /healthz
GET /readyz
```

`/healthz` checks process is alive.  
`/readyz` checks dependencies:

- PostgreSQL;
- Centrifugo API;
- R2 configuration;
- Redis if enabled.

### 19.3 Metrics

Minimum metrics:

- request count;
- request latency;
- error rate;
- active WebSocket connections;
- messages sent per minute;
- push success/failure;
- DB connection pool usage;
- disk usage;
- backup success/failure.

---

## 20. Infrastructure

### 20.1 Development / Staging

Initial infrastructure can run on a home server.

Use home server for:

- development environment;
- internal testing;
- prototype builds;
- staging-like environment.

Requirements:

- stable internet connection;
- static IP or dynamic DNS;
- firewall configured;
- no production user data unless security is reviewed;
- regular local backups;
- HTTPS via Caddy if exposed publicly.

### 20.2 Production

Before public production launch, migrate to Hetzner VPS in the EU.

Production baseline:

```text
Hetzner VPS, Germany/EU
Docker Compose
Caddy reverse proxy
PostgreSQL container or managed PostgreSQL later
Centrifugo container
Go API container
Redis container optional
Cloudflare R2 for media
```

### 20.3 Docker Compose Services

```yaml
services:
  api:
    image: swetgram-api
    depends_on:
      - postgres
      - centrifugo

  postgres:
    image: postgres:16
    volumes:
      - postgres_data:/var/lib/postgresql/data

  centrifugo:
    image: centrifugo/centrifugo

  redis:
    image: redis:7
    profiles: ["optional"]

  caddy:
    image: caddy
    ports:
      - "80:80"
      - "443:443"
```

---

## 21. Backup & Recovery

### 21.1 Backup Policy

Backups are mandatory.

Minimum policy:

```text
PostgreSQL: daily encrypted backup
Retention: 7 daily + 4 weekly
Media: R2 lifecycle/versioning policy where appropriate
Config/secrets: backed up separately, encrypted
Restore test: at least once per month
```

### 21.2 Backup Requirements

- Backups must be encrypted.
- Backups must not live only on the same server.
- Restore procedure must be documented.
- Backup success/failure must be monitored.

### 21.3 Recovery Targets

Initial MVP targets:

| Target | Goal |
|---|---:|
| RPO | 24 hours |
| RTO | 4–8 hours |

Later production target:

| Target | Goal |
|---|---:|
| RPO | 1–6 hours |
| RTO | < 2 hours |

---

## 22. Security Architecture

### 22.1 Transport Security

- HTTPS everywhere.
- WSS for realtime connections.
- Caddy handles TLS certificates.
- HSTS after production domain is stable.

### 22.2 Password Security

- Use Argon2id.
- Never store plaintext passwords.
- Rate-limit login attempts.
- Add password reset token expiration.

### 22.3 Token Security

- Short-lived access JWT.
- Rotating refresh tokens.
- Refresh tokens stored as hashes.
- Logout revokes refresh token.
- Suspicious refresh token reuse revokes session family.

### 22.4 API Security

- Validate every request.
- Rate-limit auth and write endpoints.
- Enforce group/chat membership server-side.
- Never trust client-provided user id.
- Use request IDs for auditability.

### 22.5 Abuse Protection

Required v1 controls:

- block user;
- report user/post/comment/message;
- admin delete post;
- admin block user;
- rate limit registration/login/posting/messaging;
- restrict unverified users.

### 22.6 E2E Encryption

E2E encryption is not part of v1.

Product must not claim:

```text
No one can read your messages.
```

Allowed positioning:

```text
Private messaging.
No phone number required.
European infrastructure.
We do not sell your data.
```

Future v2/v3 may add secret chats with a dedicated E2E design.

---

## 23. Rate Limiting

### 23.1 v1 Rate Limits

Recommended starting limits:

| Action | Limit |
|---|---:|
| Register | 5/hour/IP |
| Login failed | 10/hour/account/IP |
| Resend email verification | 3/hour/account |
| Send direct message to new user | 20/day/user until trust improves |
| Create Discover post | 30/day/user |
| Create comment | 100/day/user |
| Create group | 10/day/user |
| Reports | 30/day/user |

Store rate limits in Redis if available. Otherwise begin with PostgreSQL-backed limits for MVP.

---

## 24. Configuration & Secrets

### 24.1 Environment Variables

Use environment variables for configuration:

```text
APP_ENV
APP_PORT
DATABASE_URL
JWT_ACCESS_SECRET
JWT_REFRESH_SECRET
CENTRIFUGO_API_KEY
CENTRIFUGO_TOKEN_HMAC_SECRET
R2_ACCESS_KEY_ID
R2_SECRET_ACCESS_KEY
R2_BUCKET
SMTP_HOST
SMTP_USERNAME
SMTP_PASSWORD
FCM_CREDENTIALS_PATH
APNS_KEY_ID
APNS_TEAM_ID
APNS_BUNDLE_ID
```

### 24.2 Secrets Rules

- Never commit secrets to Git.
- Use `.env.example` for documentation.
- Rotate secrets before production.
- Use separate secrets for dev/staging/prod.

---

## 25. Deployment Strategy

### 25.1 Environments

| Environment | Purpose |
|---|---|
| local | developer machine |
| dev-server | home server testing |
| staging | pre-production testing |
| production | real users |

For early MVP, `dev-server` and `staging` may be the same home server, but production must move to a proper VPS before public launch.

### 25.2 Deployment Flow

```text
Git push
  |
  v
CI build/test
  |
  v
Docker image build
  |
  v
Deploy to server
  |
  v
Run migrations
  |
  v
Health check
  |
  v
Release
```

### 25.3 Migration Rules

- Migrations are versioned.
- Migrations must be reversible where practical.
- Never manually edit production schema.
- Backup before risky migrations.

---

## 26. Performance Targets

### 26.1 API Targets

| Operation | Target |
|---|---:|
| Login | < 500 ms p95 |
| Load chat list | < 300 ms p95 cached / < 800 ms network |
| Send message API response | < 300 ms p95 |
| Realtime delivery online | < 500 ms typical |
| Load Discover feed | < 800 ms p95 |
| Search users | < 500 ms p95 |

### 26.2 Client Targets

| Area | Target |
|---|---:|
| Cold start | < 2.5 seconds on average device |
| Open chat | < 300 ms from cached state |
| Message send UI feedback | instant optimistic display |
| Scroll chat history | 60 FPS target |

---

## 27. Scalability Path

### 27.1 MVP Scale

Target:

- 1 server;
- 1 PostgreSQL instance;
- 1 Centrifugo instance;
- R2 for media;
- thousands of users;
- hundreds of concurrent users.

### 27.2 Next Step Scale

When needed:

- move PostgreSQL to managed/dedicated server;
- add Redis for rate limits and caching;
- run multiple API replicas;
- run Centrifugo with Redis/NATS engine if needed;
- add CDN rules for media;
- add background worker service.

### 27.3 Future Services

Only split out services when there is real pressure.

Potential future services:

- notifications worker;
- media processor;
- search service;
- analytics pipeline;
- moderation tools;
- recommendation service.

---

## 28. Testing Strategy

### 28.1 Backend Tests

Required:

- unit tests for services;
- integration tests for repositories;
- auth token tests;
- permission/membership tests;
- message send flow tests;
- Discover visibility tests;
- group invite tests;
- rate limit tests.

### 28.2 Client Tests

Required:

- ViewModel tests;
- repository tests;
- network error mapping tests;
- UI state tests;
- local database migration tests.

### 28.3 Manual QA Checklist

Before MVP beta:

- register;
- verify email;
- login/logout;
- refresh token flow;
- create direct chat;
- send/receive message realtime;
- send image;
- create group;
- join group by invite;
- create Discover post;
- comment;
- like;
- report;
- block user;
- push notification on Android;
- push notification on iOS;
- archive chat;
- delete chat/message/post;
- privacy settings.

---

## 29. MVP Build Roadmap

### Phase 0 — Foundation

- repository setup (backend, Android, iOS as separate repos);
- Docker Compose;
- PostgreSQL migrations;
- Go API skeleton;
- Android app skeleton;
- iOS app skeleton;
- CI pipeline (backend, Android, iOS);
- logging/health checks.

### Phase 1 — Auth & Profile

- register/login;
- JWT access + rotating refresh;
- email verification;
- profile read/update;
- avatar upload;
- privacy settings.

### Phase 2 — Chats

- conversation list;
- direct chat;
- message send/history;
- Centrifugo realtime;
- read status;
- typing;
- archive/delete;
- pinned chats;
- image messages.

### Phase 3 — Groups

- create group;
- group chat;
- invite links;
- join group;
- member list;
- group admin actions.

### Phase 4 — Discover

- global Discover posts;
- group Discover posts;
- comments;
- likes;
- share to chat;
- message author;
- reports.

### Phase 5 — Notifications & Moderation

- FCM;
- APNs;
- notification settings;
- admin panel;
- report queue;
- block users;
- rate limits.

### Phase 6 — Beta Hardening

- backups;
- monitoring;
- performance testing;
- privacy review;
- security review;
- closed beta release.

---

## 30. Acceptance Criteria

Swetgram MVP technical architecture is acceptable when:

- Android and iOS clients are fully native, independently implemented, and maintain feature and design parity per the SDS.
- Users can register with email + username + password.
- Access tokens are short-lived and refresh tokens rotate.
- Users can send and receive realtime direct messages.
- Users can create and join groups.
- Groups include chat + Discover + members.
- Global Discover supports chronological text posts.
- Group Discover supports group-only posts.
- Users can comment and like Discover posts.
- Users can search by username/display name.
- Users can archive and delete chats.
- Push notifications work on Android and iOS.
- Reports can be viewed in a minimal admin panel.
- Admins can delete posts and block users.
- PostgreSQL backups run daily and restore has been tested.
- Production deployment plan to Hetzner is documented.
- Logs, health checks, and basic metrics are available.

---

## 31. Key Risks

### 31.1 Duplicated Work Across Two Native Codebases

Risk:

- every feature is built twice, doubling client engineering effort versus a shared-code approach;
- Android and iOS can drift out of sync in behavior, timing, or edge-case handling;
- design system (SDS) can be implemented inconsistently between platforms since there is no shared token/component layer enforcing it.

Mitigation:

- treat the SDS as the single source of truth for both platforms and review both implementations against it;
- maintain a shared, versioned API contract (OpenAPI/schema) so both clients build against the same backend behavior;
- keep a feature-parity checklist per release; QA explicitly signs off on both platforms before ship;
- staff or rotate engineers with context on both platforms where possible to reduce drift;
- prefer simple, well-established native patterns over clever platform-specific shortcuts that are hard to mirror.

### 31.1b Native iOS/Android Skill Requirement

Risk:

- requires genuinely separate Android and iOS engineering skill sets (previously one KMP-capable team could cover both); hiring or contracting for two specialties instead of one.

Mitigation:

- budget for two platform owners (or one engineer per platform at minimum) rather than a single mobile generalist;
- document shared product/business logic decisions in this STA so platform teams implement matching behavior independently.

### 31.2 Home Server Reliability

Risk:

- home server is not reliable enough for real production users.

Mitigation:

- use home server only before production;
- migrate to Hetzner before public launch;
- never rely on residential uptime for production messaging.

### 31.3 Abuse and Spam

Risk:

- public Discover and username messaging may attract spam.

Mitigation:

- email verification restrictions;
- rate limits;
- reports;
- admin panel;
- block user;
- progressive trust limits.

### 31.4 Hard Delete vs Moderation

Risk:

- hard delete can remove evidence needed for moderation.

Mitigation:

- retain minimal report metadata;
- store content hash where useful;
- review policy before scaling;
- consider legal/safety retention policy later.

### 31.5 Realtime Complexity

Risk:

- realtime bugs are hard to debug.

Mitigation:

- keep PostgreSQL as source of truth;
- use event IDs;
- use client reconciliation from REST history;
- do not depend only on WebSocket delivery.

---

## 32. Open Technical Decisions

These decisions are not blocking MVP architecture, but should be resolved before implementation or beta:

1. Confirm Navigation Compose (Android) and NavigationStack (iOS) as final navigation approaches.
2. Exact iOS push integration approach.
3. Whether to support offline outgoing message queue in v1.
4. Whether media URLs are public, signed, or proxied.
5. Whether admin panel is server-rendered, separate web app, or internal-only tool.
6. Whether to use Redis from day one or add after MVP.
7. Exact password policy.
8. Exact data retention policy for reports and deleted accounts.
9. Exact email provider.
10. Exact analytics implementation: internal table only or PostHog-compatible pipeline.

---

## 33. Engineering Principles

Swetgram engineering should follow these rules:

1. Build simple first.
2. Keep PostgreSQL as source of truth.
3. Use realtime for delivery, not truth.
4. Do not introduce microservices before real need.
5. Do not build custom crypto.
6. Do not claim E2E before E2E exists.
7. Keep modules clean and replaceable.
8. Prefer boring infrastructure for v1.
9. Keep Android and iOS in feature and design parity even without shared code.
10. Backups are a product feature, not an ops detail.

---

## 34. Reference Stack Notes

This STA aligns with the existing Swetgram v1 architecture direction: Go + Fiber, PostgreSQL, sqlc, Centrifugo, R2, JWT, Argon2id, Docker Compose, and EU deployment. The product model has been updated from marketplace/listings to Discover text posts and group Discover. The mobile client direction has been updated from a shared Kotlin Multiplatform + Compose Multiplatform app to two fully separate native applications.

Useful external references for implementation:

- Jetpack Compose documentation: https://developer.android.com/jetpack/compose
- Android Retrofit documentation: https://square.github.io/retrofit/
- Android Room documentation: https://developer.android.com/training/data-storage/room
- SwiftUI documentation: https://developer.apple.com/documentation/swiftui/
- Swift Concurrency documentation: https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/
- SwiftData documentation: https://developer.apple.com/documentation/swiftdata
- Centrifugo documentation: https://centrifugal.dev/docs/getting-started/introduction
- Centrifugo WebSocket transport: https://centrifugal.dev/docs/transports/websocket
- Fiber documentation: https://gofiber.io/
- sqlc documentation: https://sqlc.dev/

---

# End of Document
