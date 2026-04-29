# Unused Games Analysis

**Generated:** April 18, 2026

## Overview

This document compares the game implementations in the codebase against the Supabase database to identify games that are not currently used in any active playlists.

---

## Games in Supabase Database (22 Total)

### Actively Used in Playlists (20 games)

| Game ID | Slug | Name | Usage Count | Status |
|---------|------|------|------------|--------|
| 5 | ranky | Ranky | 27 | ✅ Active |
| 3 | odd-man-out | Odd Man Out | 24 | ✅ Active |
| 19 | superlative | Superlative | 22 | ✅ Active |
| 6 | Jigsaw-puzzle | Jigsaw | 21 | ✅ Active |
| 4 | What do you see? | Photo Mystery | 15 | ✅ Active |
| 7 | Split Decision | Split Decision | 13 | ✅ Active |
| 12 | snake | Snake | 7 | ✅ Active |
| 13 | hive-mind | Hive Mind | 4 | ✅ Active |
| 15 | fake-out | Fake Out | 3 | ✅ Active |
| 21 | multiple-choice | Multiple Choice | 3 | ✅ Active |
| 17 | color-clash | ColorClash | 3 | ✅ Active |
| 18 | recall | Recall | 3 | ✅ Active |
| 25 | slot | Slot | 2 | ✅ Active |
| 24 | flashbang | Flashbang | 2 | ✅ Active |
| 20 | true-false | True or False | 2 | ✅ Active |
| 23 | clutch | Clutch | 1 | ✅ Active |
| 27 | debris | Debris | 1 | ✅ Active |
| 22 | tracer | Tracer | 1 | ✅ Active |
| 26 | pivot | Pivot | 1 | ✅ Active |

### **NOT Used in Any Playlist (3 games)**

| Game ID | Slug | Name | Status | Notes |
|---------|------|------|--------|-------|
| 16 | double-fake | DoubleFake | ❌ Inactive | Marked as is_active=false |
| 1 | emoji-master | Emoji Master | ❌ Inactive | Marked as is_active=false, appears to be legacy |
| 9 | split-decision | Split Decision | ❌ Inactive | Duplicate/legacy version (ID 7 is the active one) |

---

## Games in Codebase (27 Total)

### Implemented Components

All games have corresponding React/TypeScript components in `src/components/`:

1. ✅ **OddManOut.tsx** (odd-man-out) - Database ID: 3
2. ✅ **PhotoMystery.jsx** (photo-mystery) - Database ID: 4
3. ✅ **RankAndRoll.tsx** (rank-and-roll) - Database ID: 5
4. ✅ **SnapShot.tsx** (snapshot) - Database ID: 6
5. ✅ **SplitDecision.tsx** (split-decision) - Database ID: 7
6. ✅ **WordRescue.tsx** (word-rescue) - No database entry
7. ✅ **ShapeSequence.tsx** (shape-sequence) - No database entry
8. ✅ **Snake.tsx** (snake) - Database ID: 12
9. ✅ **UpYours.tsx** (up-yours) - No database entry
10. ✅ **FakeOut.tsx** (fake-out) - Database ID: 15
11. ✅ **HiveMind.tsx** (hive-mind) - Database ID: 13
12. ✅ **SlopeRider.tsx** (slope-rider) - No database entry
13. ✅ **NeuralPulse.tsx** (neural-pulse) - No database entry
14. ✅ **ZenGravity.tsx** (zen-gravity) - No database entry
15. ✅ **Superlative.tsx** (superlative) - Database ID: 19
16. ✅ **TrueFalse.tsx** (true-false) - Database ID: 20
17. ✅ **MultipleChoice.tsx** (multiple-choice) - Database ID: 21
18. ✅ **Tracer.tsx** (tracer) - Database ID: 22
19. ✅ **Clutch.tsx** (clutch) - Database ID: 23
20. ✅ **Flashbang.tsx** (flashbang) - Database ID: 24
21. ✅ **ColorClash.tsx** (color-clash) - Database ID: 17
22. ✅ **Recall.tsx** (recall) - Database ID: 18
23. ✅ **Slot.tsx** (slot) - Database ID: 25
24. ✅ **Pivot.tsx** (pivot) - Database ID: 26
25. ✅ **Debris.tsx** (debris) - Database ID: 27
26. ✅ **DoubleFake.tsx** (double-fake) - Database ID: 16 (marked inactive)
27. ? **Other procedural games** - No database entries

---

## Games Without Database Entries

These games are implemented in the codebase but have no corresponding entry in the `games` table:

1. **WordRescue** (word-rescue) - Implemented component exists
2. **ShapeSequence** (shape-sequence) - Implemented component exists
3. **SlopeRider** (slope-rider) - Implemented component exists
4. **NeuralPulse** (neural-pulse) - Implemented component exists
5. **ZenGravity** (zen-gravity) - Implemented component exists
6. **UpYours** (up-yours) - Implemented component exists

These games are registered in the `GAME_REGISTRY` in `GameSession.tsx` but have no entries in Supabase's `games` table, meaning:
- They cannot be tracked in `round_results` or `user_progress`
- They won't appear in leaderboards
- User statistics for these games won't be recorded

---

## Unused Codebase Games

### **Codebase Implementation vs. Active Usage**

Games implemented in the codebase but **never used in any playlist**:

| Component | Slug | Database ID | Status | Reason |
|-----------|------|------------|--------|--------|
| DoubleFake.tsx | double-fake | 16 | Marked Inactive | Deliberately disabled |
| - | emoji-master | 1 | Marked Inactive | Legacy game, no component found |
| - | split-decision (legacy) | 9 | Marked Inactive | Duplicate of ID 7 |
| WordRescue.tsx | word-rescue | - | No DB entry | Procedural-only |
| ShapeSequence.tsx | shape-sequence | - | No DB entry | Procedural-only |
| SlopeRider.tsx | slope-rider | - | No DB entry | Procedural-only |
| NeuralPulse.tsx | neural-pulse | - | No DB entry | Procedural-only |
| ZenGravity.tsx | zen-gravity | - | No DB entry | Procedural-only |
| UpYours.tsx | up-yours | - | No DB entry | Procedural-only |

---

## Key Findings

### 1. **Unused in Playlists** (3 games)
- **DoubleFake** - Has DB entry (ID 16) but marked inactive and not used in any playlist
- **Emoji Master** - Legacy game (ID 1), marked inactive, no component in codebase
- **Split Decision Legacy** - Duplicate entry (ID 9), marked inactive, superseded by ID 7

### 2. **Procedural-Only Games** (6 games)
These games are fully implemented but have no database entries. They run locally and don't track scores or persist data:
- Word Rescue
- Shape Sequence
- Slope Rider
- Neural Pulse
- Zen Gravity
- Up Yours

### 3. **Registry vs. Database Mismatch**
The `GAME_REGISTRY` in `GameSession.tsx` contains 27 games, but:
- 22 games have database entries (in `games` table)
- 5 games are procedural-only
- This creates a disconnect between client-side game availability and server-side tracking

### 4. **Most Used Games**
1. Ranky (27 uses)
2. Odd Man Out (24 uses)
3. Superlative (22 uses)
4. Jigsaw (21 uses)
5. Photo Mystery (15 uses)

### 5. **Least Used Games** (But Still Active)
1. Pivot (1 use)
2. Tracer (1 use)
3. Debris (1 use)
4. Clutch (1 use)

---

## Recommendations

### Immediate Actions
1. **Resolve Double Fake** - Either enable it in playlists or remove from active code
2. **Clean up legacy entries** - Remove unused database entries (Emoji Master, legacy Split Decision)
3. **Add database entries for procedural games** - If they should be tracked, create corresponding database records

### Long-term
1. **Audit rarely-used games** - Games with 1-3 uses may not be popular; consider analysis
2. **Consider removing unused games** - DoubleFake and the 6 procedural-only games could be archived if not needed
3. **Standardize game registration** - Ensure all playable games have database entries for complete tracking

---

## Database Summary

**Active Games in Database:** 20 games
**Inactive Games:** 3 games (1 is legacy, 1 is duplicate, 1 is disabled)
**Codebase Components:** 27 games
**Games with Full Tracking:** 22 games
**Games without Tracking:** 5 games

