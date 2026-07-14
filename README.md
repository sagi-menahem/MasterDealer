<div align="center">
  <h1>MasterDealer</h1>
  <p><strong>Premium Poker Tracking Application</strong></p>
  <p>A poker session tracking platform featuring real-time updates, multi-group support, and automatic settlement calculations. Designed for serious poker groups who want to track their games professionally.</p>

  <br/>

  <a href="https://masterdealer.online">
    <img src="https://img.shields.io/badge/Live_Demo-masterdealer.online-98c379?style=for-the-badge&logo=googlechrome&logoColor=white" alt="Live Demo">
  </a>

  <br/><br/>

  ![Next.js](https://img.shields.io/badge/Next.js_16-000000?style=flat-square&logo=nextdotjs&logoColor=white)
  ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
  ![Tailwind CSS](https://img.shields.io/badge/Tailwind_v4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white)
  ![Supabase](https://img.shields.io/badge/Supabase-3FCF8E?style=flat-square&logo=supabase&logoColor=white)
  ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white)
  ![Drizzle](https://img.shields.io/badge/Drizzle_ORM-C5F74F?style=flat-square&logo=drizzle&logoColor=black)
</div>

---

## About

**MasterDealer** is a premium web application built to replace a legacy Android/Firebase poker tracking system. It provides professional-grade poker session management for serious players and clubs. Built end-to-end as the sole engineer — the codebase itself is private; this README is the portfolio-facing summary covering the architecture, engineering decisions, and screenshots that I can share publicly.

The platform enables poker groups to:
- Track game sessions with precise buy-in and cash-out recording
- Calculate settlements automatically using an optimized algorithm
- View comprehensive statistics and lifetime records
- Share memorable moments with photo galleries

---

## How This Was Built

Built solo, **AI-first**: I orchestrate AI coding agents (Claude Code, Codex) through a documented methodology rather than writing every line by hand — the engineering discipline is the point, not the speed.

- **`AGENTS.md` as the single source of truth** — a rules file in the repo defines the architecture, conventions, and hard constraints every agent must obey: money never in floats (`decimal.js` for every settlement calculation), the pot must always validate zero-sum before a game can close, every group-scoped query is checked against the caller's role and group membership server-side.
- **Guardrail scripts & audit pipelines** — automated checks run on every change (Zod schema validation on every mutation, decimal-money precision rules, Hebrew/English/Russian translation parity), so quality is enforced by tooling, not vigilance.
- **The engineer decides, the agent executes** — every schema, settlement algorithm, and architectural choice on this page was designed and reviewed by me. Agents accelerate implementation; they never own the design.

The result: one engineer delivering a production system at team-level velocity — with the discipline the decisions below reflect.

---

## Features

### Game Management
**Real-time Session Tracking** — Create and manage poker sessions with instant synchronization across all connected devices. Track buy-ins and cash-outs as they happen with automatic balance calculations.

**Smart Settlement** — When a game ends, the greedy settlement algorithm calculates the minimum number of payment transfers needed to settle all debts between players.

**Pot Validation** — Built-in validation ensures the pot always balances (zero-sum), catching any recording errors before they cause problems.

### Multi-Group Support
**Multi-tenant Architecture** — Support for multiple poker groups/clubs, each with their own members, statistics, and game history.

**Role-based Permissions** — Three permission levels (Owner, Admin, Member) control who can manage games, invite players, and access admin features.

**Invite System** — Shareable invite links allow group owners to bring in new members securely.

### Statistics & Records
**Comprehensive Analytics** — Track lifetime profit/loss, session count, average performance, and more at both personal and group levels.

**Detailed History** — View game-by-game breakdowns with full transaction ledgers showing every buy-in and cash-out with timestamps.

**Leaderboards** — See how players rank within each group across various metrics.

### User Experience
**Real-time Updates** — Powered by Supabase Realtime, all changes sync instantly across devices with optimistic UI updates.

**Dark/Light Themes** — Beautiful Atom One color scheme supporting both dark and light modes.

**Internationalization** — Full support for Hebrew (RTL), English, and Russian with locale-prefixed routing.

**Mobile Responsive** — Glassmorphism design that works seamlessly on desktop, tablet, and mobile devices.

---

## Tech Stack

| Layer              | Technologies                                         |
| :----------------- | :--------------------------------------------------- |
| **Framework**      | Next.js 16 (App Router), TypeScript                  |
| **Styling**        | Tailwind CSS v4, Atom One Theme, Glassmorphism       |
| **Database**       | Supabase (PostgreSQL), Drizzle ORM                   |
| **State**          | React Context (user, group, realtime subscriptions)  |
| **Validation**     | Zod schemas for type-safe input validation           |
| **Financial Math** | decimal.js for precision currency calculations       |
| **i18n**           | next-intl with locale-prefixed routing               |
| **Icons**          | Lucide React                                         |

---

## Architecture Highlights

### Financial Precision
All monetary calculations use `decimal.js` to ensure precision. Native JavaScript floating-point arithmetic is never used for money, preventing rounding errors that could cause pot imbalances.

### Settlement Algorithm
When a game ends, the system calculates optimal payment transfers using a greedy algorithm:
1. Players with positive balances are creditors (owed money)
2. Players with negative balances are debtors (owe money)
3. The algorithm matches largest debtor to largest creditor, minimizing total transfers

### Real-time Architecture
- **Supabase Realtime** provides WebSocket-based synchronization
- **Optimistic updates** ensure responsive UI while changes propagate
- **Debounced subscriptions** (500ms) prevent rapid re-fetches during bulk operations
- **Request deduplication** via React's `cache()` optimizes database queries

### Multi-tenant Design
Each group is fully isolated with:
- Separate member lists and balances
- Independent game history
- Role-based access control
- Group-scoped realtime subscriptions

---

## Design System

The application uses the **Atom One** color scheme, inspired by the popular VS Code theme.

### Dark Mode (One Dark Pro Darker)
| Token      | Color      | Usage                    |
|:-----------|:-----------|:-------------------------|
| Background | `#1f2228`  | Deep charcoal with blue  |
| Text       | `#abb2bf`  | Soft gray                |
| Primary    | `#61afef`  | Blue accents & buttons   |
| Accent     | `#c678dd`  | Purple highlights        |
| Felt       | `#98c379`  | Poker green, profit      |
| Gold       | `#e5c07b`  | Premium actions          |
| Danger     | `#e06c75`  | Red for loss/destructive |

### Light Mode (One Light)
| Token      | Color      | Usage                    |
|:-----------|:-----------|:-------------------------|
| Background | `#fafafa`  | Off-white with blue tint |
| Text       | `#383a42`  | Dark gray                |
| Primary    | `#4078f2`  | Blue accents & buttons   |
| Accent     | `#a626a4`  | Purple highlights        |
| Felt       | `#50a14f`  | Poker green, profit      |
| Gold       | `#c18401`  | Amber premium actions    |
| Danger     | `#e45649`  | Red for loss/destructive |

### Visual Effects
- **Glassmorphism** — Translucent cards with backdrop blur for a modern, layered appearance
- **Subtle animations** — Smooth transitions and micro-interactions enhance the premium feel
- **Responsive typography** — Heebo font family with excellent Hebrew support

---

## Background

MasterDealer was created to modernize a legacy Android application that had been tracking poker games for several years. The original system, built on Firebase, had accumulated valuable historical data that needed to be preserved while moving to a more maintainable and feature-rich platform.

Key improvements over the legacy system:
- **Web-based** — No app installation required, works on any device
- **Multi-group** — Support multiple poker clubs instead of one global group
- **Real-time** — Instant synchronization vs. manual refresh
- **Statistics** — Rich analytics and leaderboards
- **Modern UX** — Responsive design with dark mode support

---

## Get Started

Ready to track your poker games? Visit [masterdealer.online](https://masterdealer.online) to create your group and start tracking.

---

<div align="center">

**Built by Sagi Menahem**

[![GitHub](https://img.shields.io/badge/GitHub-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/sagi-menahem)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/sagi-menahem/)
[![Portfolio](https://img.shields.io/badge/Portfolio-0D2440?style=flat-square&logo=googlechrome&logoColor=white)](https://sagimenahem.tech)

</div>
