# build-bench — a realistic Astro build, for provider benchmarking

A **generated** Astro site used to measure build runners (Clever Cloud, Scaleway, our own
worker). It exists because earlier spikes benchmarked a one-page site with an 8x8 px image,
which proved a toolchain runs and nothing about what a real build needs.

Calibrated against a real 76-page client site: it fails the same way at the same memory.

| | Pages | Image variants | dist files | 512 MB | 1 GB |
|---|---|---|---|---|---|
| Real client site | 76 | 265 | 368 | OOM at 173/265 | 21 s |
| **This bench** | 81 | 300 | 392 | **OOM at 124/300** | **22 s** |

Shape deliberately matches the real site: Astro 6, Tailwind 4 via `@tailwindcss/vite`, lucide
icons, two fontsource variable fonts, `clsx`/`tailwind-merge`/`tailwind-variants`, a sitemap
integration, and `sharp` doing real work (100 source images at 1600x1200, three widths each).

**`vite` is pinned on purpose.** Astro 6 otherwise pulls in rolldown-vite, and
`@tailwindcss/vite` fails against it with `Missing field 'tsconfigPaths'`. The client site only
builds because it pins vite too.

Everything here is generated — no client content, no secrets. `main` still holds the original
one-page spike site, kept for comparison.
