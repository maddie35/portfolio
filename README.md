# Portfolio — Maddie Reardon (Technical Writing)

Technical writing portfolio site for Maddie Reardon, senior technical writer. Static HTML, no build step, no dependencies. Sibling site to [learning-portfolio](https://maddie35.github.io/learning-portfolio/), which covers the learning design side of the practice.

## Pages

| File | Purpose |
| --- | --- |
| `index.html` | Home, with the samples index and "How I work" |
| `samples.html` | All five samples, sorted by the problem solved |
| `about.html` | Background, track record |
| `request-access.html` | How to request the two client-owned locked samples |
| `sample-affirm.html` | Sample — Affirm checkout documentation (excerpt) |
| `sample-black-holes.html` | Sample — Introduction to black holes (full explainer) |
| `sample-information-architecture.html` | Sample — Affirm developer docs IA overhaul |
| `sample-manage-connections.html` | Sample — Manage connections help topic |

`assets/styles.css` holds the design tokens and component classes (`.blueprint`, `.btn`, `.tag`, `.card`). It is the same "Industry" design system used by the learning-portfolio site — retune variables in `:root` rather than overriding at the page level.
