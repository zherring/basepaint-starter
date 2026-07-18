# AGENTS.md — BasePaint Starter

Context for AI coding assistants (Replit Agent, Claude, Cursor, etc.) working in this repo.

## What this repo is

A ~150-line, zero-dependency web page showing today's live BasePaint canvas: theme, palette, live-updating image, and artist/pixel stats. It exists to be **forked and turned into something else** — a gallery, a game, a leaderboard, an art experiment. It's the starter for the BasePaint Year 3 Hackathon (hack.basepaint.xyz).

There is no build step, no framework, no package.json. `index.html` is the whole app. Keep that spirit unless the user asks for more: prefer adding to `index.html` (or splitting out plain JS/CSS files) over introducing a toolchain.

## Run it

Any static server, e.g. `python3 -m http.server 8000`, or just open `index.html`. On Replit, hit Run.

## Where the data comes from (all public, no keys)

Everything the page uses, plus the surfaces you'll want when extending it. Full canonical reference: **https://basepaint.xyz/ai.txt** — fetch it before doing anything nontrivial.

- **Day math**: one canvas per day, day 1 started at unix `1691599315`, 86400s each; day flips 16:41:55 UTC. `day = floor((now - 1691599315)/86400) + 1`. Token id == day number.
- **Theme + palette**: `GET https://basepaint.xyz/api/theme/{day}` → `{theme, proposer, size, palette[]}` (CORS enabled).
- **Live canvas image**: `https://basepaint.xyz/api/art/image?day={day}&scale=4` (in progress); finished days live at `https://basepaint.net/v3/{day}.png`, timelapses at `https://basepaint.net/animations/{day}.mp4`.
- **GraphQL indexer**: `POST https://graphql.basepaint.xyz/` — canvases, strokes (raw pixel bytes), brushes, accounts, earnings. Plural queries return `{items{...}}` with `where`/`orderBy`/`limit`; paginate with `where: {id_gt: $lastId}`. Singular lookups take Int ids: `{ canvas(id: 1075) { totalArtists pixelsCount } }`.
- **Contracts** (Base mainnet, chain 8453): BasePaint `0xBa5e05cb26b78eDa3A2f8e3b3814726305dcAc83` (`paint`, `mint`, `today`, `Painted` events), Brush `0xD68fe5b53e7E1AbeB5A4d0A6660667791f39263a`, Subscription `0x75CF063a65d361527180805b244bC51c1deAb075`, MetadataRegistry `0x5104482a2Ef3a03b6270D3e931eac890b86FaD01`. Full list + concepts in ai.txt.
- **Pixel format**: 3 bytes per pixel — x, y, palette index. Reference decoders: `basepaint-mini/src/pixels.ts` (TS), `bpverify-cli` (Go).

## Design language

Match BasePaint: bg `#1E2735`, text `#ffffff`, accent `#fde047`, headers `#073eb1`; Roboto Mono / Viga / MEK Sans. Canvases are pixel art — always render images with `image-rendering: pixelated`.

## Bigger reference codebases

- https://github.com/BasePaint/basepaint-mini — full minimal client (Preact + viem), the step up from this starter
- https://github.com/BasePaint/basepaint-ponder — the indexer, self-hostable
- https://github.com/BasePaint/basepaint-contracts — ABIs + tests

## Ground rules for agents

- All artwork is CC0; the APIs above are public and keyless — no auth flows needed.
- Don't hardcode a day number; compute it (users will run this on a different day than you).
- Verify queries against the live GraphQL endpoint before shipping them — entity/field names are ponder-generated and easy to guess wrong.
