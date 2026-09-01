# SBC Court Queue

A live, no-login queue manager for a single pickup basketball court. Anyone can add
their name from their own phone, teams draft and rotate automatically, and admins
(Shannon & David) can manage the night with a shared PIN â€” no app install, no accounts.

Live at: `https://shelbybc.github.io/Hoops/` *(update this if the URL ever changes)*

---

## How a night works

**1. People add their name.**
Anyone opens the link, types their first and last name, and hits Join. No account,
no login. They land on the bench in the order they joined.

**2. The first 10 form Game 1.**
Once 10 active people have joined, they're automatically split into two random
teams of 5 â€” Team A and Team B. Nothing needs to be tapped to make this happen.

**3. Game 1's winner gets picked once â€” and only once.**
After Game 1, an admin taps "Team A won" or "Team B won." That's the *only* game
all night where a winner needs to be picked.

**4. From then on, everyone plays exactly 2 games per turn, then rotates off.**
- The winning team from Game 1 stays on court for a 2nd game.
- The losing team goes to the back of the bench.
- Whichever team reaches 2 games in a row comes off â€” win or lose â€” and the next
  5 people in line (first-come, first-served) take their spot.
- The other team, now on their 1st game of this new stint, plays on.

This repeats automatically. Admins just tap **"Game finished â†’ next game"** between
games â€” no winner needs to be selected again after Game 1.

**5. Everyone eventually cycles back through.**
Once your group of 5 rotates off, you go to the back of the bench and get pulled
back in for another 2 games once you reach the front again.

---

## The bench

- **Position numbers** show your place in line (only counting people who are
  actually available to play).
- **"Next game" / "2 games away" / etc.** gives a rough estimate of how many more
  games until you're pulled onto the court, based on the fact that one group of 5
  rotates in per game.
- **â˜‘ / â˜ (checkmark)** â€” toggles you (or anyone, for admins) between available
  and unavailable. If you're already on the court and get marked unavailable, you're
  pulled off immediately and the seat is backfilled from the bench â€” your team stays
  at a full 5, nobody else is affected.
- **âœ• (delete)** â€” *admin only.* Fully removes someone from the night (not just
  benching them). Asks for confirmation first. Use this for duplicate entries,
  wrong names, or someone who's definitely not coming.
- **Edit** â€” *admin only.* Fixes a typo in someone's name in place, wherever it
  appears (bench or on-court).
- **â–² / â–¼** â€” *admin only.* Nudges someone up or down the line, e.g. to land two
  friends in the same group of 5.

---

## Admin controls

Picking a winner, advancing games, undoing a game, editing names, deleting
players, toggling availability, reordering the bench, and starting a new night
all require a shared PIN. The first tap on any of these prompts for it; after
that, the device stays "unlocked" until someone taps the ðŸ”’/ðŸ”“ **Admin** button
in the header to lock it again.

**This is not real authentication** â€” it's just enough friction to stop
accidental taps. Anyone who knows the PIN can act as an admin on their own phone.

âš ï¸ **The PIN is stored in plain text in this file.** Search for `ADMIN_PIN` near
the top of the `<script>` section and change it before relying on it. Whoever
edits the code and re-uploads it controls what the PIN is.

### Other admin tools
- **â†© Back a game** â€” undoes the last winner-pick or "next game" action, restoring
  the exact rosters, bench order, and game count from right before it. Works
  multiple times in a row (up to 8 steps back).
- **New Night** â€” wipes everyone and resets the game counter to start fresh.
  There's no way to recover a night's data after this, so use History (below)
  first if you want a record of it.

---

## Other features

- **Live sync** â€” every phone sees the same queue and court in real time, no
  refreshing needed.
- **Game timer** â€” a running clock next to "Currently on court" shows how long
  the current game has been going, and resets each time a new game starts.
- **History** â€” a running log at the bottom of every game played tonight: who
  was on each team, when, and (for Game 1 only) who won.
- **Active viewers** â€” a low-key count at the very bottom showing roughly how
  many people have the page open right now.
- **Duplicate name warning** â€” if someone types a name that's already on the
  list, they're warned before it's added (and non-admins can't add a duplicate
  at all â€” only admins can override it).
- **Script last updated** â€” a timestamp at the very bottom of the page. Compare
  it after re-uploading a new version to confirm your phone actually picked up
  the update and isn't showing a cached copy.

---

## Tech stack

- **Single HTML file** (`index.html` on GitHub Pages) â€” no build step, no
  server. Just plain HTML/CSS/JavaScript.
- **Firebase Realtime Database** â€” powers the live multi-phone sync and the
  active-viewer count. Configured via the `firebaseConfig` object near the top
  of the `<script>` section.
- **Fallback mode** â€” if `firebaseConfig.apiKey` is left as the placeholder (or
  the file is opened outside a browser that has Firebase configured), the app
  still works, but only on one device at a time, saving to that browser's local
  storage instead of syncing across phones.

### Hosting

Hosted for free on **GitHub Pages** â€” deploys automatically from the `main`
branch whenever `index.html` is updated. There's no cost for the hosting itself,
and Firebase's free tier comfortably covers a single weekly pickup game (roughly
20â€“30 people, a couple hours a week) â€” realistically thousands of times over
before ever approaching a paid tier.

### Making changes

This app is built and maintained by editing `index.html` directly (via GitHub's
web editor) and re-uploading. There's no local dev environment required. After
any change:
1. Update the `SCRIPT_UPDATED` constant near the top of the `<script>` section
   to the current date/time, so the footer timestamp reflects the real update.
2. Upload the file to the GitHub repo, overwriting `index.html`.
3. Give it a minute for GitHub Pages to redeploy, then hard-refresh on a phone
   to confirm the new "Script last updated" time shows up.

---

## Known limitations

- **No real login** â€” "admin" is just a shared PIN, and "your name" on the bench
  isn't tied to any verified identity. This is by design, to keep joining
  frictionless for a casual weekly game.
- **Data doesn't persist across nights** â€” hitting "New Night" clears everything.
  There's currently no automatic archive of past nights (this could be added â€”
  ask if it'd be useful).
- **Rough time estimates** â€” "games away" and the game timer are based on the
  rotation rules and actual elapsed time, not a predicted game length, so they're
  estimates, not guarantees.
