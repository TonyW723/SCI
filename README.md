# SCI — Safety Coaching Industry Monitor

A scheduled Claude Code Routine that scans the public web weekly for
articles and activity from businesses offering safety coaching services
(behavioral safety coaching, safety culture coaching, workplace safety
coaching, etc.), and reports new findings as a digest.

## Why not scrape LinkedIn directly?

LinkedIn requires a login to browse most content and its Terms of
Service prohibit automated scraping; there is no public API for
searching third-party company posts. Instead, this monitor uses public
web search to surface LinkedIn posts/articles and company announcements
that are publicly indexed, which is reliable and ToS-clean.

## How it works

- A weekly Routine (Claude Code scheduled trigger) fires into this
  session.
- Each run searches the web for open-keyword mentions of safety
  coaching businesses and their recent posts/articles (see keyword list
  below).
- Findings are deduplicated against `monitor/seen.json` so you only see
  what's new since the last run.
- New findings are reported back as a digest message, and `monitor/seen.json`
  is updated and pushed.

## Keywords searched

- "safety coaching"
- "safety coach" (business/service context)
- "behavioral safety coaching" / "behaviour-based safety coaching"
- "safety culture coaching"
- "workplace safety coaching"
- "safety leadership coaching"

## Files

- `monitor/seen.json` — running list of previously reported items
  (URL + short identifier + date first seen), used for dedup.

## Adjusting the monitor

To change frequency, keywords, or scope (e.g. track specific named
competitors instead of/alongside open discovery), just ask in the
session that owns the Routine, or edit the keyword list above and let
the next run pick it up.
