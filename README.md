# SCI — Safety Coaching Industry Monitor

A weekly Claude Code Routine that scans the public web for articles and
activity from businesses offering safety coaching services, and reports
new findings as a digest.

## Why not scrape LinkedIn directly?

LinkedIn requires a login to browse most content and its Terms of
Service prohibit automated scraping; there is no public API for
searching third-party company posts. Instead, this monitor uses public
web search to surface LinkedIn posts/articles and company announcements
that are publicly indexed, which is reliable and ToS-clean.

## How it is set up

- **Branch:** everything lives on `main` (the default branch). There are
  no other working branches.
- **Session:** one dedicated Claude Code session ("Safety Coaching
  Monitor"), created with this repo attached at `main`.
- **Schedule:** a Routine wakes that session every Monday at 01:00 UTC
  (9am Perth) and asks it to follow the run procedure below.
- **State:** `monitor/seen.json` — every item already reported (URL,
  business, summary, date first seen), used so each digest shows only
  new items.

## Weekly run procedure

The session follows these steps on every run. Edit this section to
change what the monitor does; the next run picks it up.

1. **Health check.** `git fetch origin main`, check out `main`, and
   `git pull`. If the fetch or pull fails, stop: send a push
   notification starting "Safety monitor FAILED:" with the exact error,
   and repeat it as the first line of the reply.
2. **Search window.** From the latest `date_first_seen` in
   `monitor/seen.json` to today. If that is more than 7 days, cover the
   whole gap (a run was missed).
3. **Search** the public web (never log in to or scrape LinkedIn) for
   recent LinkedIn posts/articles and company activity from businesses
   offering safety coaching services, using these keywords, combined
   with `linkedin.com` where useful:
   - "safety coaching"
   - "safety coach" (business/service context)
   - "behavioral safety coaching" / "behaviour-based safety coaching"
   - "safety culture coaching"
   - "workplace safety coaching"
   - "safety leadership coaching"

   This is open discovery, not a fixed company list.
4. **Dedup.** Drop anything whose URL is already in `monitor/seen.json`.
5. **Digest.** Reply with only the new findings: business name, what
   they posted (title/summary), the link, and one line on why it is
   relevant to safety coaching services. If nothing is new, say so in
   one line.
6. **Save.** Append new items to `monitor/seen.json` (`url`, `business`,
   `summary`, `date_first_seen` = today), commit as
   `Weekly monitor: N new safety coaching items (YYYY-MM-DD)`, and push
   to `main`. If nothing is new, skip the commit. If the push fails, send a "Safety monitor FAILED:" push
   notification with the error.
7. **Notify.** Send a push notification with a one-line summary
   (e.g. "Safety monitor: 3 new items this week").
8. Keep replies brief; this session runs indefinitely and its history
   grows each week.
