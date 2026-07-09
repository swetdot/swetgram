# Swetgram Design System (SDS)

**Version:** 1.0  
**Status:** Draft / Foundation  
**Product:** Swetgram  
**Document Type:** Company Design System Specification  
**Platforms:** Android, iOS  
**Client Strategy:** Two separate native apps — Android (Kotlin + Jetpack Compose) and iOS (Swift + SwiftUI), no shared code  
**Last Updated:** 2026-07-07

---

## 0. Document Purpose

This document defines the official design language, visual system, interaction rules, component standards, accessibility requirements, and implementation guidelines for Swetgram.

The goal of the SDS is to ensure that every Swetgram screen feels consistent, premium, fast, minimal, and recognizable across Android and iOS.

This document should be used by:

- Product designers
- UI/UX designers
- Mobile engineers
- Frontend engineers
- Brand designers
- Illustration/icon designers
- QA testers
- Future product teams

The SDS is not a moodboard. It is the source of truth for how Swetgram should look, feel, and behave.

---

## 1. Product Design Direction

### 1.1 Brand Positioning

Swetgram is a modern European messenger focused on fast private communication, communities, and lightweight public discovery.

Swetgram should feel:

- Fast
- Premium
- Minimal
- Modern
- Friendly
- Private
- Calm
- Trustworthy

Swetgram should not feel:

- Loud
- Childish
- Overdecorated
- Corporate-heavy
- Like a Telegram clone
- Like a social media feed-first app
- Like a marketplace app

---

## 2. Design Principles

### 2.1 Speed First

Every UI decision must support the perception of speed.

Rules:

- Prioritize instant interaction feedback.
- Avoid heavy decorative UI.
- Keep transitions under 250ms in most cases.
- Use simple layouts that are easy to scan.
- Avoid loading screens unless absolutely necessary.

### 2.2 Content First

Messages, conversations, posts, and people are the focus.

Rules:

- UI chrome should be minimal.
- Avoid large decorative headers in core flows.
- Do not use unnecessary dividers when spacing can solve separation.
- Do not overuse shadows.

### 2.3 Premium Minimalism

Swetgram should feel refined, not empty.

Rules:

- Use generous spacing.
- Use consistent radii.
- Use subtle borders and surfaces.
- Keep color usage restrained.
- Use one main accent color.

### 2.4 Familiar but Ownable

Swetgram should use familiar messenger patterns while building a distinct identity.

Rules:

- Chat UX can be familiar to Telegram users.
- Visual identity must be Swetgram-specific.
- Icons must be custom, not default system icons in final production.
- Avoid using paper plane imagery as a primary brand symbol.

### 2.5 Privacy by Design

Privacy should be visible through clear settings and simple language, not exaggerated marketing.

Rules:

- Do not require phone numbers.
- Do not expose emails publicly.
- Show privacy controls clearly.
- Avoid misleading claims such as “nobody can read your messages” unless E2E is implemented.

---

## 3. Brand Foundations

### 3.1 Brand Core

Swetgram uses a Graphite / Black brand core with a single blue accent.

Core identity:

- Absolute Black: `#000000`
- Graphite: `#262223`
- Accent Blue: `#3B82F6`

### 3.2 Personality

Primary personality:

- Fast & powerful

Secondary traits:

- Premium & elegant
- Private & secure
- Friendly & community-focused
- Calm & minimal

### 3.3 UI Mood

The interface should feel like a premium messenger designed for everyday use.

Inspiration direction:

- Telegram-style messaging familiarity
- Apple-like restraint
- Linear-like precision
- Modern European digital product feel

Do not copy any single product directly.

---

## 4. Color System

### 4.1 Color Strategy

Swetgram is dark-first, but light theme must be production-level.

Default theme behavior:

- Default: System
- Design priority: Dark
- Light theme: fully supported

### 4.2 Core Brand Colors

| Token | Hex | Usage |
|---|---:|---|
| `brand.black.absolute` | `#000000` | Deepest background, OLED-safe areas, splash background |
| `brand.graphite` | `#262223` | Brand graphite, selected surfaces, high-emphasis dark UI |
| `brand.blue` | `#3B82F6` | Primary accent, CTAs, active tabs, links, unread badges, read receipts |

### 4.3 Dark Theme Tokens

| Token | Hex | Usage |
|---|---:|---|
| `dark.background.deep` | `#000000` | Deepest background only |
| `dark.background` | `#0B0B0B` | Main app background |
| `dark.surface` | `#171717` | Main surface, sheets, nav bars |
| `dark.card` | `#222222` | Cards, secondary containers, other-user chat bubbles |
| `dark.graphite` | `#262223` | Brand graphite surfaces |
| `dark.border` | `#2E2E2E` | Subtle borders/dividers |
| `dark.border.strong` | `#3A3A3A` | Focused borders, selected states |
| `dark.text.primary` | `#FFFFFF` | Primary text |
| `dark.text.secondary` | `#A1A1AA` | Secondary text, metadata |
| `dark.text.tertiary` | `#71717A` | Placeholder, disabled text |
| `dark.text.inverse` | `#0B0B0B` | Text on light/high-contrast surfaces |

### 4.4 Light Theme Tokens

| Token | Hex | Usage |
|---|---:|---|
| `light.background` | `#FAFAFA` | Main app background |
| `light.surface` | `#FFFFFF` | Main surface, sheets, nav bars |
| `light.card` | `#F3F4F6` | Cards, other-user chat bubbles |
| `light.border` | `#E5E7EB` | Subtle borders/dividers |
| `light.border.strong` | `#D1D5DB` | Focused borders, selected states |
| `light.text.primary` | `#111827` | Primary text |
| `light.text.secondary` | `#6B7280` | Secondary text, metadata |
| `light.text.tertiary` | `#9CA3AF` | Placeholder, disabled text |
| `light.text.inverse` | `#FFFFFF` | Text on dark/accent surfaces |

### 4.5 Accent Tokens

| Token | Hex | Usage |
|---|---:|---|
| `accent.blue` | `#3B82F6` | Primary actions, links, selected state |
| `accent.blue.pressed` | `#2563EB` | Pressed state |
| `accent.blue.soft.dark` | `#1D4ED8` | Dark elevated surfaces if needed |
| `accent.blue.soft.light` | `#DBEAFE` | Light selected backgrounds |

### 4.6 Semantic Colors

| Token | Dark | Light | Usage |
|---|---:|---:|---|
| `success` | `#22C55E` | `#16A34A` | Online indicator, success states |
| `warning` | `#F59E0B` | `#D97706` | Warnings |
| `danger` | `#EF4444` | `#DC2626` | Delete, report, destructive actions |
| `info` | `#3B82F6` | `#2563EB` | Informational states |

### 4.7 Online Indicator

Online indicator uses semantic success.

Rules:

- Use a small green dot.
- Dot size: 8dp for avatar overlays.
- Dot border: 2dp surface color to separate it from avatar.
- Last seen text is shown in chat info, not always in chat list.

### 4.8 Color Usage Rules

Do:

- Use blue for primary actions and active states.
- Use grayscale for most UI.
- Use red only for destructive actions.
- Use green only for online/success states.

Do not:

- Use multiple accent colors.
- Use blue for decorative backgrounds everywhere.
- Use absolute black as the main surface color everywhere.
- Use emoji as visual navigation icons.

---

## 5. Typography

### 5.1 Typeface

Primary font:

- Inter

Fallback:

- System sans-serif

Implementation fallback examples:

- Android: Inter if bundled; otherwise system default
- iOS: Inter if bundled; otherwise San Francisco fallback

### 5.2 Typography Principles

Swetgram typography should be:

- Clean
- Highly readable
- Compact but not cramped
- Modern
- Friendly

### 5.3 Type Scale

| Token | Size | Line Height | Weight | Usage |
|---|---:|---:|---:|---|
| `display.large` | 36sp | 44sp | 700 | Marketing screens, major empty states |
| `title.large` | 28sp | 36sp | 700 | Main screen title if needed |
| `title.medium` | 24sp | 32sp | 700 | Auth title, section hero |
| `headline` | 20sp | 28sp | 600 | Screen headers, card titles |
| `body.large` | 17sp | 25sp | 400 | Chat messages, main text |
| `body.medium` | 16sp | 24sp | 400 | Default body text |
| `body.small` | 14sp | 20sp | 400 | Secondary copy, descriptions |
| `label.large` | 15sp | 20sp | 600 | Buttons, tabs |
| `label.medium` | 13sp | 18sp | 500 | Metadata, chips |
| `caption` | 12sp | 16sp | 400 | Timestamps, counters, helper text |

### 5.4 Chat Typography

| Element | Token | Notes |
|---|---|---|
| Message text | `body.large` | 17sp recommended for readability |
| Message timestamp | `caption` | Right-bottom inside bubble |
| Chat row title | `body.medium` / 600 | Name of user/group |
| Chat row preview | `body.small` | Last message |
| Chat row time | `caption` | Time / date metadata |

### 5.5 Typography Rules

Do:

- Use sentence case in UI labels.
- Keep labels short.
- Use strong weight only where hierarchy is needed.
- Use larger message text than metadata.

Do not:

- Use all caps for normal UI.
- Use more than 2 font weights in one component.
- Use decorative typefaces in the app UI.

---

## 6. Layout and Spacing

### 6.1 Grid

Swetgram uses an 8dp spacing system.

Base spacing tokens:

| Token | Value | Usage |
|---|---:|---|
| `space.0` | 0dp | No spacing |
| `space.0_5` | 4dp | Tiny spacing |
| `space.1` | 8dp | Small spacing |
| `space.2` | 16dp | Default spacing |
| `space.3` | 24dp | Section spacing |
| `space.4` | 32dp | Large spacing |
| `space.6` | 48dp | Screen-level spacing |
| `space.8` | 64dp | Hero spacing |

### 6.2 Screen Margins

| Device | Horizontal Margin |
|---|---:|
| Small phones | 16dp |
| Standard phones | 16dp |
| Large phones | 20dp |
| Tablets / desktop future | 24–32dp |

### 6.3 Touch Targets

Minimum touch target:

- 44dp minimum
- 48dp recommended

Important interactive elements:

- Buttons: minimum height 48dp
- Icon buttons: 44–48dp
- Chat rows: 64–72dp
- Bottom nav items: 56–64dp

---

## 7. Shape System

### 7.1 Radius Tokens

| Token | Value | Usage |
|---|---:|---|
| `radius.none` | 0dp | Full-bleed edges |
| `radius.xs` | 6dp | Tiny elements |
| `radius.sm` | 10dp | Chips, small controls |
| `radius.md` | 16dp | Default cards, buttons, inputs |
| `radius.lg` | 20dp | Large cards, search bars |
| `radius.xl` | 24dp | Bottom sheets, dialogs |
| `radius.full` | 999dp | Pills, avatars |

### 7.2 Shape Rules

Default Swetgram radius:

- 16dp

Use circles for:

- Avatars
- Online indicators
- Floating action buttons

Use 24dp for:

- Bottom sheets
- Dialogs
- Large modal containers

---

## 8. Elevation, Borders, and Surfaces

### 8.1 Elevation Strategy

Swetgram should use almost no heavy shadows.

Preferred hierarchy:

1. Background
2. Surface
3. Card
4. Border
5. Accent state

### 8.2 Borders

Use subtle borders to separate layers.

Dark:

- Default border: `#2E2E2E`
- Strong border: `#3A3A3A`

Light:

- Default border: `#E5E7EB`
- Strong border: `#D1D5DB`

### 8.3 Shadow Usage

Use shadows only for:

- Floating action buttons
- Bottom sheets if needed
- Temporary overlays

Avoid shadows on normal cards and chat rows.

---

## 9. Iconography

### 9.1 Icon Direction

Swetgram will use custom-designed icons created by the brand/illustration specialist.

Final production UI should not rely on default Material icons as the main identity.

### 9.2 Icon Style

Icons should feel similar in clarity and simplicity to Telegram’s icon language:

- Simple
- Recognizable
- Rounded
- Minimal
- Lightweight
- Functional

But the icon set must be Swetgram-owned and visually distinct.

### 9.3 Icon Rules

Do:

- Use consistent stroke width.
- Use rounded terminals.
- Keep icons readable at 20–24dp.
- Use filled selected states only when needed.
- Use outline icons for default states.

Do not:

- Use emoji as app navigation icons.
- Mix icon families.
- Use overly detailed illustrations for small UI icons.
- Copy Telegram’s paper plane or visual assets.

### 9.4 Icon Sizes

| Token | Size | Usage |
|---|---:|---|
| `icon.xs` | 16dp | Metadata, small inline icons |
| `icon.sm` | 20dp | Secondary actions |
| `icon.md` | 24dp | Primary navigation/action icons |
| `icon.lg` | 32dp | Empty states, large actions |
| `icon.xl` | 48dp | Illustrative UI moments |

---

## 10. Illustration System

### 10.1 Illustration Direction

Illustrations should follow a Telegram-like spirit:

- Friendly
- Clean
- Lightweight
- Simple
- Characterful but not childish

They should support empty states, onboarding, security explanations, and feature education.

### 10.2 Illustration Rules

Do:

- Keep illustrations minimal and clear.
- Use Swetgram brand colors subtly.
- Use graphite/black/blue as core visual anchors.
- Make illustrations feel calm and premium.

Do not:

- Use noisy 3D visuals.
- Use childish cartoon characters as the default tone.
- Overload empty states with large art.
- Use illustrations where text alone is enough.

### 10.3 Illustration Usage

Use illustrations for:

- Welcome screen
- Empty chat list
- Empty Discover feed
- Empty Groups screen
- Email verification
- Error states
- Security/privacy education

Avoid illustrations inside:

- Active chat screens
- Dense lists
- Settings pages unless needed

---

## 11. Component System

## 11.1 Buttons

### Button Types

Swetgram uses four primary button types:

1. Primary button
2. Secondary button
3. Text button
4. Destructive button

### Primary Button

Usage:

- Login
- Create account
- Send important confirmation
- Save changes
- Create group
- Start chat

Visual:

- Background: `accent.blue`
- Text: white
- Radius: 16dp
- Height: 48–52dp
- Horizontal padding: 20dp

States:

| State | Behavior |
|---|---|
| Default | Blue background |
| Pressed | `accent.blue.pressed` |
| Disabled | Low opacity, no shadow |
| Loading | Spinner centered, label hidden or dimmed |

### Secondary Button

Usage:

- Cancel
- Edit
- Retry
- Secondary choices

Visual:

- Background: transparent or surface
- Border: subtle border
- Text: primary text
- Radius: 16dp

### Text Button

Usage:

- Forgot password
- Terms
- Privacy Policy
- Learn more

Visual:

- No background
- Text color: accent blue

### Destructive Button

Usage:

- Delete chat
- Leave group
- Report user
- Remove member

Visual:

- Text or background: danger
- Use confirmation dialogs for destructive actions

---

## 11.2 Text Fields

### Usage

- Login email/username
- Password
- Username
- Display name
- Search
- Bio

### Visual

- Height: 52dp minimum
- Radius: 16dp
- Border: subtle
- Background: surface/card
- Padding: 16dp horizontal

### States

| State | Visual Rule |
|---|---|
| Default | Subtle border |
| Focused | Accent blue border |
| Error | Danger border + helper text |
| Disabled | Reduced opacity |
| Filled | Same as default, text primary |

### Placeholder Tone

Placeholders should be direct:

- `Email or username`
- `Password`
- `Search`
- `Message @alex...`

---

## 11.3 Search Bar

### Usage

Search should always be visible on the Chats screen.

Search can cover:

- Chats
- Users
- Groups
- Discover posts in future versions

### Visual

- Height: 44–48dp
- Radius: 16–20dp
- Background: surface/card
- Left icon: custom search icon
- Placeholder: `Search`

### Behavior

- Tapping search opens active search state.
- Keyboard appears immediately.
- Results update as user types.
- Empty result state should be simple and friendly.

---

## 11.4 Cards

### Usage

- Discover posts
- Group cards
- Profile sections
- Settings sections
- Empty state containers

### Visual

- Radius: 16dp
- Background: card/surface
- Border: optional subtle border
- Shadow: none by default

### Rules

- Cards should not feel heavy.
- Use spacing instead of excessive dividers.
- Use consistent padding: 16dp default.

---

## 11.5 Avatars

### Shape

- Circle

### Sizes

| Token | Size | Usage |
|---|---:|---|
| `avatar.xs` | 24dp | Mentions, compact metadata |
| `avatar.sm` | 32dp | Comments, small rows |
| `avatar.md` | 44dp | Chat list |
| `avatar.lg` | 64dp | User cards |
| `avatar.xl` | 96dp | Profile screen |

### Avatar Fallback

If no avatar exists:

- Use initials.
- Use deterministic background color from neutral palette.
- Do not use random bright colors that break the brand.

### Online State

- Green dot at bottom-right.
- Dot size: 8dp on `avatar.md`, 10dp on larger avatars.
- Dot has 2dp border matching surrounding surface.

---

## 11.6 Bottom Navigation

### Tabs

Swetgram has four bottom navigation tabs:

1. Chats
2. Discover
3. Groups
4. Profile

### Rules

- No emoji in production UI.
- Use custom icons.
- Active tab uses `accent.blue`.
- Inactive tabs use secondary/tertiary text color.
- Labels should be visible by default.

### Height

- 64dp recommended
- Respect safe areas on iOS

---

## 11.7 Top App Bar

### Chats App Bar

Structure:

- Title: `Chats`
- Right action: Search if search is collapsed in future; for v1 search is visible below title
- No hamburger menu

### Screen Titles

Use direct titles:

- `Chats`
- `Discover`
- `Groups`
- `Profile`
- `Settings`

### Rules

- Avoid large decorative headers.
- Keep top bars calm and functional.
- Do not place the logo inside every app bar.

---

## 11.8 Floating Action Buttons

Swetgram uses two floating actions on the Chats screen.

### Primary FAB

Purpose:

- Start new chat

Visual:

- Circular
- Larger than secondary FAB
- Accent blue background
- White icon

### Secondary FAB

Purpose:

- Create Discover post

Visual:

- Circular
- Smaller than primary FAB
- Surface/card or graphite background
- Accent/primary icon depending on theme

### Placement

- Bottom-right
- Secondary FAB stacked above primary FAB
- Respect bottom navigation safe area

### Behavior

Primary FAB:

- Opens New Chat search flow

Secondary FAB:

- Opens Create Post composer

---

## 12. Messaging Components

## 12.1 Chat List Row

### Anatomy

Each chat row contains:

- Avatar
- Name
- Last message preview
- Time
- Unread badge if unread
- Mute icon if muted

### Row Height

- 68–76dp recommended

### Visual Rules

- No heavy separators.
- Use spacing for separation.
- Use avatar as visual anchor.
- Keep unread state clear but not noisy.

### Pinned Chats

- Maximum 5 pinned chats.
- Pinned chats appear above normal chats.
- Show `Pinned` section label only if at least one pinned chat exists.

### Archived Chats

- Archive row appears above pinned/normal chats only if archive is not empty.
- If archive is empty, hide it.

---

## 12.2 Swipe Actions

Chat list swipe actions:

### Swipe Right

Action:

- Archive chat

### Swipe Left

Action:

- Delete chat

Rules:

- Delete must require confirmation.
- Archive should be reversible via undo snackbar where possible.
- Swipe action backgrounds must be clear and readable.

---

## 12.3 Chat Screen

### Header

Header contains:

- Back button
- Avatar
- Display name
- Online / last seen summary
- Optional actions menu

### Message Area

- Messages sorted oldest to newest from top to bottom.
- Latest messages at bottom.
- Date separators appear between message days.

### Input Area

Placeholder format:

- `Message @alex...`

For groups:

- `Message Group Name...`

### Attachments

v1 may support:

- Images
- Files

If attachments are not in MVP, hide attachment action until ready.

---

## 12.4 Chat Bubbles

### Direction

Private chats:

- Own messages: right aligned
- Other messages: left aligned

Group chats:

- Own messages: right aligned
- Other messages: left aligned with avatar/name grouping as needed

### Style

Chosen style:

- Telegram-like familiar bubble layout

Own message:

- Background: `accent.blue`
- Text: white
- Timestamp/checks: white with reduced opacity or blue-read state as appropriate

Other message dark:

- Background: `dark.card`
- Text: `dark.text.primary`

Other message light:

- Background: `light.card`
- Text: `light.text.primary`

### Radius

Bubble radius:

- 16dp default
- Slight tail or asymmetric radius allowed if brand wants Telegram-like feel

### Timestamp

- Timestamp appears inside bubble, bottom-right.
- Format: `18:45`
- Read receipts appear next to timestamp for own messages.

### Read Receipts

Use Telegram-style status:

- One check: sent/delivered depending backend status model
- Two checks: read
- Read state: subtle blue or accent-aware

### Message Length

Maximum message length:

- 4096 characters

---

## 12.5 Message Long-Press Menu

Long press opens a universal action menu.

Actions:

- Reply
- Copy
- Forward
- Pin
- Delete
- Report

Rules:

- Same menu model for text messages wherever possible.
- Destructive actions visually separated.
- Report should be available for messages not sent by the current user.

---

## 13. Discover Components

## 13.1 Discover Purpose

Discover is a lightweight public text discussion feed.

v1 content type:

- Text posts only

No images, no videos, no algorithm in v1.

### Feed Order

- Chronological
- Newest first

### Post Anatomy

Each post contains:

- Author avatar
- Display name
- Username
- Timestamp
- Text content
- Like count
- Comment count
- Share action
- More/report action

### Post Card Style

- Minimal card or row-style surface
- No heavy shadow
- Comfortable spacing
- Text is primary focus

### Actions

Supported actions:

- Like
- Comment
- Share
- Message author
- Report

### Comments

- Comments open on post details screen.
- Comments are not fully expanded inside the feed by default.
- Feed remains lightweight.

---

## 14. Group Components

## 14.1 Group Model

A group is not only a chat. A group is a lightweight community.

Each group contains:

- Chat
- Discover
- Members

### Group Screen Navigation

Inside a group:

1. Chat
2. Discover
3. Members

### Group Card Anatomy

- Group avatar/icon
- Group name
- Member count
- Last activity or description
- Join/open action if needed

### Group Identity

Group avatars may use:

- Custom uploaded image
- Generated initials
- Custom icon/illustration

---

## 15. Profile Components

## 15.1 Profile Screen

Profile contains:

- Avatar
- Display name
- Username
- Bio
- Joined date
- User posts
- Groups if public/allowed
- Settings entry for own profile

### Public Email

User email must not be shown publicly.

### Phone Number

Swetgram does not require phone numbers in v1.

---

## 15.2 User Card

Before opening full profile, tapping a user can show a compact user card.

User card contains:

- Avatar
- Display name
- Username
- Online status
- Message button
- View Profile button

### Rules

- User card should open quickly.
- It should feel lightweight.
- It should not replace the full profile.

---

## 16. Authentication Screens

## 16.1 Auth Principles

Auth should be fast, minimal, and privacy-forward.

Registration uses:

- Display name
- Username
- Email
- Password
- Confirm password

Login uses:

- Email or username
- Password

### No Phone Number

Phone number is not required.

### Email Verification

Email verification should be simple and calm.

Tone example:

- `Check your email`
- `We sent a verification link to your inbox.`

---

## 17. Empty States

### 17.1 Voice and Tone

Empty states should be simple and friendly.

Approved tone:

- Short
- Helpful
- Calm
- Not overly funny
- Not corporate

### 17.2 Examples

Chats empty state:

```text
No chats yet.
Start a conversation.
```

Discover empty state:

```text
No posts yet.
Share the first thought.
```

Groups empty state:

```text
No groups yet.
Create one or join with a link.
```

Search empty state:

```text
No results found.
Try another name or username.
```

Error state:

```text
Something went wrong.
Please try again.
```

---

## 18. Voice and Tone

### 18.1 Tone Direction

Swetgram uses tone B:

- Simple
- Friendly
- Direct
- Calm

### 18.2 Writing Rules

Do:

- Use short sentences.
- Explain what happened.
- Tell the user what they can do next.
- Use human language.

Do not:

- Use technical errors directly.
- Use too much humor.
- Use long paragraphs in UI.
- Use aggressive or alarming language unless safety requires it.

### 18.3 Examples

Good:

```text
No chats yet.
Start a conversation.
```

Bad:

```text
Conversation dataset is currently empty.
```

Good:

```text
Username is already taken.
```

Bad:

```text
Validation error: unique constraint failed.
```

---

## 19. Motion System

### 19.1 Motion Principles

Motion should make Swetgram feel fast and polished.

Rules:

- Minimal
- Smooth
- Useful
- Never distracting

### 19.2 Duration Tokens

| Token | Duration | Usage |
|---|---:|---|
| `motion.instant` | 80ms | Tiny feedback |
| `motion.fast` | 150ms | Buttons, state changes |
| `motion.normal` | 220ms | Screen transitions, sheets |
| `motion.slow` | 300ms | Larger modals, rare emphasis |

### 19.3 Easing

Recommended easing:

- Standard ease-out for enter
- Standard ease-in for exit
- Avoid bounce unless intentionally used for playful micro-feedback

### 19.4 Motion Use Cases

Use motion for:

- Button press feedback
- Message send animation
- Message appear animation
- Bottom sheet opening
- Like action
- Pull-to-refresh
- Tab transitions

Avoid motion for:

- Every list item on normal scrolling
- Decorative loops
- Long animated onboarding sequences

---

## 20. Haptics

### 20.1 Haptic Principles

Haptics should feel premium and rare.

Use haptics for:

- Sending a message
- Long-pressing a message
- Pull-to-refresh completion
- Successful login
- Important toggles

Do not use haptics for:

- Every tap
- Scroll events
- Passive state changes

---

## 21. Sound

### 21.1 Notification Sound

Swetgram may have a unique notification sound.

Direction:

- Soft
- Short
- Recognizable
- Not harsh
- Not similar to Telegram/WhatsApp default sounds

### 21.2 In-App Sound

Use sparingly.

Possible sounds:

- Message sent
- Message received

User must be able to disable sounds.

---

## 22. Accessibility

### 22.1 Accessibility Goals

Swetgram should be usable by people with different vision, motor, and cognitive needs.

### 22.2 Contrast

Rules:

- Primary text must meet WCAG AA contrast where possible.
- Do not rely on color alone for destructive or status states.
- Online status should have label support for screen readers.

### 22.3 Dynamic Type

Support system font scaling where possible.

Rules:

- Chat bubbles must adapt to larger text sizes.
- Buttons should not truncate critical labels.
- Screen layouts should remain usable with larger text.

### 22.4 Screen Readers

All interactive elements need accessible labels.

Examples:

- `Start new chat`
- `Create Discover post`
- `Archive chat`
- `Delete chat`
- `User is online`

### 22.5 Touch Targets

Minimum touch target:

- 44dp minimum
- 48dp recommended

### 22.6 Reduced Motion

Respect reduced motion settings.

If reduced motion is enabled:

- Disable non-essential transitions.
- Replace movement with fades where appropriate.

---

## 23. Platform Guidelines

### 23.1 Cross-Platform Philosophy

Swetgram should feel like one product across Android and iOS, while respecting platform expectations. Android and iOS are built as two fully separate native codebases with no shared UI or logic layer, so this document (the SDS) is the mechanism that keeps them visually and behaviorally consistent — not shared code.

Rules:

- Shared design language across both platforms, enforced through this document rather than a shared component library.
- Respect safe areas on iOS.
- Respect Android back behavior.
- Use native-feeling keyboard and input behavior.
- Do not force iOS patterns onto Android or Android patterns onto iOS when they conflict with usability.
- Any visual or interaction change to a component must be applied to both platforms' native implementations; design review should check both, not assume parity.

### 23.2 Per-Platform Design System Implementation

Each platform implements its own native design-system layer, following the same structure conceptually.

Android (Kotlin/Compose) recommended structure:

```text
android/app/src/main/java/com/swetgram/design/
  tokens/
  theme/
  components/
  icons/
  motion/

feature-auth/
feature-chats/
feature-discover/
feature-groups/
feature-profile/
core-ui/
```

iOS (Swift/SwiftUI) recommended structure:

```text
ios/SwetgramApp/Sources/Design/
  Tokens/
  Theme/
  Components/
  Icons/
  Motion/

Feature/Auth/
Feature/Chats/
Feature/Discover/
Feature/Groups/
Feature/Profile/
CoreUI/
```

### 23.3 Token Naming in Code

Use semantic tokens, not raw colors, in UI code — on both platforms.

Good (Android/Kotlin):

```kotlin
SwetgramTheme.colors.background
SwetgramTheme.colors.surface
SwetgramTheme.colors.accent
SwetgramTheme.colors.textPrimary
```

Bad (Android/Kotlin):

```kotlin
Color(0xFF3B82F6)
Color.Black
```

Good (iOS/Swift):

```swift
SwetgramTheme.colors.background
SwetgramTheme.colors.surface
SwetgramTheme.colors.accent
SwetgramTheme.colors.textPrimary
```

Bad (iOS/Swift):

```swift
Color(hex: 0x3B82F6)
Color.black
```

Raw values should live only inside each platform's design token layer.

---

## 24. Design Tokens for Implementation

Since Android and iOS are separate native codebases, each platform defines and owns its own copy of these tokens. Values must match exactly between platforms — only the syntax differs.

### 24.1 Color Tokens

Android (Kotlin):

```kotlin
object SwetgramColorTokens {
    val BrandBlackAbsolute = Color(0xFF000000)
    val BrandGraphite = Color(0xFF262223)
    val AccentBlue = Color(0xFF3B82F6)

    val DarkBackgroundDeep = Color(0xFF000000)
    val DarkBackground = Color(0xFF0B0B0B)
    val DarkSurface = Color(0xFF171717)
    val DarkCard = Color(0xFF222222)
    val DarkBorder = Color(0xFF2E2E2E)

    val LightBackground = Color(0xFFFAFAFA)
    val LightSurface = Color(0xFFFFFFFF)
    val LightCard = Color(0xFFF3F4F6)
    val LightBorder = Color(0xFFE5E7EB)
}
```

iOS (Swift):

```swift
enum SwetgramColorTokens {
    static let brandBlackAbsolute = Color(hex: 0x000000)
    static let brandGraphite = Color(hex: 0x262223)
    static let accentBlue = Color(hex: 0x3B82F6)

    static let darkBackgroundDeep = Color(hex: 0x000000)
    static let darkBackground = Color(hex: 0x0B0B0B)
    static let darkSurface = Color(hex: 0x171717)
    static let darkCard = Color(hex: 0x222222)
    static let darkBorder = Color(hex: 0x2E2E2E)

    static let lightBackground = Color(hex: 0xFAFAFA)
    static let lightSurface = Color(hex: 0xFFFFFF)
    static let lightCard = Color(hex: 0xF3F4F6)
    static let lightBorder = Color(hex: 0xE5E7EB)
}
```

### 24.2 Spacing Tokens

Android (Kotlin):

```kotlin
object SwetgramSpacing {
    val Zero = 0.dp
    val Xs = 4.dp
    val Sm = 8.dp
    val Md = 16.dp
    val Lg = 24.dp
    val Xl = 32.dp
    val Xxl = 48.dp
}
```

iOS (Swift):

```swift
enum SwetgramSpacing {
    static let zero: CGFloat = 0
    static let xs: CGFloat = 4
    static let sm: CGFloat = 8
    static let md: CGFloat = 16
    static let lg: CGFloat = 24
    static let xl: CGFloat = 32
    static let xxl: CGFloat = 48
}
```

### 24.3 Radius Tokens

Android (Kotlin):

```kotlin
object SwetgramRadius {
    val Xs = 6.dp
    val Sm = 10.dp
    val Md = 16.dp
    val Lg = 20.dp
    val Xl = 24.dp
    val Full = 999.dp
}
```

iOS (Swift):

```swift
enum SwetgramRadius {
    static let xs: CGFloat = 6
    static let sm: CGFloat = 10
    static let md: CGFloat = 16
    static let lg: CGFloat = 20
    static let xl: CGFloat = 24
    static let full: CGFloat = 999
}
```

### 24.4 Keeping Tokens in Sync

Since these values are duplicated by hand across two codebases, treat this SDS as the single source of truth: any token change must be updated here first, then mirrored into both the Android and iOS token files in the same release. Consider a lightweight process (e.g., a shared token JSON/YAML file used to generate or cross-check both native files) if drift becomes a recurring problem.

---

## 25. Component States

Every interactive component must define these states where applicable:

- Default
- Pressed
- Focused
- Hovered future/desktop
- Disabled
- Loading
- Error
- Success
- Selected
- Unselected

### 25.1 State Rules

- Pressed state should be visible but subtle.
- Disabled state should reduce opacity and remove elevation.
- Error state should explain what happened.
- Loading state should prevent duplicate actions.

---

## 26. Do / Don’t Guidelines

### 26.1 Color

Do:

- Use blue for active actions.
- Use graphite/black for brand mood.
- Keep most surfaces neutral.

Don’t:

- Use many bright colors.
- Make every card blue.
- Use absolute black as every surface.

### 26.2 Typography

Do:

- Use Inter.
- Use clear hierarchy.
- Keep UI text short.

Don’t:

- Use decorative fonts.
- Use all caps for normal labels.
- Overuse bold text.

### 26.3 Components

Do:

- Reuse components.
- Follow state rules.
- Use consistent radius.

Don’t:

- Create one-off UI patterns for every screen.
- Use inconsistent button heights.
- Mix icon styles.

### 26.4 Product Feel

Do:

- Make Swetgram feel fast.
- Make common actions obvious.
- Keep messaging familiar.

Don’t:

- Make the app feel like a marketplace.
- Make Discover too heavy.
- Overcomplicate onboarding.

---

## 27. Screen-Level Design Rules

### 27.1 Splash Screen

- Use `#000000` deepest background.
- Center Swetgram logo.
- Keep it minimal.
- No unnecessary text unless brand requires it.

### 27.2 Welcome Screen

- Premium minimal layout.
- Short headline.
- Primary action: Create account.
- Secondary action: Log in.

### 27.3 Login Screen

- Fields: Email or username, Password.
- Text button: Forgot password.
- Primary button: Log in.

### 27.4 Register Screen

Fields:

- Display name
- Username
- Email
- Password
- Confirm password

### 27.5 Chats Screen

- Search always visible.
- Archive row visible only if archive is not empty.
- Pinned section visible only if pinned chats exist.
- Two FABs: new chat and create Discover post.

### 27.6 Chat Screen

- Message input placeholder: `Message @username...`
- Own messages right aligned and blue.
- Other messages left aligned and neutral.
- Timestamp inside bubble.

### 27.7 Discover Screen

- Chronological text feed.
- Lightweight cards/rows.
- Comments open on post details.
- No images/videos in v1.

### 27.8 Groups Screen

- Show user's groups.
- Support create group.
- Support join via invite link.
- Group detail has Chat / Discover / Members.

### 27.9 Profile Screen

- Avatar, display name, username, bio.
- Own profile includes settings entry.
- Public profile hides email.

---

## 28. Naming Conventions

### 28.1 Product Areas

Use these exact names in product and design docs:

- Chats
- Discover
- Groups
- Profile
- Settings

### 28.2 Avoided Names

Do not use these names in user-facing UI for v1:

- Marketplace
- Board
- Feed, unless used internally or later changed intentionally
- Channels
- Stories
- Reels

### 28.3 Component Naming

Component names should be descriptive:

- `SwetButton`
- `SwetTextField`
- `SwetSearchBar`
- `SwetAvatar`
- `SwetChatBubble`
- `SwetChatRow`
- `SwetPostCard`
- `SwetBottomNav`
- `SwetUserCard`

---

## 29. QA Design Checklist

Before a screen is approved, check:

- Uses correct theme tokens.
- Works in dark mode.
- Works in light mode.
- Supports system theme.
- Uses correct typography scale.
- Uses 8dp spacing system.
- Uses correct radius.
- Has all required states.
- Has empty state.
- Has loading state.
- Has error state.
- Meets touch target requirements.
- Has accessible labels.
- Does not use emoji for navigation icons.
- Does not expose private email.
- Does not make unsupported privacy claims.

---

## 30. MVP Component Priority

### Must Build First

1. Theme tokens
2. Typography tokens
3. Spacing/radius tokens
4. Primary/secondary/text/destructive buttons
5. Text fields
6. Search bar
7. Avatar
8. Bottom navigation
9. Top app bar
10. Chat row
11. Chat bubble
12. Message input
13. Discover post card
14. Group card
15. User card
16. Empty states
17. Dialogs and bottom sheets

### Can Build Later

- Advanced animations
- Rich illustrations
- Media gallery components
- Voice message components
- Call UI
- Desktop/tablet adaptive layouts

---

## 31. Versioning

SDS versions should follow semantic document versioning:

- `1.0` Foundation for MVP
- `1.1` Minor component additions
- `1.2` Accessibility/motion expansion
- `2.0` Major visual redesign or new product platform

All major changes must include:

- What changed
- Why it changed
- Affected screens/components
- Migration notes for design and engineering

---

## 32. Open Design Decisions

The following decisions remain open:

1. Final Swetgram logo direction
2. Final custom icon set style guide
3. Final illustration style examples
4. Exact notification sound identity
5. Final chat bubble tail/asymmetry style
6. Whether Discover post cards should be full cards or row-based surfaces
7. Whether group Discover should visually differ from global Discover
8. Whether profile QR code appears in v1 or v1.1

---

## 33. Summary

Swetgram’s design system is built around a dark-first, premium, minimal visual identity with a Graphite / Black brand core and a single blue accent.

The product should feel fast, calm, private, and modern. Messaging patterns should remain familiar, especially in chat, while Swetgram builds its own identity through custom icons, restrained colors, polished typography, consistent spacing, and lightweight community features.

The SDS must be treated as the visual and interaction foundation of the product. New screens and features should extend this system, not bypass it.

