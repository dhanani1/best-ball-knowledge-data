# Best Ball Knowledge by FPTS — Tester Guide

Welcome. This doc is everything you need to install the extension, use it during a draft, and help us fix things when something looks off. Reading time: ~5 minutes.

---

## 1. What this is

A Chrome extension that adds a recommendation sidebar to your Underdog Fantasy draft pages. While you draft, it tells you who to take, why, and surfaces game-week stacks, bring-backs, and championship-week setups.

It reads what's on your draft screen — it does NOT log in for you, does NOT touch your account, and does NOT send your picks anywhere.

---

## 2. Installing the extension (one-time, ~3 minutes)

You'll get a zip file from Zain (link in your email or Google Drive).

1. **Download the zip** to your computer.
2. **Right-click → Extract / Unzip** (Mac: double-click; Windows: right-click → Extract All). You should end up with a folder named something like `extension`. Remember where you put it — moving it later will break the extension.
3. Open Chrome.
4. In the address bar, type `chrome://extensions` and hit Enter.
5. In the top-right corner of that page, toggle **Developer mode** ON. Three new buttons appear at the top-left.
6. Click **Load unpacked**.
7. In the file picker, navigate to the folder you unzipped in step 2 — pick the inner `extension` folder (the one that has files like `manifest.json` and `sidebar.html` in it). Click **Select**.
8. The extension appears in your list with the name **Best Ball Knowledge by FPTS**. You're done.

You can pin it to your toolbar for visibility: click the puzzle-piece icon in the Chrome toolbar, find the extension, and click the pin icon next to it.

---

## 3. Using it during a draft

1. Go to your draft on Underdog: `https://app.underdogsports.com/draft/...` or `/active/...`
2. Within 1–2 seconds, a sidebar slides in from the right edge of the page.
3. Recommendations populate as soon as the draft loads. When it's your turn, your top picks appear at the top of the sidebar with a 0–100 score badge.

That's it. You don't need to click anything to get fresh data — see § 4.

---

## 4. Data updates happen automatically (NEW — June 2026)

Player ADP, ranks, contest info, and the NFL schedule **refresh themselves in the background**, no action required from you.

Look at the very bottom of the sidebar — there's a small line that reads something like:

```
Data: 2h ago
```

That tells you the recommender is using data that's at most 2 hours old. If it reads:

```
Data: bundled fallback · check network
```

it means your computer couldn't reach GitHub (where the data lives) and the extension is using whatever data shipped with the zip. Recommendations still work, but they may be a couple days stale. Reconnect to wifi or check your network and the next time you load a draft page, it'll refresh.

**Hover over that line** to see exactly which data files came from the live source vs. the bundled fallback.

---

## 5. When you DO need to update the extension

Zain will only send a new zip when there's **new code** — a new feature, a bug fix, a tuning improvement, etc. Data refreshes (daily ADP) do NOT require a new zip.

When a new zip arrives:

1. Download the new zip.
2. Unzip it. **Overwrite the old folder** (or unzip somewhere new and update the path).
3. Go to `chrome://extensions`.
4. Find **Best Ball Knowledge by FPTS** in the list.
5. Click the small **circular reload arrow** at the bottom of the card (you'll see it because Developer mode is on).
6. Refresh any open Underdog tabs.

That's it. You're on the new version.

---

## 6. Helping us debug when something looks off

If a pick the tool recommends seems weird, or the score doesn't make sense, the most useful thing you can do is send us **the exact draft state at that moment**.

There's a button for that. During a live (in-progress) draft, look at the round/pick info bar near the top of the sidebar. You'll see a small button labeled:

```
Export Live
```

Click it. A CSV file downloads with a name like:

```
The_Puppy_live_pick85_caa1b8d2.csv
```

The filename tells us the contest, the pick number when you exported, and the unique draft ID. Send that CSV to Zain. With that one file, he can perfectly reproduce what the tool was looking at.

Same button (labeled `Export Draft`) appears on completed drafts — works the same way.

### If you want to send more detail
Open the sidebar's DevTools console:

1. Right-click anywhere inside the sidebar panel (not the page behind it).
2. Click **Inspect**.
3. A small DevTools window opens. Click the **Console** tab.
4. Type `BBM_DIAG()` and hit Enter. A big JSON output appears.
5. Right-click the output → **Copy object**.
6. Paste it into your message to Zain.

That output contains everything — your roster, the recommendations, where the data came from, your contest. Anything Zain might need.

---

## 7. Common things that go wrong + fixes

| Symptom | Fix |
|--|--|
| Sidebar doesn't appear on a draft page | Hard-refresh the page: **Cmd+Shift+R** (Mac) or **Ctrl+Shift+R** (Windows). |
| Sidebar shows "Gathering data…" and never loads | Same — hard-refresh. If that doesn't fix it, restart Chrome. |
| Recommendations look stale or wrong | Check the footer (§ 4). If it says "bundled fallback · check network," reconnect to wifi and refresh. If it says "Data: <some short time> ago," it's working with current data — the rec is just what the tool thinks. Use Export Live + send to Zain. |
| Console error "Extension context invalidated" | This means Chrome updated the extension while your draft page was open. Hard-refresh the page (**Cmd+Shift+R**). The extension auto-detects this on most actions and offers to reload for you. |
| Sidebar is gone entirely after a Chrome update | Go to `chrome://extensions`, make sure Developer mode is still on, and re-enable the extension's toggle if it got disabled. If the extension is missing entirely, repeat § 2 (Chrome occasionally removes side-loaded extensions). |
| You moved or deleted the unzipped folder | Chrome can no longer find the extension. Re-unzip and repeat § 2. |

---

## 8. Things to NOT worry about

- **Privacy:** the extension never reads your login, never stores your password, never tells anyone what you're drafting. It only reads what's already on your screen.
- **Performance:** the sidebar uses < 50 MB of memory and < 1% CPU during a draft. It won't slow down Chrome.
- **Cheating:** Underdog has no rule against this kind of tool. It's a sophisticated cheat-sheet — same category as a spreadsheet open on a second monitor.

---

## 9. Quick reference

- **Did the data refresh?** Look at the sidebar footer. "Data: <Nh ago>" means yes.
- **Need to send Zain a bug report?** Click **Export Live** during the draft → email him the CSV.
- **Got a new zip?** Replace the folder, click the reload arrow at `chrome://extensions`, refresh open tabs.
- **Sidebar misbehaving?** Hard-refresh the Underdog page: Cmd+Shift+R / Ctrl+Shift+R.

That's everything. If you hit something not on this list, screenshot it and message Zain with the screenshot + roughly what you were doing.
