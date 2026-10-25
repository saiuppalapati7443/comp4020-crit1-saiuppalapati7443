# Process overview

**Note on where this was built:** the course never provisioned a
`comp4020-crit1-<handle>` repo for me in the course org (checked via `gh repo
list` and a GitHub search across the org — no matches, and `doctor` confirmed
I'm an active org member, so it's a provisioning gap, not a membership one).
With the convenor's permission to do Crit 1 late, I built it in my own GitHub
account instead:
[`saiuppalapati7443/comp4020-crit1-saiuppalapati7443`](https://github.com/saiuppalapati7443/comp4020-crit1-saiuppalapati7443),
on **26 October 2026**, from the course's public `template-static` starter. I
intend to show this to my tutor and ask for it to be moved into the course org.

## What I built

A Shrine to Lost Umbrellas: a five-page, pure HTML/CSS site styled like a
1998 GeoCities fan page, memorialising umbrellas I've personally lost, plus
a running log of other people's abandoned umbrellas I've noticed around
town. The idea is simple — nobody keeps a record of lost umbrellas, so I
built one, and the look (loud colours, table-style layout, marquee,
under-construction banner, webring footer) is the deliberate joke that
makes the sincerity of the content land.

## How I got here

I worked with Claude Code end to end, directing it at each stage rather
than asking for the whole site at once.

**Harness before any page.** My first instruction fixed the topic (umbrellas,
GeoCities era), the five-page structure, the era rules as concrete
checkable constraints (zero JavaScript, no CDNs, semantic HTML, alt text,
visible focus, reduced-motion support, no horizontal scroll at 390px), and a
no-invented-facts rule — written into `CLAUDE.md` before any HTML existed:
[`70a2fad`](https://github.com/saiuppalapati7443/comp4020-crit1-saiuppalapati7443/commit/70a2fad).

**Build in small commits.** Stylesheet first
([`41f095b`](https://github.com/saiuppalapati7443/comp4020-crit1-saiuppalapati7443/commit/41f095b)),
then the home page with `main.ts` deleted since the brief rules out JS
([`e66d1e7`](https://github.com/saiuppalapati7443/comp4020-crit1-saiuppalapati7443/commit/e66d1e7)),
then the four content pages one at a time, each its own commit
([`5824904`](https://github.com/saiuppalapati7443/comp4020-crit1-saiuppalapati7443/commit/5824904)...[`20e9d6e`](https://github.com/saiuppalapati7443/comp4020-crit1-saiuppalapati7443/commit/20e9d6e)).
I ran `pnpm check` after every page and confirmed it stayed green (18 tests
at the harness stage, growing to 50 once all five pages existed) before
committing.

**Checks caught two real bugs I hadn't asked about.** After the five pages
existed, I had the agent grep the whole site for `<script` and
`on[a-z]+=` (zero matches — confirmed no JS anywhere), run `linkinator`
against the built `dist/` (all internal links resolved), and run
`html-validate` against the built HTML, which it isn't asked to do by the
starter — I added it because I wanted a second opinion beyond the starter's
own invariants. That surfaced an invalid `aria-label` on a decorative `<p>`
(ARIA doesn't apply to a plain paragraph, and the visible text already said
the same thing), fixed in
[`5203a86`](https://github.com/saiuppalapati7443/comp4020-crit1-saiuppalapati7443/commit/5203a86).
Then Playwright screenshots at 390px and 1280px for every page turned up a
real layout bug: the home page had horizontal scroll at 390px. The cause was
`.page-frame`'s `display: table` defaulting to `table-layout: auto`, so the
marquee's long, non-wrapping scrolling text forced the whole table wider
than the viewport instead of being clipped by `overflow: hidden` on its
parent. Fixed with `table-layout: fixed` in
[`f885832`](https://github.com/saiuppalapati7443/comp4020-crit1-saiuppalapati7443/commit/f885832),
and I re-ran the screenshot check afterward to confirm the fix actually held
across all five pages at both viewports, not just the one page I'd noticed
it on.

**My corrections, after reviewing the screenshots.** I asked for two
changes:

1. The sightings page read as invented guestbook posts from other people —
   "written in the voice of the people who might report them." I wanted it
   to be clearly mine: my own log of umbrellas I've personally seen
   abandoned, not a fabricated guestbook. I asked for a rewrite with a
   consistent format (place, colour, condition, one-line note), and for a
   permanent rule to be added to the harness so this didn't recur:
   [`a2dfce2`](https://github.com/saiuppalapati7443/comp4020-crit1-saiuppalapati7443/commit/a2dfce2)
   added the rewrite and the `CLAUDE.md` rule ("no invented visitors,
   testimonials or quotes; all voices on the site are mine") in the same
   commit.
2. Looking at the screenshots, the under-construction banner showed broken
   glyph boxes instead of the emoji I'd used — a font-rendering gap, and also
   not period-accurate (GeoCities didn't have emoji). I asked for it to be
   swapped for ASCII, fixed in
   [`8447fa1`](https://github.com/saiuppalapati7443/comp4020-crit1-saiuppalapati7443/commit/8447fa1).

The agent also caught its own inconsistency after the sightings rewrite: the
home page still described the sightings page as "sightings reported by
visitors to this site," which was now stale. It fixed and committed that on
its own in
[`3b49413`](https://github.com/saiuppalapati7443/comp4020-crit1-saiuppalapati7443/commit/3b49413)
rather than leaving a contradiction between two pages.

**Shipping.** Because this repo lives in my own GitHub account rather than
the course org, the course's `/ship` skill (which flips a course-org repo
public and enables Pages for it) didn't apply. I created the repo public
from the start and had the agent enable GitHub Pages directly via
`gh api`, set to build from the GitHub Actions workflow the starter already
ships (`actions/deploy-pages`), then pushed and let CI deploy it. It curled
every page's live URL to confirm all five return `200` before reporting the
site as live.

**Verifying along the way.** For every commit, "the checks" meant
`pnpm check` (typecheck, build, vitest — the starter's shipped invariants:
nav landmark, one `<h1>`, `lang`, title, meta description, `og:image`,
viewport meta, alt text), plus a manual `grep` for script tags and inline
event handlers, `linkinator` against the built site for internal links, and
`html-validate` against the built HTML, which isn't part of `pnpm check`
but which I asked for as extra backpressure beyond the starter's own
invariants. For the 390px/1280px check, Chromium itself needed system
libraries this sandboxed Linux environment didn't have and couldn't `sudo
apt install` — the agent downloaded the `.deb` packages without installing
them, extracted the shared libraries into a scratch directory, and pointed
`LD_LIBRARY_PATH` at them for the one Playwright run, rather than skipping
the screenshot check. `playwright` itself was added as a temporary dev
dependency for each screenshot pass and removed again afterward; I had it
confirm `package.json` and `pnpm-lock.yaml` were byte-identical to the
previous commit each time, since a stray dependency left behind would have
been a mistake nobody asked for.

## Before you ship

`pnpm check:evidence` is green: this file no longer contains the template
comment, every cited commit resolves in this repo, `reflections/crit-1.md`
exists, and `CLAUDE.md` is present.
