---
title: "Swetgram Product Specification (SPS)"
subtitle: "Version 2.0 - MVP Product Requirements"
author: "Project Swetgram"
date: "2026-07-07"
lang: ru
geometry: margin=22mm
mainfont: "DejaVu Sans"
fontsize: 10.5pt
colorlinks: true
linkcolor: black
urlcolor: black
toc: true
toc-depth: 3
---

# 1. Document Control

| Field | Value |
|---|---|
| Document | Swetgram Product Specification (SPS) |
| Version | 2.0 |
| Status | Draft - product baseline for MVP |
| Product | Swetgram |
| Product Type | Cross-platform messenger for Android and iOS |
| Owner | Founder / Product Lead |
| Last Updated | 2026-07-07 |
| Source Inputs | Swetgram v1 architecture document and product decisions from planning sessions |

## 1.1 Purpose

This document defines the product scope, user experience, feature requirements, non-goals, success metrics, and product roadmap for the Swetgram MVP.

The SPS is the product source of truth for design, frontend, backend, QA, and future planning. It answers the question: "What are we building, for whom, and how should it behave?"

## 1.2 Document Audience

This specification is intended for:

- Founder / Product Lead
- UI/UX Designer
- Mobile Developer
- Backend Developer
- QA Tester
- Future investors, advisors, or technical partners

## 1.3 Product Decision Summary

| Area | Decision |
|---|---|
| Product Direction | Messenger + Discover + Groups + Profile |
| Initial Audience | First promoted to users in Cyprus, with expansion to Cyprus, Europe, and global markets |
| Internal Positioning | General-purpose messenger, not a student-only product |
| Auth Model | Email + username + password; no phone number required |
| Main Screen | Chats |
| Bottom Navigation | Chats / Discover / Groups / Profile |
| Public Feed | Discover - chronological text posts, comments, likes |
| Marketplace | Excluded from v1 |
| Phone Contacts | Excluded from v1 |
| Design Style | Modern, premium, minimal, fast, friendly, private |
| Icons | Custom Swetgram icon system; Material Symbols Rounded may be used only as prototype fallback |

# 2. Executive Summary

Swetgram is a modern European messenger focused on fast private conversations, groups, and public text-based discovery.

The MVP is built around four primary areas:

1. Chats - private and group messaging.
2. Discover - chronological text discussions.
3. Groups - communities with chat, group Discover, and members.
4. Profile - identity, settings, privacy, and personal presence.

Swetgram is not positioned as a marketplace, a student-only app, or a Telegram clone. Students in Cyprus are the first adoption channel, but the product itself must be general enough to scale across Cyprus, then Europe, then globally.

# 3. Product Vision

## 3.1 Mission

Create a fast, private, and beautiful messenger where people can communicate, discover conversations, and build communities without needing phone numbers.

## 3.2 Vision Statement

Swetgram is a modern messenger for private chats, communities, and lightweight public discussions.

It should feel:

- fast like a messenger;
- clean like a premium productivity app;
- friendly like a community app;
- private without being complicated;
- scalable beyond one country or one audience segment.

## 3.3 Product Principles

Every feature in Swetgram should follow these principles.

### Fast

The app should feel instant. Opening chats, sending messages, loading conversations, and switching tabs should be fast.

### Minimal

Every screen should contain only what is necessary. If a control does not support a clear user action, it should be removed.

### Premium

The visual experience should feel polished, calm, and modern. The app should not look like a clone of an existing messenger.

### Private

Users should not need to share a phone number. Discovery is based on username, display name, QR code, and profile links.

### Community-first

Groups should be stronger than ordinary chat rooms. A group should be a small community with chat, group Discover, and members.

# 4. Product Positioning

## 4.1 What Swetgram Is

Swetgram is:

- a messenger;
- a group communication platform;
- a place to discover public text discussions;
- a username-based communication product;
- a mobile-first Android and iOS application.

## 4.2 What Swetgram Is Not

Swetgram is not:

- a student-only app;
- a marketplace in v1;
- a social media video feed;
- a Telegram clone;
- a WhatsApp clone;
- a Discord clone;
- a phone-number-first messenger.

## 4.3 Initial Growth Strategy

The product is general-purpose. However, the first marketing audience is users in Cyprus, especially communities where fast adoption can happen through existing social circles, group links, and peer recommendations.

Expansion path:

1. Early closed beta in one or more local communities.
2. Broader Cyprus launch.
3. Expansion to additional European countries.
4. Global availability.

# 5. MVP Scope

## 5.1 Included in MVP

The MVP includes:

- Email + username registration.
- Login with email or username.
- Email verification.
- User profiles.
- Private 1:1 chats.
- Group chats.
- Real-time messages.
- Typing indicators.
- Online status and last seen.
- Telegram-style read receipts.
- Chat list with search, pinned chats, archive, and delete.
- Discover text feed.
- Discover post details and comments.
- Likes on Discover posts.
- Message author from Discover post.
- Groups with Chat / Discover / Members.
- Invite links for groups.
- Basic notifications.
- Privacy settings.
- Block and report flows.
- Light and dark mode.

## 5.2 Excluded from MVP

The MVP excludes:

- Phone-number authentication.
- Phone contact sync.
- Marketplace/listings.
- Stories.
- Reels/video feed.
- Voice calls.
- Video calls.
- Voice messages.
- Bots.
- Channels.
- Mini apps.
- Payments.
- Premium subscriptions.
- End-to-end encrypted secret chats.
- Desktop app.
- Web app.

## 5.3 MVP Product Hypothesis

If Swetgram provides a fast, premium messenger with username-based identity, groups, and a simple Discover feed, then early communities can adopt it as a lightweight alternative to existing messengers and social feeds.

# 6. Target Users and Personas

## 6.1 Primary Early User

The early user is a mobile-first person who wants to communicate quickly with friends, groups, and communities without relying on phone numbers.

Needs:

- Start chats quickly.
- Find people by username.
- Join groups easily.
- Read short public discussions.
- Control privacy.
- Avoid complicated onboarding.

## 6.2 Community Organizer

A user who creates and manages groups.

Needs:

- Create group easily.
- Invite people with a link.
- Moderate members.
- Keep group discussion organized.
- Use group Discover for more persistent posts.

## 6.3 Discover Contributor

A user who posts public text updates or questions.

Needs:

- Write a post quickly.
- Receive comments and likes.
- Message other users from posts.
- Avoid noisy or complex posting tools.

# 7. Information Architecture

Swetgram has four main tabs.

```text
Swetgram
├── Chats
├── Discover
├── Groups
└── Profile
```

## 7.1 Bottom Navigation

The bottom navigation must contain exactly four tabs in MVP:

| Tab | Purpose |
|---|---|
| Chats | Main home screen, conversations, private and group chat list |
| Discover | Public chronological text discussions |
| Groups | User's groups, group discovery, group creation |
| Profile | User profile, settings, privacy, QR code |

## 7.2 Default Start Screen

After login, the default screen is Chats.

Rationale: Swetgram is primarily a messenger. Chats must feel like home.

# 8. Authentication and Account Creation

## 8.1 Registration

Registration requires:

- Display name.
- Username.
- Email.
- Password.
- Confirm password.

Phone number is not required.

## 8.2 Username Rules

Recommended username rules:

- Must be unique.
- Lowercase normalized.
- 3-24 characters.
- Allowed characters: letters, numbers, underscore.
- Cannot impersonate reserved words such as admin, support, swetgram, security.
- Must be changeable, but with rate limits.

## 8.3 Login

Users can log in with:

- email + password;
- username + password.

## 8.4 Email Verification

Email verification is required before full access.

Possible limited access before verification:

- view app shell;
- edit profile;
- resend verification email.

Restricted before verification:

- sending messages;
- creating groups;
- posting in Discover.

## 8.5 Password Recovery

Password reset uses email recovery.

# 9. Identity and Discovery

## 9.1 User Identity

Each user has:

- avatar;
- display name;
- username;
- bio;
- joined date;
- online status if allowed by privacy settings.

Email is private and not visible to other users.

## 9.2 Discovery Methods

Users can find each other by:

1. @username - primary method.
2. Display Name - secondary convenience method.
3. QR code - in-person discovery.
4. Profile link - shareable link.

Email search is excluded from MVP for privacy.

## 9.3 Profile Link

Recommended format:

```text
swetgram.app/u/{username}
```

Behavior:

- If app is installed, opens profile in app.
- If app is not installed, opens landing page or app store page.

## 9.4 QR Code

Each profile should have a QR code that opens the user's profile.

# 10. Chats

## 10.1 Purpose

Chats are the core of Swetgram. The Chats tab is the default home screen and should be optimized for speed and clarity.

## 10.2 Chat Types

MVP supports:

- private 1:1 chats;
- group chats.

## 10.3 Chat List Requirements

Each chat row contains:

- avatar;
- chat title / user display name;
- last message preview;
- timestamp;
- unread badge if unread;
- mute indicator if muted.

No unnecessary metadata should be visible in the row.

## 10.4 Search on Chat List

Search is always visible at the top of the Chats screen.

Search should search:

- existing chats;
- users;
- groups.

Future versions may include message and Discover search.

## 10.5 Pinned Chats

Users can pin up to 5 chats.

Behavior:

- pinned chats appear above normal chats;
- pinned chat order can be manual or last-activity based in v1;
- if the user tries to pin a sixth chat, show a clear limit message.

## 10.6 Archive

Archive is supported.

Behavior:

- swipe right on a chat archives it;
- archived chats appear in an Archived entry above the chat list when archive is not empty;
- archived chats can be restored.

## 10.7 Delete

Delete is supported.

Behavior:

- swipe left deletes a chat;
- deletion requires confirmation;
- delete removes the chat from the user's view;
- server-side behavior must be defined technically: local delete for user vs conversation deletion for all participants.

Recommended v1 behavior: delete for current user only, not for all participants.

## 10.8 Floating Actions

Chats screen has two floating buttons:

- Large button: create new chat.
- Small button: create Discover post.

The two-button pattern gives fast access to the two most important creation actions.

## 10.9 New Chat Flow

```text
Chats
-> Large floating chat button
-> New Chat screen
-> Search @username or display name
-> Select user
-> Open conversation
```

## 10.10 Message Composer

The message input placeholder should be personalized:

```text
Message @alex...
```

For groups:

```text
Message Group Name...
```

## 10.11 Message States

Messages should support:

- sending;
- sent;
- delivered;
- read;
- failed.

Read receipts follow a familiar Telegram-style pattern:

- single check - sent;
- double check - delivered;
- blue/different accent double check - read.

## 10.12 Chat Screen Layout

Private chats:

- no avatar beside every message;
- avatar and online indicator in top bar;
- date separators for Today, Yesterday, and specific dates;
- message time inside message bubble.

Group chats:

- show sender name and avatar when useful;
- consecutive messages by same sender may be grouped;
- group top bar shows group name and status/member count.

## 10.13 Message Long-Press Menu

Every message should use the same long-press menu structure:

- Reply.
- Copy.
- Forward.
- Pin.
- Delete.
- Report.

Availability may change depending on ownership, permissions, and message type.

## 10.14 Message Length

Maximum message length: 4096 characters.

Rationale: Swetgram is a messenger, not a document editor.

# 11. Discover

## 11.1 Purpose

Discover is a public text-based discussion area. It is lightweight and chronological.

Discover is not a marketplace and not a video feed.

## 11.2 Feed Rules

MVP feed is chronological.

No algorithmic ranking in v1.

Rationale:

- simpler to build;
- transparent for users;
- avoids moderation complexity;
- fits early-stage community growth.

## 11.3 Post Content

MVP supports text-only posts.

Excluded from v1 Discover posts:

- images;
- videos;
- audio;
- polls;
- hashtags;
- paid promotions.

## 11.4 Post Card

Each post card contains:

- author avatar;
- display name;
- username;
- timestamp;
- post text;
- like count;
- comment count;
- share action;
- report action;
- message author action.

## 11.5 Post Details

Tapping a post opens the post detail screen.

Post detail contains:

- original post;
- comments;
- comment composer;
- author actions;
- report action.

## 11.6 Comments

Comments are displayed only on the post detail screen.

Rationale: the main feed should stay compact and fast.

## 11.7 Likes

Users can like posts and comments.

Recommended v1 rule:

- one like per user per post;
- like can be removed;
- likes are public as counts, not necessarily as visible lists in v1.

## 11.8 Message Author

Discover posts should allow users to start a private chat with the author.

Flow:

```text
Discover post
-> Tap Message
-> Open or create private chat with author
```

This connects Discover to the core messenger experience.

## 11.9 Create Post Flow

```text
Chats small floating post button OR Discover create button
-> Create Post screen
-> Write text
-> Publish
-> Post appears in chronological feed
```

# 12. Groups

## 12.1 Purpose

Groups are communities inside Swetgram.

A group is not only a chat. Each group has:

```text
Group
├── Chat
├── Discover
└── Members
```

This is one of Swetgram's strongest product differentiators.

## 12.2 Group Creation

Users can create a group from:

- Chats tab via creation flow;
- Groups tab via create group button.

Required group fields:

- group name;
- optional avatar;
- optional description.

## 12.3 Group Invite Links

Groups support invite links.

Invite links allow communities to migrate from other platforms.

## 12.4 Group Chat

Group chat behaves like a normal multi-person conversation.

MVP supports:

- messages;
- typing;
- read/delivery status at a simplified level;
- member list;
- basic admin controls.

## 12.5 Group Discover

Each group has its own Discover area.

Group Discover posts are visible only to group members.

Use cases:

- announcements;
- questions;
- persistent discussions;
- event planning;
- resources.

## 12.6 Members

Members tab contains:

- member list;
- admins;
- search members;
- member card;
- leave group action.

## 12.7 Group Roles

MVP roles:

- Owner.
- Admin.
- Member.

Recommended permissions:

| Action | Owner | Admin | Member |
|---|---:|---:|---:|
| Send messages | Yes | Yes | Yes |
| Create group Discover posts | Yes | Yes | Yes |
| Invite members | Yes | Yes | Yes or configurable |
| Remove member | Yes | Yes | No |
| Promote admin | Yes | No | No |
| Delete group | Yes | No | No |

# 13. Profile

## 13.1 My Profile

Profile contains:

- avatar;
- display name;
- @username;
- bio;
- joined date;
- QR code;
- settings entry.

## 13.2 Public Profile

Public profile contains:

- avatar;
- display name;
- @username;
- bio;
- online / last seen if privacy allows;
- Message button;
- shared groups if privacy allows;
- report / block actions.

## 13.3 User Card

Before opening full profile, tapping a user may show a compact user card.

User card contains:

- avatar;
- display name;
- username;
- online status;
- Message button;
- View Profile button.

Purpose: fast context without leaving the current screen.

# 14. Search

## 14.1 Global Search Scope

Search should support:

- people by username;
- people by display name;
- groups;
- existing chats.

Future scope:

- Discover posts;
- messages;
- files.

## 14.2 Search Result Design

Results should be grouped:

```text
People
Groups
Chats
```

If a query begins with @, people search should be prioritized.

## 14.3 Empty State

If no results:

```text
No results found.
Try another username or name.
```

# 15. Notifications

## 15.1 Notification Categories

Notifications should be separated into categories:

- Messages.
- Group activity.
- Discover replies.
- Mentions.
- Security.

## 15.2 Push Notifications

MVP push notification types:

- new private message;
- new group message;
- Discover comment reply;
- mention;
- security event.

## 15.3 Notification Controls

Users should be able to mute:

- a specific chat;
- a group;
- Discover notifications;
- all notifications temporarily.

# 16. Privacy and Safety

## 16.1 Privacy Settings

MVP privacy settings:

- Show online status: Everyone / Contacts or Chats / Nobody.
- Show last seen: Everyone / Contacts or Chats / Nobody.
- Who can message me: Everyone / People with shared groups / Nobody except existing chats.
- Blocked users list.

Exact labels can be adjusted during UI design.

## 16.2 Blocking

Blocking a user should:

- prevent them from sending new messages to the blocker;
- hide or limit their presence from the blocker;
- prevent direct interaction where appropriate.

## 16.3 Reporting

Users can report:

- users;
- messages;
- Discover posts;
- Discover comments;
- groups.

Report reasons:

- spam;
- scam;
- harassment;
- hate or abusive content;
- impersonation;
- illegal content;
- other.

## 16.4 Moderation

MVP requires admin/moderator ability to:

- review reports;
- delete posts/comments/messages where necessary;
- suspend users;
- rate-limit abusive accounts.

## 16.5 Security Positioning

Swetgram v1 should not claim end-to-end encryption if it is not implemented.

Allowed messaging:

- private messaging;
- data protection;
- account security;
- transparent privacy controls.

Disallowed messaging:

- "nobody can read your messages" unless true end-to-end encryption is implemented.

# 17. Settings

## 17.1 Settings Sections

Recommended settings structure:

```text
Account
Privacy
Notifications
Appearance
Chats
Storage
Language
About
Log Out
```

## 17.2 Appearance

Appearance supports:

- light mode;
- dark mode;
- system mode.

## 17.3 Language

Language is an open product decision.

Recommended approach:

- MVP UI can start in English or Russian depending on launch audience;
- architecture should support localization from day one;
- future languages should include English, Russian, Greek, and other European languages.

# 18. Design Requirements

## 18.1 Design Style

Swetgram design style:

- modern;
- premium;
- minimal;
- fast;
- friendly;
- private.

## 18.2 Visual Direction

The interface should feel calm and polished.

Avoid:

- heavy shadows;
- noisy gradients;
- too many colors;
- childish rounded shapes;
- social-media clutter.

## 18.3 Color Direction

Working brand colors:

| Token | Value | Purpose |
|---|---|---|
| Absolute Black | #000000 | Primary dark background / brand anchor |
| Graphite | #262223 | Main surface / cards |
| Swetgram Blue | #3B82F6 | Recommended action/accent color |
| White | #FFFFFF | Primary dark-mode text |

Final color accessibility should be validated during the design system phase.

## 18.4 Shape Language

Medium rounded corners.

Recommended radius:

- Buttons: 16dp.
- Cards: 16dp.
- Inputs: 16dp.
- Bottom sheets: 24dp.
- Avatars: circles.

## 18.5 Motion

Animations should be minimal and fast.

Recommended duration:

- 150-250ms for standard transitions;
- no excessive bounce effects;
- use subtle fades and slides.

## 18.6 Icons and Illustrations

Swetgram will use a custom icon and illustration system created by a specialist.

Prototype fallback: Material Symbols Rounded.

Logo direction remains open. Recommended exploration:

- abstract S;
- speech-inspired S;
- rounded geometric mark;
- no paper airplane to avoid Telegram association.

# 19. Empty States and Error States

## 19.1 Empty Chats

```text
No conversations yet.
Start chatting with someone.
[ New Chat ]
```

## 19.2 Empty Discover

```text
No discussions yet.
Start the first one.
```

## 19.3 Empty Groups

```text
You have not joined any groups yet.
Create a group or join with an invite link.
```

## 19.4 Network Error

```text
Connection problem.
Check your internet and try again.
[ Retry ]
```

## 19.5 Message Send Failure

Failed messages should show a retry action.

# 20. Analytics and Success Metrics

## 20.1 North Star Metric

Recommended North Star Metric:

Weekly Active Conversations.

Definition: number of conversations with at least one message sent in the last 7 days.

Rationale: Swetgram is primarily a messenger, so active conversations matter more than vanity signups.

## 20.2 Core Metrics

| Metric | Purpose |
|---|---|
| DAU | Daily usage |
| WAU | Weekly active user base |
| MAU | Monthly active user base |
| D1 retention | First-day value |
| D7 retention | Early habit formation |
| D30 retention | Long-term stickiness |
| Messages sent per user | Messaging engagement |
| New chats created | Network growth |
| Groups created | Community growth |
| Discover posts created | Public discussion activity |
| Discover comments | Conversation quality |
| Invites sent | Viral growth |
| Reports per 1,000 users | Safety signal |

## 20.3 Event Tracking

Recommended analytics events:

- account_registered;
- email_verified;
- login_success;
- chat_created;
- message_sent;
- group_created;
- group_joined;
- discover_post_created;
- discover_comment_created;
- user_searched;
- profile_opened;
- report_submitted;
- user_blocked.

# 21. MVP Acceptance Criteria

Swetgram MVP can be considered product-complete when:

1. A user can register with email, username, and password.
2. A user can verify email.
3. A user can log in with email or username.
4. A user can edit profile information.
5. A user can find another user by username or display name.
6. A user can open a 1:1 chat.
7. A user can send and receive messages in real time.
8. A user can see message delivery/read states.
9. A user can create a group.
10. A user can invite others to a group using a link.
11. A user can post text in Discover.
12. A user can comment on a Discover post.
13. A user can like a Discover post.
14. A user can message a Discover post author.
15. A user can block another user.
16. A user can report users, messages, posts, and groups.
17. A user can change appearance mode.
18. Push notifications work for core messaging.
19. The app supports both Android and iOS.
20. The app is stable enough for closed beta.

# 22. Roadmap

## 22.1 MVP - v1

Focus: core messenger + Discover + groups.

Included:

- auth;
- profile;
- chats;
- groups;
- Discover;
- basic privacy;
- basic moderation;
- notifications.

## 22.2 v1.1

Focus: polish and retention.

Possible additions:

- better search;
- improved group admin tools;
- message forwarding;
- improved moderation panel;
- better onboarding;
- analytics dashboard.

## 22.3 v2

Focus: richer communication.

Possible additions:

- voice messages;
- reactions;
- media in Discover;
- advanced privacy;
- secret chats with end-to-end encryption if technically ready;
- more localization.

## 22.4 v3

Focus: ecosystem.

Possible additions:

- events;
- marketplace;
- mini apps;
- AI-powered recommendations;
- web/desktop exploration;
- premium features.

# 23. Risks and Mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Users compare it to Telegram | High | Build unique identity around Discover and group ecosystems |
| Empty app problem | High | Launch through communities and invite links |
| Discover spam | Medium/High | Rate limits, reports, moderation tools |
| Cross-platform complexity | Medium | Keep MVP UI simple and use shared design system |
| No phone number reduces contact discovery | Medium | Strong username, QR, and profile link flows |
| Overbuilding v1 | High | Keep strict MVP scope and avoid calls, stories, marketplace, bots |
| Privacy claims too strong | High | Do not claim E2E until implemented |

# 24. Open Decisions

These decisions should be finalized before implementation or during design system creation.

| Decision | Current Status |
|---|---|
| Final logo direction | Open |
| Final accent color | Working recommendation: #3B82F6 |
| MVP default UI language | Open |
| Exact privacy labels | Open |
| Exact group role permissions | Needs technical validation |
| Whether Discover is global only or also group-specific in MVP | Recommended: both if timeline allows; otherwise global first |
| Whether delete chat is local-only or for everyone | Recommended: local-only in v1 |
| Admin/moderation dashboard scope | Open |

# 25. Final MVP Definition

The Swetgram MVP is complete when a user can:

- create an account without a phone number;
- find people by username;
- chat privately;
- create and join groups;
- use Discover for public text discussions;
- control basic privacy;
- report and block abusive users;
- receive notifications;
- use the app on Android and iOS.

Swetgram v1 should be simple, fast, polished, and focused.

The goal is not to match Telegram feature-for-feature. The goal is to create a clean, modern communication product with a strong foundation for growth.
