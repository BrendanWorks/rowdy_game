# Zooma - Arcade Game Platform

> A fast-paced, engaging arcade quiz game platform with 20+ mini-games designed to test knowledge, reflexes, and quick thinking.

![Zooma](https://img.shields.io/badge/Status-Production-success) ![Games](https://img.shields.io/badge/Games-20+-blue) ![Users](https://img.shields.io/badge/Leaderboards-Global-brightgreen)

---

## Quick Start

```bash
npm install
npm run dev      # Start dev server
npm run build    # Production build
npm run lint     # Check code quality
```

Visit `http://localhost:5173` to play.

---

## Game Collection

### Knowledge & Trivia
| Game | Time | Challenge |
|------|------|-----------|
| **Ranky** | 90s | Arrange items in correct order |
| **Odd Man Out** | 60s | Spot 2 items that don't belong |
| **Multiple Choice** | 90s | Pick the right answer (A, B, or C) |
| **True or False** | 90s | Call out factual statements |
| **Recall** | 90s | Remember items from memory |

### Visual & Pattern Recognition
| Game | Time | Challenge |
|------|------|-----------|
| **Zooma** | 45s | Identify photos as they zoom out |
| **SnapShot** | 30s | Drag puzzle pieces to complete the image |
| **Flashbang** | 45s | Memorize lit tiles, tap them back |
| **Tracer** | 120s | Trace a shape from memory |
| **ColorClash** | 30s | Tap color names vs. the ink color (tricky!) |

### Social & Opinion
| Game | Time | Challenge |
|------|------|-----------|
| **Hive Mind** | 60s | Guess what most people chose |
| **Superlative** | 60s | Compare which item is bigger, older, etc. |
| **Fake Out** | 60s | Real photo or AI-generated? |

### Word & Puzzle
| Game | Time | Challenge |
|------|------|-----------|
| **Split Decision** | 60s | Categorize items: A, B, or BOTH |
| **Slot** | 90s | Fill blanks with correct letters |
| **Pivot** | 60s | Find the word connecting two phrases |

### Action & Reflexes
| Game | Time | Challenge |
|------|------|-----------|
| **Snake** | 75s | Eat food, avoid walls (classic arcade) |
| **Clutch** | 60s | Tap when the ring hits the sweet spot |

### Experimental
| Game | Time | Challenge |
|------|------|-----------|
| **Debris** | High score | Rotate, thrust, shoot asteroids Asteroids-style |

---

## Features

### Player Experience
- ✨ **Smooth Animations** - Framer Motion for polished transitions
- 🎮 **Multiple Game Modes** - Quiz playlists, solo challenges, leaderboards
- 🏆 **Global Leaderboards** - Real-time rankings with badges
- 📊 **Score Tracking** - Personal stats and progress
- 🎵 **Audio Feedback** - Optional sound effects and music
- 📱 **Mobile-First** - Optimized for touch devices
- 🔐 **Authentication** - Optional account creation for leaderboards
- 📊 **Analytics** - Understand player engagement

### Technical
- **React 18** - Modern component architecture
- **TypeScript** - Type-safe code
- **Tailwind CSS** - Responsive design system
- **Vite** - Lightning-fast builds
- **Supabase** - Real-time database & auth
- **React GA4** - Google Analytics integration

---

## Architecture

### Project Structure

```
src/
├── components/          # 20+ game implementations
│   ├── GameSession.tsx  # Main game container & registry
│   ├── GameMenu.tsx     # Playlist selection
│   └── [GameName].tsx   # Individual games
│
├── hooks/              # React hooks for state management
│   ├── useAuth.ts      # Authentication
│   ├── useUserStats.ts # Player statistics
│   └── useQuizRound.ts # Round state
│
├── lib/                # Utilities & services
│   ├── supabase.ts     # DB client
│   ├── audioManager.ts # Sound effects
│   ├── scoringSystem.ts # Point calculation
│   └── analytics.ts    # Event tracking
│
├── context/            # Global state
│   └── OfflineContext.tsx  # Offline mode support
│
└── utils/              # Helpers
    └── submissionQueue.ts  # Batch score uploads
```

### Database Schema

**Core Tables:**
- `games` - Game registry (20 games)
- `playlists` - Quiz collections (32 playlists)
- `playlist_rounds` - Game assignments per playlist
- `puzzles` - Quiz questions/content
- `user_profiles` - Player info
- `game_sessions` - Play sessions
- `round_results` - Individual round scores
- `leaderboard_entries` - Ranking system

**Security:**
- Row-Level Security (RLS) policies on all tables
- User data isolation
- Admin-only content management

---

## Game Registry

All games are defined in `src/components/GameSession.tsx`:

```typescript
const GAME_REGISTRY = [
  {
    id: 'odd-man-out',
    name: 'Odd Man Out',
    component: OddManOut,
    duration: 60,
    dbId: 3,  // Links to database
    instructions: "Select the 2 items that don't belong"
  },
  // ... 19 more games
];
```

Each game has:
- **Unique ID** for routing and analytics
- **Duration** in seconds (for score multipliers)
- **Database ID** for score tracking (null for procedural games)
- **Component** - The actual game UI
- **Instructions** - Player guidance

---

## Scoring System

### Base Score Calculation
- Each game worth up to 1000 points
- Accuracy matters: correct answers = points
- Speed bonus: faster completion = bonus multiplier
- Difficulty adjustment: harder games worth more

### Session Grading
| Grade | Range | Threshold |
|-------|-------|-----------|
| A+ | 95-100% | 475-500 points |
| A  | 90-94% | 450-474 points |
| B  | 80-89% | 400-449 points |
| C  | 70-79% | 350-399 points |
| F  | <70%  | <350 points |

### Leaderboard Badges
- 🦅 **Eagle Eye** - 50+ games completed
- 📚 **Wordsmith** - High accuracy on word games
- 🌍 **Zeitgeist** - Strong superlative game performance
- 👾 **Arcade King** - 100+ rounds played

---

## Development

### Adding a New Game

1. **Create component** at `src/components/[GameName].tsx`
2. **Add to GAME_REGISTRY** in `GameSession.tsx`
3. **Create database record** for tracking (if needed)
4. **Test locally** with `npm run dev`
5. **Deploy** with `npm run build`

### Testing Games
```typescript
// Run build to verify no errors
npm run build

// Check bundle size
npm run build  # See output stats

// Lint code
npm run lint
```

---

## Deployment

### Production Build
```bash
npm run build  # Creates optimized dist/
```

### Environment Variables
Create `.env`:
```
VITE_SUPABASE_URL=your_supabase_url
VITE_SUPABASE_ANON_KEY=your_anon_key
```

### Netlify Configuration
Included in `netlify.toml`:
- Auto-deploys from git
- 404 handling for SPA routing
- Environment variables support

---

## Performance

### Bundle Analysis
- Main JS: ~215 KB (57 KB gzipped)
- CSS: Inline with Tailwind
- Images: Lazy-loaded where possible
- Games: Code-split for faster loading

### Optimization Features
- Lazy component loading (React.lazy)
- Audio context pooling
- Submission batching (5-second debounce)
- Offline mode support

---

## Analytics

Events tracked:
- `game_started` - When a player begins
- `game_completed` - Round finished with score
- `playlist_selected` - User chooses a playlist
- `user_signed_up` - New account created
- Custom events per game type

Metrics:
- Average game duration
- Completion rate per game
- Most popular playlists
- Score distribution

---

## Troubleshooting

### Game Won't Load
- Check browser console for errors
- Verify Supabase connection
- Clear localStorage: `localStorage.clear()`

### Scores Not Saving
- Ensure authentication is working
- Check network tab for failed requests
- Review Supabase RLS policies

### Audio Issues
- Allow microphone/speaker permissions
- Check volume control in game menu
- Test audio context availability

---

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make changes
4. Run tests and linting
5. Submit pull request

---

## Tech Stack

| Category | Technology |
|----------|-----------|
| **Frontend** | React 18, TypeScript, Tailwind CSS |
| **Bundler** | Vite 5 |
| **Backend** | Supabase (PostgreSQL) |
| **Auth** | Supabase Auth |
| **Analytics** | Google Analytics 4 |
| **Animations** | Framer Motion |
| **Icons** | Lucide React |

---

## License

Private - All rights reserved

---

## Support

Issues? Questions? Email support or file an issue on GitHub.

---

**Play. Score. Compete. Dominate.** 🎮🏆

