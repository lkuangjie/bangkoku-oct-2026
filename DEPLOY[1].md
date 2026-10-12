# Put the Bangkok guide on your iPhone for offline use

The claude.ai artifact needs internet. For no-signal use, deploy the offline app (`bkk-guide-pwa.zip`) to GitHub Pages, the same way as your push-up tracker. A small service worker saves the whole guide on the phone after the first open.

## One-time setup (about 5 minutes)

1. On github.com, create a new repository. Use a name that's hard to guess (for example `bkk-guide-x7q2`), because a GitHub Pages site is public on free plans and the guide shows your hotel and travel dates.
2. Unzip `bkk-guide-pwa.zip` and upload all its files to the repo root: Add file → Upload files, drag them in, Commit.
3. Settings → Pages → Build and deployment → Deploy from a branch → `main` / `(root)` → Save.
4. Wait a minute or two, then open the address GitHub shows (`https://<your-username>.github.io/<repo-name>/`) in Safari on the iPhone.
5. Scroll to the bottom of the Lists & info tab. It should say "Saved on this phone for offline use."
6. Share → Add to Home Screen.

## Check it works offline

Turn on Aeroplane Mode, open the app from the home screen, and switch between Itinerary, Map and Lists. Everything should load.

What still needs internet: the "Open in Google Maps" buttons (they hand off to the Maps app), and the web fonts the very first time (after that they are cached; without them the page falls back to system fonts).

## Updating

Rebuild (see README.md), upload the new files from `dist/pwa/` over the old ones, and commit. Open the app once with internet; it fetches the new version and keeps it for offline use. If an old version sticks, close the app fully and reopen it while online.

## Notes

- Shopping-list ticks are saved on the phone, separately from the claude.ai version.
- Before you fly, also save your saved list for offline use in Google Maps (download the Bangkok area as an offline map) so walking directions work without data.
