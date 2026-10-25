<!-- DRAFT: written by the agent from this session only. Rewrite this in your
     own words before the crit --- this is a starting point, not the
     submission. -->

## What was the breakthrough that moved the work forward?

The breakthrough was writing the harness before touching a single page. Once
`CLAUDE.md` fixed the topic, the five pages, the concrete era rules (fonts,
colours, layout, the specific signature elements), and the hard constraints
(zero JavaScript, semantic HTML, alt text, reduced motion, no horizontal
scroll) as checkable facts, every page after that got built against a fixed
target instead of being improvised one at a time. It also meant the checks
had something real to catch: the aria-label misuse and the 390px overflow
bug both turned up because the constraints were explicit enough to test
against, not because I eyeballed every page for a mistake.

## What did this work change about who I want to be as a software developer?

I want to be someone who treats "it looks fine" as a claim to verify, not a
conclusion. The screenshots caught a horizontal-scroll bug on the one page I
was most confident about, and the HTML validator caught an ARIA misuse I
wouldn't have noticed just by reading the markup. Neither check was required
by the starter — I added both because I wanted a second opinion beyond what
was already there. I think that's the habit worth keeping: build the thing,
then go looking for the ways it's wrong, rather than trusting that it's
right because it was built carefully.
