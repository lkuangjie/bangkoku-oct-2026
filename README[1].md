# Bangkok Area Guide: how to update it

The guide is generated. You edit plain data, run one command, and get two outputs:

- `dist/index.html`: the version published to the claude.ai artifact (online only).
- `dist/pwa/` and `dist/bkk-guide-pwa.zip`: the offline app for your iPhone (see DEPLOY.md).

## The four source files

| File | What lives here | Edit it when |
|---|---|---|
| `data.py` | Hotel, areas (name, day, colour, history, facts, getting there), plan stops, and which area each saved place belongs to | You add, move or remove a stop, change hours, or write area notes |
| `build.py` | The day-by-day itinerary (`DAYS`) and the drawn map (roads, canals, river, landmarks, district names) | You change the order or timing of a day |
| `saved.tsv` | Your Google Maps "Bkk" list: name, exact lat/lng, your note, address, category, hours | Your saved list changes |
| `template.html` | Page layout, styling and map drawing code | Only for design changes |

## Common edits

**Add a stop to a day.** In `data.py`, copy an existing line in `PLAN` and change it. Example:

```python
dict(id="padseeew1", a="china", n=3, name="Example Pad See Ew", lat=13.74100, lng=100.50900,
     t=["food","must"], addr="Yaowarat Rd", meta="Daily 10:00–20:00", desc="Why it's worth it."),
```

- `id`: any unique short word.
- `a`: area id. Plan areas: `home, banthat, suk, old, china, songwat, talatnoi, ari, chan, khlongsan, silom, siam`. Detours: `nangloeng, talatphlu, kudichin, bangkrachao, jodd, elsewhere`.
- `n`: walk order inside the area (the number on the pin). Use `0` for "nearby, not in the walk".
- `lat, lng`: copy from Google Maps (long-press the place, the numbers appear at the top). Five decimals is enough.
- `t`: tags. `must` (red ring), `nice` (good to have), `kj`, `wife`, `food`, `kopi` (old-school coffee).
- Optional: `saved=True`, `note="your note"`, `warn="clash text"` (shows in red), `unv=True` (Unverified badge), `approx=True` (pin is approximate).

Then put it on the day's timeline in `build.py`, inside `DAYS`, as `["14:00", "padseeew1", "One-line instruction."]`. Use `None` instead of an id for a line with no place.

The walking map for each day is built automatically from the order of the stops in `DAYS` (starting and ending at the hotel). Reorder the lines to change the route. Legs over 1.2 km show as Grab rides; change `WALK_MAX` in `template.html` to adjust that.

**Change opening hours or a description.** Edit `meta` or `desc` for that stop in `data.py`.

**Your Google Maps list changed.** Add or edit the row in `saved.tsv` (tab-separated). Then give the place an area in `SAVED_MAP` in `data.py`: `"Exact name": ("area_id", ["food"], None),` or a warning string instead of `None`. The build stops with an error if a saved name has no area, so nothing gets lost.

## Rebuild

On a Mac, in the folder with these files:

```
python3 -m pip install pillow   # first time only, for the app icons
python3 build.py
```

It prints the number of places and a version code. Open `dist/pwa/index.html` in Chrome to check it before deploying.

## Publish the update

- **Offline app (iPhone):** upload the contents of `dist/pwa/` to your GitHub Pages repo, replacing the old files. Next time the phone opens the app with internet, it picks up the new version and keeps it for offline use.
- **claude.ai artifact:** ask Claude in the Holidays project to "rebuild and republish the Bangkok guide" (it reads these files from the project).

## Easiest route

Open a new chat in the Holidays project and say what changed, for example "add X to Tuesday after Song Wat". Claude reads `claude/bkk-guide-src/`, edits, rebuilds, republishes the artifact, saves the source back, and sends you a fresh `bkk-guide-pwa.zip`.
