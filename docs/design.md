# TKN Odometer Design

## Problem

LLM subscription usage is spread across provider dashboards. Today, checking GitHub Copilot quota requires opening `https://github.com/settings/copilot/features` manually, which is repetitive and hard to monitor throughout the day.

TKN Odometer should provide a tiny macOS menu bar app that gives quick visibility into LLM quota usage. The first supported provider is GitHub Copilot.

## Goals

- Live in the macOS menu bar with a small, unobtrusive footprint.
- Show current usage status when the menu bar item is clicked.
- Start with GitHub Copilot quota usage.
- Keep the design extensible for additional LLM subscriptions later.
- Make quota checking faster than opening each provider dashboard manually.

## Non-goals

- Replacing provider billing or subscription management dashboards.
- Supporting every LLM provider in the first version.
- Editing subscription settings from the app.
- Running as a full desktop dashboard application.

## MVP scope

### Platform

- macOS menu bar application.
- Displays a menu/popover from the menu bar item.

### First provider: GitHub Copilot

- Show GitHub Copilot usage status available from the user's GitHub account.
- Link back to the official GitHub Copilot settings/features page for details.
- Handle unauthenticated or expired sessions clearly.

### Display

The first version should answer:

- What quota categories are available for GitHub Copilot?
- How much of each quota has been used?
- How much remains, if the provider exposes that information?
- When does the quota reset, if the provider exposes that information?
- When was the data last refreshed?

### Refresh behavior

- Refresh automatically on a lightweight interval.
- Allow manual refresh from the menu.
- Avoid excessive polling of provider pages or APIs.

## User flow

1. User installs and opens TKN Odometer.
2. A small icon appears in the macOS menu bar.
3. User connects or authorizes GitHub.
4. User clicks the menu bar icon.
5. The app displays GitHub Copilot quota usage and last refresh time.
6. User can refresh, open GitHub Copilot settings, or quit the app.

## Provider model

Each provider integration should expose a common usage model:

- Provider name.
- Account identity.
- Usage categories.
- Used amount.
- Limit amount, when available.
- Remaining amount, when available.
- Reset time, when available.
- Source URL.
- Last refresh time.
- Error state.

This keeps the app ready for future providers such as OpenAI, Anthropic, Cursor, Claude, Gemini, or other LLM subscriptions.

## Open questions

- Does GitHub expose Copilot quota usage through an official API, or must the first version rely on browser/session access to the settings page?
- Which GitHub Copilot quota categories should be tracked first?
- Should authentication use GitHub OAuth, a local browser session, a personal access token, or another mechanism?
- What refresh interval is acceptable for accurate data without creating unnecessary traffic?
- Should usage history be stored locally, or should the MVP only show current status?
- What should the menu bar icon display when quota is healthy, near limit, exhausted, or unavailable?

## Risks

- GitHub Copilot usage details may not be available through a stable public API.
- Scraping account settings pages can be fragile if GitHub changes the UI.
- Provider authentication must be handled carefully to avoid storing sensitive credentials.
- Quota terminology may differ across providers, so the shared provider model must stay flexible.

## Suggested next discussion topics

- Confirm the exact GitHub Copilot quota fields to show.
- Decide whether the MVP should prioritize an official API path or a browser-based fallback.
- Choose the initial macOS implementation approach.
- Define the visual states for normal, warning, limit reached, and error conditions.
- Decide whether local usage history is required in the first release.
