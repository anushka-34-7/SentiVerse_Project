# SentiVerse

SentiVerse is a premium sentiment-analysis workspace where people can write, speak, or share an image and understand its emotional tone.

## Run & Operate

- `pnpm --filter @workspace/api-server run dev` — run the API server (port 5000)
- `pnpm run typecheck` — full typecheck across all packages
- `pnpm run build` — typecheck + build all packages
- `pnpm --filter @workspace/api-spec run codegen` — regenerate API hooks and Zod schemas from the OpenAPI spec
- `pnpm --filter @workspace/db run push` — push DB schema changes (dev only)
- Required env: `DATABASE_URL` — Postgres connection string

## Stack

- pnpm workspaces, Node.js 24, TypeScript 5.9
- API: Express 5
- DB: PostgreSQL + Drizzle ORM
- Validation: Zod (`zod/v4`), `drizzle-zod`
- API codegen: Orval (from OpenAPI spec)
- Build: esbuild (CJS bundle)

## Where things live

- `artifacts/sentiverse/src/App.tsx` — the rendered landing, auth, analysis, history, and settings experience.
- `artifacts/sentiverse/src/index.css` — SentiVerse theme tokens, responsive layout, and motion.
- `lib/api-spec/openapi.yaml` — source of truth for sentiment analysis and summary API contracts.
- `artifacts/api-server/src/routes/sentiment.ts` — API adapter for the Python engine.
- `artifacts/api-server/sentiment_engine.py` — dependency-free Python sentiment and visual-tone engine.

## Architecture decisions

- Sentiment scoring stays in Python so a trained model can replace the current lexicon/visual heuristic without redesigning the client contract.
- History is intentionally local to the browser for this first version; the settings screen makes that privacy behavior explicit.
- Text and browser voice dictation share the same analysis request shape; image input uses the same endpoint with a visual-tone path.

## Product

Users can enter SentiVerse through a lightweight name/email screen, analyze text or voice-transcribed text, attach an image for visual-tone analysis, revisit saved analyses, clear local history, and switch between designed dark and light themes.

## User preferences

- Keep the interface clearly about sentiment analysis rather than general-purpose chat.
- Preserve the premium black/navy/white/cyan direction and avoid purple as the dominant color.

## Gotchas

- The API workflow runs with `artifacts/api-server` as its working directory, so Python engine path resolution must support that cwd.

## Pointers

- See the `pnpm-workspace` skill for workspace structure, TypeScript setup, and package details
