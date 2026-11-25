# Hidden Gem Badge, Simplified PRD (StreamLine)

**Author:** Me · **Status:** Draft · **Target:** High-Fidelity Prototype · **Persona:** Priya, a streaming viewer who enjoys exploring content but struggles to decide what to watch from a large library. Her goal is to find something genuinely interesting without spending too much time browsing.

## 1. The Big Picture
- **Vision:** Priya should never leave StreamLine thinking there's nothing worth watching. The Hidden Gem Badge turns the quality buried in our library into a visible, trusted signal, so great content stops being invisible just because it isn't popular. Less scrolling, more confidence.
- **Press release:** StreamLine Spotlight now flags the great titles nobody's talking about. The new Hidden Gem Badge appears on highly rated titles that few viewers have found yet, giving explorers a quick, trustworthy reason to press play. There are no extra taps or filters, just a clear signal right where you're already deciding what to watch.
- **Success metric:** Spotlight selection-to-playback conversion for badged titles versus the holdout group (target: [X]% lift, to be set once baseline is measured).
- **Guardrail:** Sessions staying within StreamLine (no increase in drop-off), and no decline in playback completion for badged titles, which would signal a badge that over-promises.

## 2. The Details
### User stories
- As Priya, I want to see which Spotlight titles are hidden gems at a glance, so that I can pick something good without browsing for long.
- As Priya, I want to start playing a badged title in one tap, so that the badge helps me decide instead of adding a step.
- As Priya, I want badges to appear only on a few titles, so that I can trust the signal.
- As a PM, I want to see selections and playbacks for badged versus unbadged titles, so that I can prove the badge lifts conversion.
- As a PM, I want to switch the badge off for a holdout view, so that I can compare against a no-badge experience.
### Screens to build
- Entry point: StreamLine Home. A simple home with a "Spotlight" tile or row. Tapping it opens screen 2.
- Feature core: Spotlight list. A grid of about 10 mock title cards. About 2 carry the Hidden Gem badge (icon + text), and the rest are plain. This screen also holds the holdout toggle and the small event counter.
- Success: Now Playing. A mock player screen confirming the selected title is playing, with a back link to Spotlight.
### Functional requirements
- A card shows the badge only if quality score ≥ 4.0 and view count < 10,000. Both are hardcoded values in the mock data.
- No more than 20% of mock titles are badged (2 of 10).
- 100% of flagged cards show the badge, with an icon and the text "Hidden Gem," visible without hover or tap. 0% of unflagged cards show it.
- Tapping any card, badged or not, reaches the Now Playing screen in exactly 1 tap, with no modal or interstitial.
- Entry to playback takes ≤ 3 taps (Home → Spotlight → card).
- Three events are logged to an on-screen counter: badge impression (once per badged card on render), title selected, and playback started. Each tags whether the title was badged.
- A "Holdout" toggle hides all badges instantly (0 visible) without changing card order, layout, or content.
- The counter shows selection and playback counts split by badged and unbadged titles.
### Smart behaviors (Situation → Outcome)
- If a title meets both quality and view-count thresholds, then it renders with the Hidden Gem badge.
- If a title fails either threshold, then it renders as a plain card.
- If Priya taps a badged card, then Now Playing opens immediately and a "selected" and "playback started" event are logged for a badged title.
- If Priya taps an unbadged card, then Now Playing opens immediately and the events are logged as unbadged.
- If the Holdout toggle is on, then no badges render and impressions are not logged.
- If the Holdout toggle is turned off, then badges reappear on flagged cards only.
- If Priya taps Back from Now Playing, then she returns to the Spotlight list with her state intact.
### Technical constraints
- Single-page React app (or plain HTML + JS) with useState only.
- Mock titles are a hardcoded array of about 10 objects: id, title, genre, quality, views, and a derived isGem.
- Navigation is a simple screen state value (home / spotlight / playing).
- Event log is an in-memory array held in state, shown in a small panel.
- Works in a mobile-width viewport, with text and icon on the badge so it's not color-only.

## 3. The Logistics
### Features out
- User ratings or "mark as gem"- New data collection with no validated need.
- Notifications, email, sharing- Breaks the stay-in-app guardrail (A6–A8).
### Edge cases & safety guard
- Unhappy paths and expected behavior
- Quality score or view count is missing: the title renders plain (fail closed). A missing signal never produces a badge.
- Exactly at a threshold (quality 4.0, views 10,000): the rule is quality ≥ 4.0 and views < 10,000, so 4.0 / 9,999 badges and 4.0 / 10,000 does not. Test both boundaries.
- Zero titles qualify: the grid renders normally with no badges, and no empty-state error.
- Too many titles qualify (over the 20% cap): badge only the top-scoring titles up to the cap, so the signal stays scarce.
- Rapid double-tap on a card: one navigation and one logged selection, not two.
- Holdout toggled mid-session: badges disappear or reappear instantly, with no reorder and no lost state.
- Back from Now Playing: Spotlight returns with the same toggle state and counter values.
- Long titles or a missing thumbnail: the badge never overlaps or truncates the title, and the card still renders with a placeholder.
### Decision log
- One surface, zero badge interactions. The badge is passive and appears only on the Spotlight grid. I gave up the tap-for-explanation tooltip, which would build trust but add an interaction and a state to test. Priya can still succeed without it, and it's the most tempting creep item since it sits half-way to A2.
- A hardcoded rule, not a scoring system. The gem flag comes from two mock fields and one threshold rule. I gave up realistic data and tunable scoring. The prototype exists to test whether the badge changes behavior, not whether we can compute quality, and a rule this simple makes every badge decision explainable and testable.
### Evals
- Accuracy: badge correctness
- Time-on-task
- Safety

## MoSCoW scope
- **Must:** A1 · Spotlight Curated Rail; A3 · Hidden Gem Badge
- **Should:** A2 · Why You'll Love This Label; A5 · Personalized Spotlight Queue
- **Could:** A4 · Mood-Based Entry Point
- **Won't (now):** A6 · Spotlight Digest Email; A7 · Curator Profiles

---
**Builder hook:** Build a working prototype based on this PRD. Use the User Story as the core flow, Functional Requirements as build constraints, and prioritize speed and clarity over visual complexity.
