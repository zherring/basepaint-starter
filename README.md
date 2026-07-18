# BasePaint Starter

A tiny, zero-dependency window into today's live [BasePaint](https://basepaint.xyz) canvas — theme, palette, the image repainting itself every 30 seconds, and live artist stats. One HTML file. No build step, no keys, no framework.

**It exists to be forked.** Turn it into a gallery, a game, a leaderboard, a screensaver, an art experiment — that's the point.

## Run it

```sh
python3 -m http.server 8000   # or any static server, or just open index.html
```

On **Replit**: remix this repl and hit Run.

## Build with AI

This repo ships with an [`AGENTS.md`](./AGENTS.md) that gives AI coding assistants (Replit Agent, Claude, Cursor…) everything they need: the public APIs, contract addresses, pixel format, and design language. Open the repo, tell your assistant what you want to make, and go. The full canonical reference lives at [basepaint.xyz/ai.txt](https://basepaint.xyz/ai.txt).

## The data you have to play with

- `GET basepaint.xyz/api/theme/{day}` — theme, proposer, palette (CORS enabled)
- `basepaint.xyz/api/art/image?day={day}&scale=4` — the live canvas; finished days at `basepaint.net/v3/{day}.png`, timelapses at `basepaint.net/animations/{day}.mp4`
- `graphql.basepaint.xyz` — every canvas, stroke, brush, artist, and payout since day 1
- The contracts themselves on Base — `paint()`, `mint()`, subscriptions, and the `Painted` event firehose

Day math: day 1 began at unix `1691599315`; a new canvas starts every 24h at 16:41:55 UTC.

## Going deeper

- [basepaint-mini](https://github.com/BasePaint/basepaint-mini) — a full minimal client (Preact + viem), the natural next step up
- [basepaint-ponder](https://github.com/BasePaint/basepaint-ponder) — the indexer behind the GraphQL endpoint
- [basepaint-contracts](https://github.com/BasePaint/basepaint-contracts) — ABIs and reference implementations

## BasePaint Year 3 Hackathon

This starter is the official starting line for the Year 3 Hackathon — build a tool for artists, collectors, or an art project on top of BasePaint. Details, prizes, and how to enter: **hack.basepaint.xyz**.

All BasePaint artwork is CC0. This starter is MIT.
