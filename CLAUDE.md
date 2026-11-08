# peninsula.repair

A one-page community calendar of repair, maker, reuse and eco events on the SF Bay Peninsula. It's run by **Repair Café of the Peninsula** (RCP), a free monthly repair event in San Carlos (https://repaircafepeninsula.org/). The site is owned by RCP's organizer.

The site should feel like a local, hand-made community map. It should not look like RCP's own site or like a generic AI-made landing page.

## Files

- `index.html`: the whole site. Inline CSS and JS, Google Fonts only, no build step and no dependencies. Keep it as one static file unless the owner asks otherwise.
- `CLAUDE.md`: this file.

- `favicon.svg`: the two-bar mark, a short blue Coast & Hills bar beside a longer orange Bayside bar on a paper tile, with dark-mode colors built in. The header logo in index.html uses the same shape (blue bar shorter). Keep the two in sync.
- `CNAME`: the custom domain for GitHub Pages. Keep it as `peninsula.repair`.

## Hosting

- **GitHub Pages** from the public repo https://github.com/maxpereira/peninsularepair, branch `main`, root folder. Live at https://peninsula.repair (HTTPS enforced). DNS: apex A records point to GitHub's 185.199.108–111.153, and `www` is a CNAME.
- **Deploying = pushing to `main`.** The site rebuilds in about a minute. Only commit and push when the owner asks, and check the page in the preview first.

## Concept: "The Fix-It Line"

The page is a **transit-style route map of the Peninsula**. Scrolling the page is riding the line south. The owner is very happy with this concept, so keep it and build on it rather than replacing it.

- **Headline:** "Keep the Peninsula running." (orange period). It was chosen to cover repair, garden, nature and cleanup events, and to echo the transit theme. Don't go back to a repair-only slogan.
- **Departure board (hero):** an amber-on-black split-flap board showing the next 5 upcoming events. Clicking a row scrolls to and highlights that event on the map. It is generated from the event data, so never edit it by hand.
- **Two lines, running north to south:**
  - **Coast & Hills** (blue, left rail): coast and hill towns.
  - **Bayside** (orange, right rail): bayside towns.
- **Daly City** is the interchange where both lines meet (pill-shaped marker). **Mountain View** is the end of the line (buffer bar).
- **Stops are towns.** Events hang off their town as timetable rows: a big monospace date, a type tag, the title, time and venue, a note, and a "Details from the organizer ↗" link.
- **Empty stops stay on the map** in a muted style with "Host something here →". They invite submissions and keep the geography readable.
- **Highlight buttons** (Fix / Make / Grow / Outdoors / Cleanup) dim the events that don't match. They don't hide anything, so the map keeps its shape.
- **"End of the line. Unless you add a stop."** is the submission call to action. Next to it, a ticket-stub box credits RCP as the operator and holds the newsletter signup.

### Visual rules

- **Fonts:** Overpass (based on US highway signage) and Overpass Mono, nothing else.
- **Colors:** use the tokens on `:root`. `--bay` is orange, `--coast` is blue, `--amber` is for the board. Dark mode is defined both under `prefers-color-scheme` and under `[data-theme="dark"]`; add any new token to both.
- **Background:** a faint map-grid on paper.
- **Shapes:** heavy ink rules (3px) between sections, small 3px corner radius.
- **Avoid:** emoji, gradients, drop-shadow card grids, pill-shaped soft buttons, stock illustrations. Those made earlier drafts look AI-generated.
- **Tone:** plain, warm, local. Keep transit wordplay light: "stop", "line", "departures", "end of the line".
- **Layout:** must work at 375px wide (rails shrink there) and in dark mode. Respect `prefers-reduced-motion` (the board skips its flip animation).

## Data model (in the `<script>` at the bottom of index.html)

### `STOPS`: ordered north to south

```js
{ id: "sancarlos", name: "San Carlos", line: "bay" }   // line: "coast" | "bay" | "both"
```

Current order: Daly City (both), Pacifica (coast), South San Francisco (bay), Burlingame (bay), Half Moon Bay (coast), San Mateo (bay), San Carlos (bay), Redwood City (bay), Woodside (coast), Portola Valley (coast), Menlo Park (bay), Palo Alto (bay), Mountain View (bay).

- **Adding a town:** insert it by **latitude**. Coast/ocean and Skyline/hill towns (Montara, El Granada, La Honda, Los Altos Hills…) go on `coast`. Bayside and El Camino towns (Millbrae, Belmont, Foster City, East Palo Alto, Atherton…) go on `bay`.
- **Rails draw themselves:** each line runs from its first stop to its last, so ordering is all that matters.
- **Endpoints:** Mountain View must stay the last stop (the buffer marker is drawn on the last row). If a stop is added south of it, the buffer moves automatically.

### `KINDS`

`fix` (repair cafés, fixit clinics, mending, bike repair), `make` (maker/build workshops), `grow` (garden, compost, seeds, planting), `outdoors` (guided nature walks, tides, open space), `cleanup` (beach, creek and park cleanups). A highlight button only appears if at least one upcoming event uses that kind. Add a new kind only if events don't fit the existing ones.

### `EVENTS`

```js
{ stop: "sancarlos", kind: "fix", date: "2026-11-21", end: "2026-11-23" /* optional, multi-day */,
  time: "3–6pm", title: "…", venue: "Venue name, street address",
  note: "One or two short sentences: what to bring, registration, limits.",
  url: "https://organizer-page" }
```

- **Dates** are local `YYYY-MM-DD`. Events whose `end || date` is before today are hidden automatically, so old entries can stay but should be pruned now and then.
- **Times:** short lowercase style with an en dash (`10:30am–12:30pm`, `noon–2pm`, `9–11am`).
- **Venue:** the place name, then the street address. Leave out the city, because the stop already says it.
- **`url`** must point to the organizer's own page for that event whenever possible. A news roundup is only a fallback.

## Keeping events accurate (most important)

- **Only list real events confirmed from a source.** Never invent or guess dates, times or venues. If a detail is unknown, say so in `note` (e.g. "Check tide tables") or leave the event out.
- **After updating, change the "Listings last checked" date** in the footer.
- **When sources conflict, note it** in `note` and tell the owner (e.g. the Half Moon Bay cleanup meeting spot differed between sources).
- **Geographic scope:** San Mateo County, plus Palo Alto, Mountain View and Stanford-area Santa Clara County. San Jose, Sunnyvale and beyond are out of scope, even when the same organizer runs them.
- **Fit:** events must be free or low-cost community events about repair, reuse, making, gardening/compost, nature or cleanups. No commercial sales events, and no generic finance or real-estate seminars.

### Where to look for events

- **Repair Café of the Peninsula:** https://repaircafepeninsula.org/ (read the live homepage; the next event is shown at the top). Monthly, San Carlos, Callis & Elaine Community Center, 1336A Arroyo Ave.
- **San Mateo County Fixit Clinics:** https://www.smcsustainability.org/events/categories/fixit-clinics/ and the full calendar at https://www.smcsustainability.org/events/
- **Repair Café Silicon Valley:** https://repaircafesv.org/2026/ (only list its Palo Alto and Mountain View dates). Change the year in the URL each year.
- **Pacific Beach Coalition** (Pacifica and Half Moon Bay cleanups): https://www.pacificbeachcoalition.org/calendar-2026/. Half Moon Bay cleanups are usually the last Saturday of the month, 9–11am.
- **Midpen open space guided activities:** https://www.openspace.org/events
- **Palo Alto Online / The Almanac "home & garden events" roundups:** published every few weeks, good for discovery. Link to the original organizer when you can.
- **UC Master Gardeners (Santa Clara County):** https://ucanr.edu/site/uc-master-gardeners-santa-clara-county. Many sessions are in San Jose (out of scope), so check the venue.
- **Burlingame Public Library programs:** https://burlingame.libcal.com/ (repair cafés, mending, swaps)
- **Bay Area repair directory:** https://www.cultureofrepair.org/community-repair-events/ (it's often stale, so use it to find organizers, not dates)
- **RethinkWaste events:** https://rethinkwaste.org/events/ (blocks automated fetches; may need the browser)

## Placeholders and open items

- **Email addresses:** `events [at] peninsula [dot] repair` (submissions) and `hello [at] peninsula [dot] repair` (footer) aren't confirmed to exist.
- **Newsletter form:** doesn't save anything yet. It only swaps in a thank-you message and needs to be connected to the owner's mailing list provider.
- **The RCP box** quotes RCP's stats (56 of 79 items fixed after the first two events). Update the numbers from repaircafepeninsula.org as they change.
- **"Today" tag:** this is date-based only, so an event earlier today still shows "Today" after it has ended.
- **Not set up:** analytics, a social share image, and a PNG apple-touch-icon (iOS doesn't use SVG icons for home-screen shortcuts).
