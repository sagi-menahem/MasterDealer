![MasterDealer — poker session tracker](assets/brand/banner.png)

# MasterDealer

MasterDealer is an independent poker-session tracker for groups that want a shared, reliable view of buy-ins, cash-outs, settlements, and long-term records. It modernizes a legacy Android and Firebase workflow into a web product that participants can use together while a game is in progress.

**Project type:** independent product · **Role:** sole designer and engineer · **Source:** private; this repository is a public case study. · [Visit MasterDealer](https://masterdealer.online)

## Preview

<p align="center">
  <img src="docs/screenshots/desktop.webp" alt="MasterDealer public website on desktop" width="68%" />
  <img src="docs/screenshots/mobile.webp" alt="MasterDealer public website on mobile" width="26%" />
</p>

The images above show the public website. Account screens and private group records are not included.

The product gives a poker group one shared session record: members join a group, record every buy-in and cash-out, then end the game with a settlement plan and a durable history.

## Problem and solution

Poker nights often depend on one person keeping notes, reconciling the pot after the game, and explaining who should pay whom. Errors tend to surface only when the cash no longer matches the session record. The older Android application held useful historical data, but was built around a single legacy workflow.

MasterDealer makes each game a group session with live updates across connected devices. It validates whether the recorded pot balances, keeps an auditable transaction history, and creates a settlement plan when play ends. The design also supports independent groups, roles, invitations, statistics, photos, and localized routes for Hebrew RTL, English, and Russian.

## Product highlights

- Create group-scoped sessions and record buy-ins and cash-outs as they happen.
- Calculate a compact settlement plan between debtors and creditors.
- Validate the zero-sum session before settlement so recording problems are visible early.
- Keep player and group history, leaderboards, and session-level records.
- Synchronize relevant changes in real time, with responsive desktop and mobile interfaces.

## Engineering decisions

Money is treated as a domain value, never a JavaScript float. The application uses `decimal.js` for currency calculations so a displayed balance and a settlement calculation follow the same precise arithmetic.

Settlement is deliberately expressed as a simple, explainable algorithm: match the largest debtor against the largest creditor until all balances are settled. This yields a compact transfer plan while keeping the result understandable to the people who need to act on it.

The multi-group model scopes membership, balances, history, and real-time subscriptions to a group. Roles determine who may manage sessions or invitations, rather than relying on the visibility of controls in the browser.

## Stack

Next.js, TypeScript, Tailwind CSS, Supabase PostgreSQL, Drizzle ORM, Supabase Realtime, Zod, `decimal.js`, and `next-intl`. The visual system uses the Atom One palette across dark and light themes.

---

Built by **[Sagi Menahem](https://www.sagimenahem.com/)** · [LinkedIn](https://www.linkedin.com/in/sagi-menahem/)
