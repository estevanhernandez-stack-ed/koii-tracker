# Koii Tracker

🎲 **Live demo:** <https://YOUR-USERNAME.github.io/koii-tracker/>

A single-file web app for tracking BIG Games Pet Sim 99 events — clan standings, daily strats, dice timing, prize tier projection. Built initially for the **Void RNG / Angel Clan Battle (Update 78)** but designed so adapting to the next event takes ~5 minutes of editing.

## What's in this folder

```
Koii Tracker/
├── index.html      ← The whole app. Open this. Share this. Edit this.
└── README.md       ← You're reading it.
```

That's it. One self-contained HTML file. No build step, no npm install, no server.

## Use it

**Locally**: double-click `index.html`. Opens in your default browser. Works offline (with the embedded snapshot fallback).

**Share with the clan**: drop `index.html` in any Discord channel, attach to email, or host it. Every recipient gets their own local progress (saved per browser via `localStorage`).

**Host it**: any static host works.
- GitHub Pages: push the file, set repo Pages source to root.
- Netlify Drop: drag the file in.
- Vercel / Cloudflare Pages: same deal.
- Self-host: any HTTP server. Apache, nginx, `python -m http.server`, whatever.

## What it shows

- **Live banner**: countdown to event end, detects when the API flips the active clan battle to the expected one (Angel Battle), shows status.
- **Top 20 clans**: live from `ps99.biggamesapi.io`, toggleable between Battle Points and Lifetime Diamonds. Falls back to a 100-clan snapshot if the API is unreachable.
- **Clan lookup**: quick-switch pills for your clans (currently `K0i2`), plus free-text search with autocomplete from the embedded index. Live lookup pulls medals, guild level, and per-member contributions during an active battle.
- **Priority strats**: 7 ordered tactics for the Angel Battle. What to do with combo streaks, when to burn Mega Lucky Dice II, why the Ultra Chest matters.
- **Daily checklist**: per-day action items, persists across sessions.
- **Decision matrix**: when-do-I-do-X cards for each weather state and combo tier.
- **Prize projection**: type a clan rank, see the tier prize.
- **Personal grind log**: rolls, best combo streak.
- **Pitfalls section**: the dice/combo mistakes that bleed value.

## API source

Public BIG Games API: <https://ps99.biggamesapi.io/>
- Docs: <https://github.com/BIG-Games-LLC/ps99-public-api-docs>
- CORS: open (`access-control-allow-origin: *`) — direct browser fetch works
- Cache TTL: 60s server-side. Tracker self-rate-limits to one auto-refresh per 30s; manual refresh is unthrottled.

Endpoints used:
- `GET /api/activeClanBattle` → battle name + start/end times
- `GET /api/clans?page=1&pageSize=100&sort=Points&sortOrder=desc` → top 100 leaderboard
- `GET /api/clan/{name}` → single clan with members, medals, contribution

## Adapting to the next event

Open `index.html`, find the block marked `EVENT_CONFIG` near the top of the `<script>` tag (search `=== EVENT_CONFIG`). Edit these fields:

### 1. Event metadata
```js
event: {
  id: "next-event-2026-XX",        // unique slug, used for localStorage key
  title: "Next Event Name",
  updateNumber: 79,
  sourceUrl: "https://www.biggames.io/post/pet-simulator-99-update-79",
  startDate: "2026-MM-DDT00:00:00Z",
  endDate:   "2026-MM-DDT23:59:59Z",
  expectedBattleNameMatch: "keyword",  // substring matched against API battle name
}
```
Changing `event.id` resets all checklist progress and personal stats so the new event starts clean.

### 2. Prize tiers
```js
prizeTiers: [
  { rankLabel: "#1", rankMin: 1, rankMax: 1, prize: "Top Prize", pillClass: "gold" },
  // ...
]
```
`pillClass` can be `gold`, `silver`, `bronze`, or empty.

### 3. Strats, decision matrix, quick reference, checklist, pitfalls
Every section is a config array. Just rewrite the strings. The renderer reads from config; you don't touch the DOM code.

### 4. Branding (optional)
```js
brand: {
  title: "Your Clan Tracker",
  initial: "X",
  accent: "#hexcolor",
  // ...
}
```

### 5. Your clans
```js
myClans: [
  { name: "ClanName", label: "ClanName", note: "primary" },
  { name: "BackupClan", label: "BackupClan", note: "backup" },
]
```
First entry is the default selection. Add or remove entries to show or hide quick-switch pills.

## Refreshing the embedded clan snapshot

The 100-clan fallback at the bottom of `index.html` (search `CLAN_INDEX`) gets stale over time. To regenerate:

```bash
curl -s -H "User-Agent: Mozilla/5.0" \
  "https://ps99.biggamesapi.io/api/clans?page=1&pageSize=100&sort=DepositedDiamonds&sortOrder=desc" \
  | python3 -c '
import sys, json
d = json.load(sys.stdin)["data"]
out = [{"n":c["Name"],"cc":c.get("CountryCode") or "","m":c.get("Members") or 0,"mc":c.get("MemberCapacity") or 0,"d":c.get("DepositedDiamonds") or 0,"p":c.get("Points") or 0} for c in d]
print(json.dumps(out, separators=(",",":")))
'
```

Paste the output as the literal value of `CLAN_INDEX` (between the `[` and `].map(...)`). Save.

## Privacy / data

- All user state (checklist, stats, last-looked-up clan) is stored in `localStorage` on the device. Nothing leaves the browser.
- API calls go directly to `ps99.biggamesapi.io`. No third-party analytics or tracking.

## Known limits

- The `/api/activeClanBattle` endpoint can lag behind by hours when a new battle drops. The tracker handles this gracefully — banner stays in "event window open" state until the API catches up.
- Battle Points are 0 across all clans between battles. The tracker auto-suggests switching to the Lifetime Diamonds view in that state.
- Roblox group avatars (`rbxassetid://...`) aren't directly fetchable; the tracker doesn't render clan icons. Add later via the `/image/{id}` proxy if wanted.

## Credits

- Strategy content: distilled from the [BIG Games Update 78 post](https://www.biggames.io/post/pet-simulator-99-update-78) and the BIG Games public API.
- API: [ps99.biggamesapi.io](https://ps99.biggamesapi.io) (BIG-Games-LLC).

## License

Internal tool. Use freely within Koii.
