# GitHub Cleanup Strategy

## Goal
Remove testing/legacy games that clutter the repository while maintaining a clean, production-ready codebase.

---

## Analysis Documents to Archive

These internal analysis documents should be moved to a `.github/docs` directory or removed from root:

| File | Action | Reason |
|------|--------|--------|
| `CODE_REVIEW_ANALYSIS.md` | Archive | Internal audit, not needed in production |
| `MATTER_JS_REVIEW.md` | Archive | Legacy physics engine review |
| `OPTIMIZATION_CHANGES.md` | Archive | Historical optimization notes |
| `PHYSICS_ENGINE_AUDIT_SUMMARY.md` | Archive | Legacy audit documentation |
| `UNUSED_GAMES_ANALYSIS.md` | Archive | Development analysis document |
| `vite.config.ts.timestamp-*` | Delete | Build artifact |

**Action:** Create `.github/docs/` directory and move these files there for documentation purposes.

---

## Games to Handle

### 1. Procedural-Only Games (No Playlist Integration)
These games work locally but have **no database entries** and aren't in any playlists. They run but don't track scores or persist data.

| Component | Status | Recommendation |
|-----------|--------|-----------------|
| **WordRescue.tsx** | Procedural only | Archive or remove |
| **ShapeSequence.tsx** | Procedural only | Archive or remove |
| **SlopeRider.tsx** | Procedural only | Archive or remove |
| **NeuralPulse.tsx** | Procedural only | Archive or remove |
| **ZenGravity.tsx** | Procedural only | Archive or remove |
| **UpYours.tsx** | Procedural only | Archive or remove |

**Options:**
- **Option A (Aggressive):** Delete all procedural-only games. They add 60KB+ to bundle size and clutter the codebase.
- **Option B (Conservative):** Archive in `src/components/archived-games/` and remove from GAME_REGISTRY.
- **Option C (Gradual):** Keep as-is for now, but mark as deprecated in code comments.

**Recommended:** Option A - Delete entirely. If they're needed later, they can be recovered from git history.

### 2. Legacy Database Entries (Not Used in Playlists)

These have database records but aren't used:

| Game | DB ID | Status | Action |
|------|-------|--------|--------|
| **DoubleFake** | 16 | Marked inactive | Keep DB record, remove component or archive |
| **Emoji Master** | 1 | Legacy | Remove DB entry (no component exists) |
| **Split Decision (legacy)** | 9 | Duplicate | Remove DB entry (superseded by ID 7) |

**Action:**
1. Create migration to mark legacy DB entries as archived
2. Remove `DoubleFake.tsx` component from src/
3. Remove from GAME_REGISTRY in GameSession.tsx

---

## Implementation Plan

### Phase 1: Code Cleanup (Immediate)

1. **Delete procedural-only game components:**
   ```bash
   rm src/components/{WordRescue,ShapeSequence,SlopeRider,NeuralPulse,ZenGravity,UpYours}.tsx
   ```

2. **Remove from GameSession.tsx:**
   - Remove lazy imports for 6 procedural games
   - Remove entries from GAME_REGISTRY
   - Remove entries from GAME_ICONS

3. **Delete DoubleFake component:**
   ```bash
   rm src/components/DoubleFake.tsx
   ```

4. **Archive analysis documents:**
   - Create `.github/docs/` directory
   - Move analysis files there
   - Delete build artifacts (`*.mjs` files)

### Phase 2: Database Cleanup (Optional)

Create a migration that:
- Marks legacy games as archived (don't delete, preserves historical data)
- Preserves existing user scores tied to these games

---

## Bundle Size Impact

**Estimated savings from deleting procedural games:**
- WordRescue: ~14.6 KB (gzip: 5.4 KB)
- UpYours: ~12.6 KB (gzip: 4.4 KB)
- Other 4 games: ~40 KB combined (gzip: ~16 KB)

**Total savings: ~67 KB (25.8 KB gzipped)**

This reduces the main bundle from ~215KB to ~190KB (uncompressed).

---

## Files to Remove

### Remove These Components
```
src/components/WordRescue.tsx
src/components/ShapeSequence.tsx
src/components/SlopeRider.tsx
src/components/NeuralPulse.tsx
src/components/ZenGravity.tsx
src/components/UpYours.tsx
src/components/DoubleFake.tsx
```

### Remove These Build Artifacts
```
vite.config.ts.timestamp-*.mjs
```

### Archive These Documentation Files
```
CODE_REVIEW_ANALYSIS.md
MATTER_JS_REVIEW.md
OPTIMIZATION_CHANGES.md
PHYSICS_ENGINE_AUDIT_SUMMARY.md
UNUSED_GAMES_ANALYSIS.md
```

---

## Retention Policy

**Keep in Repository:**
- All active games with DB entries (20 games)
- Real game content (puzzles, ranks, superlatives)
- Production configuration
- User authentication and scoring
- Leaderboard system

**Archive (`.github/docs/`):**
- Internal audit documents
- Performance analysis
- Code review notes

**Delete:**
- Build artifacts
- Procedural test games
- Inactive game components

---

## Post-Cleanup Checklist

- [ ] Update GAME_REGISTRY
- [ ] Update GAME_ICONS object
- [ ] Remove lazy imports in GameSession.tsx
- [ ] Run `npm run build` to verify no errors
- [ ] Test all active games in playlists
- [ ] Update README with active games list
- [ ] Create git commit: "chore: remove legacy/test games"
- [ ] Archive analysis documents to `.github/docs/`
- [ ] Test bundle size reduction

---

## Rollback Plan

All deleted files are preserved in git history. To restore:
```bash
git log --all --full-history -- "src/components/WordRescue.tsx"
git checkout <commit-hash> -- src/components/WordRescue.tsx
```

