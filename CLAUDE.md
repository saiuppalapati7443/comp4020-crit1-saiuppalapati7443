# COMP4020 prototype

Your starter repo for a COMP4020 prototype: a static site in HTML/CSS/TypeScript
that builds to plain HTML/CSS/JS and deploys to GitHub Pages. The deployed site
is what gets marked, not this repo.

The
[course website](https://comp.anu.edu.au/courses/comp4020-agentic-coding-studio/)
publishes this deliverable's brief and spec, and this repo's name tells you
which deliverable applies. Read both before you plan or build.

## The link-preview card

`public/card.png` (1200x630) is the image a shared link shows; `index.html`'s
head points at it. Replace it and the `description` meta, and copy the head
block into any new page. The card URL resolves against the page that names it,
like any link --- `./card.png` is wrong one directory down, and nothing in CI
checks it, so the deployed head is the only place a broken one shows up.

## The checks

`pnpm check` runs them, and `pnpm check:evidence` is the extra gate before you
ship. CI runs the same plus links, secrets and the deploy.

`spec/README.md`, `PROCESS.md` and `reflections/README.md` are in this repo and
say what they are for.

## This week's harness: Crit 1, "Forgotten web"

**Topic:** a shrine to lost umbrellas. **Era:** GeoCities, circa 1998.

**The one idea:** every umbrella anyone has ever lost deserves a eulogy, not a
shrug, so this site is the memorial the modern world never built for them.

**Pages** (all linked from every page's nav, each with real readable content —
no lorem ipsum, no invented statistics or quotes):

- `index.html` — home. States the one idea, links to the other four.
- `about.html` — why this site exists and who's behind it. Personal,
  first-person, opinionated — not a corporate "about us."
- `collection.html` — the umbrella collection: a substantial list or
  description of specific (fictional-but-plausible, clearly personal) lost
  umbrellas, each with character.
- `field-notes.html` — umbrella field notes: observations, reflections, or
  informal "research" on lost umbrellas as a category of object. Essayistic.
- `sightings.html` — a guestbook-style "sightings" page: an in-page list of
  umbrella sightings presented as if submitted by visitors (written by me, not
  a real form — there's no JS to submit anything).

**Era rules — concrete and checkable:**

- Fonts: system serif/Times-style for body text, a Comic-Sans-style display
  font for headings — no modern sans-serif minimalism.
- Colours: bright, saturated link colours; visited links rendered in purple.
  A tiled or loud background (pattern or strong colour), not a neutral or
  white one.
- Layout: centred, table-style or fixed-width column layout — not a modern
  fluid grid.
- Signature elements, all pure HTML/CSS: a CSS-only marquee-style scrolling
  text banner, an "under construction" banner (on at least one page), a
  hit-counter look (a styled fake visitor-counter graphic/text), a webring
  footer (styled links, even if they go nowhere real), `<hr>` dividers
  between sections.
- Don't: modern cards, soft box-shadows, gradients used as UI chrome, or
  minimalist sans-serif typography anywhere.

**Hard constraints:**

- Plain HTML and CSS only. **Zero JavaScript** — no `<script>` tag anywhere,
  no `on*=` attributes. (`main.ts` and its `<script>` tag are deleted.)
- No external CDNs — no external fonts, icon kits, or scripts.
- Semantic HTML throughout.
- Every page has a `<title>` and a `lang` attribute.
- Every `<img>` has alt text.
- Visible focus states on all interactive elements (links).
- Readable contrast despite the era look — loud is fine, illegible isn't.
- The marquee/scrolling animation stops under `prefers-reduced-motion:
  reduce`.
- No horizontal scroll at 390px width.

**Facts:** don't invent statistics or quotes. If a page needs a fact, keep it
to things that are generally known, or make it clearly personal or opinion
(this is a shrine to *my* lost umbrellas, not a researched report).
