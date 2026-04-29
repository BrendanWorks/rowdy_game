# ROWDY

> Curated 5-round arcade game playlists for adults. Mobile-first, browser-based, built to be played in a group.

Live at **[rowdy.games](https://rowdy.games)**

---

## What It Is

ROWDY is a playlist-based arcade game platform. Each playlist is 5 rounds of varied mini-games -- trivia, visual memory, word puzzles, social opinion games, and reflex challenges -- mixed deliberately so that no two rounds feel the same. Think Cranium, but faster and meaner.

Playlists are themed (Booze, Junk Food, Silver Screen, etc.) and designed around a specific vibe. The scoring is real, the leaderboards are global, and the games are built for adult players who've outgrown pub quiz apps.

---

## Stack

| Layer | Tech |
|---|---|
| Frontend | React 18, TypeScript, Tailwind CSS, Framer Motion |
| Build | Vite 5 |
| Backend | Supabase (PostgreSQL + Auth) |
| Payments | Stripe |
| Audio | Web Audio API |
| Physics | Matter.js |
| Analytics | Google Analytics 4 |
| Hosting | Netlify |

---

## Quick Start

```bash
npm install
npm run dev      # http://localhost:5173
npm run build
npm run lint
```

`.env` required:
```
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
VITE_STRIPE_PUBLISHABLE_KEY=
```

---

## Game Library

### Knowledge & Trivia
| Game | Timer | Format |
|---|---|---|
| Multiple Choice | 90s | A, B, or C -- one right answer |
| True or False | 90s | Call the statement |
| Ranky | 90s | Arrange items in correct order |
| Odd Man Out | 60s | Find the 2 items that don't belong |
| Recall | 90s | Memorize, then answer from memory |
| Split Decision | 60s | Each item belongs to A, B, or Both |
| Hive Mind | 60s | Guess what most people answered |

### Visual & Pattern
| Game | Timer | Format |
|---|---|---|
| Zooma | 45s | Identify a photo as it zooms out |
| SnapShot | 30s | Drag puzzle pieces into position |
| Flashbang | 45s | Memorize tile pattern, tap it back |
| Fake Out | 60s | Real photo or AI-generated? |
| Tracer | 120s | Trace a shape from memory |
| ColorClash | 30s | Word color vs. ink color -- don't get fooled |

### Word & Logic
| Game | Timer | Format |
|---|---|---|
| Slot | 90s | Fill in the missing letters |
| Pivot | 60s | Find the word that connects two phrases |

### Comparisons
| Game | Timer | Format |
|---|---|---|
| Superlative | 60s | Which is bigger, older, more expensive? |

### Reflex & Action
| Game | Timer | Format |
|---|---|---|
| Clutch | 60s | Tap when the ring hits the target zone |
| Snake | 75s | Eat, grow, don't die |
| Debris | High score | Asteroids-style shooter |

---

## Project Structure

```
src/
├── components/         # Game components + GameSession registry
├── hooks/              # useAuth, useUserStats, useQuizRound
├── lib/                # supabase, audioManager, scoringSystem, analytics
├── context/            # OfflineContext
└── utils/              # submissionQueue (batched score uploads)
```

---

## Database Schema

Core tables:

- `games` -- game registry
- `playlists` -- themed collections
- `playlist_rounds` -- round assignments per playlist (game, puzzle, metadata)
- `puzzles` -- question/content pool
- `ranking_puzzles` / `ranking_items` -- Ranky puzzle data
- `superlative_puzzles` -- Superlative puzzle data
- `hive_mind_puzzles` -- Hive Mind survey data
- `user_profiles` -- player accounts
- `game_sessions` -- session records
- `round_results` -- per-round scores
- `leaderboard_entries` -- global rankings

All tables use Row-Level Security. User data is isolated. Content management is admin-only.

### Key Schema Notes

- `playlist_rounds` has a `UNIQUE(playlist_id, round_number)` constraint
- Ranky rounds use `ranking_puzzle_id` column; metadata should be `{}`
- MC/Odd Man Out/Fake Out rounds use `metadata: { "puzzle_ids": [...] }`
- True or False uses `metadata: { "puzzleIds": [...] }` (camelCase -- intentional)
- Superlative rounds use `superlative_puzzle_id` column
- Hive Mind rounds use `puzzle_id` column directly
- Procedural games (Debris, Snake, etc.) use `game_id = NULL` with `metadata: { "game_slug": "..." }`
- Games with existing `round_results` records cannot be deleted -- use `UPDATE SET is_active = false`

---

## Scoring

Each round is worth up to 1000 points. Final session score (5 rounds = 5000 max) maps to a grade:

| Grade | Points |
|---|---|
| A+ | 4750 -- 5000 |
| A | 4500 -- 4749 |
| B | 4000 -- 4499 |
| C | 3500 -- 3999 |
| F | < 3500 |

Speed bonuses apply. Harder games carry higher multipliers.

---

## Adding a Playlist

1. Insert a row into `playlists`
2. Insert 5 rows into `playlist_rounds` (one per round, `round_number` 1--5)
3. Each round needs a `game_id` and either a `puzzle_id`, `ranking_puzzle_id`, `superlative_puzzle_id`, or `metadata` object pointing to puzzle content -- depending on game type
4. Verify with a local playthrough before deploying

---

## Adding a Game

1. Create `src/components/[GameName].tsx`
2. Register it in `GameSession.tsx` with a unique `id`, `dbId`, `duration`, and `instructions`
3. Insert a row into the `games` table
4. Test locally, then deploy

---

## Deployment

Auto-deploys to Netlify on push to main via GitHub. SPA routing handled in `netlify.toml`.

```bash
npm run build   # dist/ ready for deploy
```

---

## Known Constraints

- **Offline resilience**: network loss currently crashes to a browser error page -- top post-launch priority
- **Apple OAuth**: requires certificate setup, not yet enabled
- **Stripe**: live mode activation pending -- switch keys, update Netlify env vars, test real transaction

---

## Design System

Dark neon arcade aesthetic. Full spec in `NEON_STYLE_GUIDE.md`.

- Background: pure black (`#000000`)
- Primary UI: cyan (`#00ffff`)
- Scores: yellow (`#fbbf24`)
- Navigation: pink/magenta (`#ec4899`)
- Danger/wrong: red (`#ef4444`)
- Correct: green (`#22c55e`)
- No gradients. No white backgrounds. Borders minimum 2px.

---

## License

Private. All rights reserved.
