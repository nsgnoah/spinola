# Spinola — His World vs Her World

A trivia game for two people who share a couch and not a knowledge base.

Each of you declares the world you know. His World: football, ball sports, cars, grilling and whiskey, games and superheroes, tools and the outdoors, action movies and dad TV, tech and gadgets, fight night. Her World: skincare, fashion, rom-coms, reality TV, pop divas, wedding season, book club, wellness, celebrity gossip. Every round you pick the deck your partner gets quizzed on. They pick yours. Seven questions, fifteen seconds each, difficulty climbing from Rookie to Legend over five rounds. First to three crowns wins, then you run it back.

Play from two phones with Game Center turn-based matches (Apple ID only, no accounts of ours, no server), or pass one phone back and forth.

Listed as **Spinola: Couples Trivia** on both stores (subtitle / short description "His World vs Her World"); the home-screen name is just **Spinola**. Ola is the host who talks to you in the game. iOS 17+, universal iPhone and iPad; Android 8.0+ (pass-and-play only).

## Store status

| | Version | Status |
|---|---|---|
| App Store | 1.1 (build 5) | Live since September 28, 2026. 1.0 was approved September 27. Free, Games > Trivia, rated 13+. Apple ID 6814338074, bundle ID `co.nsgsolutions.spinola`. |
| Google Play | 1.0 (versionCode 1) | Live since October 2, 2026 at [play.google.com/store/apps/details?id=co.nsgsolutions.spinola](https://play.google.com/store/apps/details?id=co.nsgsolutions.spinola). Rated Teen, 176 countries and regions (all but Morocco), published by the NSG LLC organization account. |

The Play release was built and submitted on September 25, before the 120-question decks and the per-phone question history landed in the repo; the Android `versionCode` is still 1, so those changes have not been uploaded to Play yet. Full records: `docs/APP-STORE.md` and `docs/PLAY-STORE.md`.

## What it does

**Getting set up**
- **Profile** — your first name and the world you know. Stored on the phone only.
- **Home** — two match lists: *Online · Two phones* (Game Center) and *Pass & play · One phone*. The menu has *Privacy & support* (the privacy policy in-app, plus a link to the hosted copy) and *Start over*, which clears the profile and pass-and-play matches on that device; Game Center matches are untouched.

**Two modes**
- **Me vs my partner** (`couple`) — you each declare the world you know and get quizzed on the *other* one. Online it starts from *Challenge your partner*.
- **Our couple vs theirs** (`teams`) — two couples, four people. Each member has a lane, the world they answer (by default the world they know; it can be swapped), and the other couple picks a deck per member. A side's round score is its two members' scores added together. Online it is one phone per couple (*Challenge another couple*; the challenged phone names its two people before joining); on one phone it is all four.

**A round**
- **Pick** — on your turn you choose the deck for each opposing member from a wheel of their world's nine decks. Spin for a suggestion, or tap the slice you want.
- **Intro** — round number and label (*Warm-up*, *Getting real*, *Pro level*, *Legend territory*, *Last word*), the deck, and the top tier in play.
- **Questions** — seven multiple-choice questions, four options each, fifteen seconds on a timer ring. After each answer Ola gives the verdict, the right answer if you missed it, and, for some questions, a short fact.
- **Reveal** — once both sides have answered a round, each side sees the scores, per-member breakdowns in couples mode, and who took the crown.
- **Match over** — final crowns and total points, confetti for the winner, and *Rematch* for pass-and-play.

**Difficulty** climbs over the five rounds by mixing tiers (1 Rookie, 2 Pro, 3 Legend):

| Round | Tiers of the seven questions |
|---|---|
| 1 | 1 1 1 1 1 2 2 |
| 2 | 1 1 2 2 2 2 3 |
| 3 | 2 2 2 2 3 3 3 |
| 4 | 2 2 3 3 3 3 3 |
| 5 | 3 3 3 3 3 3 3 |

**Scoring**
- A correct answer is worth 100, plus a speed bonus of up to 50 that shrinks linearly over the fifteen seconds, plus 25 for each consecutive correct answer after the first in a streak (capped at +75). Wrong answers and timeouts score 0 and break the streak.
- The side with the higher round score takes the round's crown; a tied round gives no crown.
- First to three crowns wins. If five rounds finish without that, more crowns wins, then more total points; identical totals are a tie.

**Pass-and-play** runs every screen on one phone. Between turns a hand-off screen names who should hold the phone next ("No peeking. Their questions are next."), and opening a pass-and-play match always shows it first so the wrong person never sees the questions.

**Look and sound** — a game-show register: world-coloured sunburst backgrounds with a halftone band and paper grain, cream chunky panels, 3D buttons, a mascot per deck with moods, the spin wheel, a timer ring, confetti. Every icon is drawn from paths (no SF Symbols). Rockwell and DIN Condensed, both built into iOS. Sounds are synthesized at launch; there are no audio files. Dynamic Type (capped at xxLarge) with a 14pt reading floor, 44pt tap targets, and Reduce Motion respected by every looping animation and the confetti.

## Layout

- `OlaBall/` — the iOS app (the folder, target and module keep the old name; nothing user-visible does).
  - `App/OlaBallApp.swift` — entry point; seeds fixed matches for the UI tests when launched with `-ui-testing-seed`.
  - `Models/` — `Deck.swift` (`World`, `Deck`, `Question`, the `Q()` authoring helper, `stableHash`), `MatchState.swift` (the match JSON: modes, sides, members, rounds, crowns), `Profile.swift`.
  - `Engine/` — `MatchEngine.swift` (pure rules) and `MatchController.swift` (one match's UI state machine and the `MatchTransport` protocol).
  - `Content/` — the 18 decks: `HerWorld.swift`, `HerWorldMore.swift`, `HisWorld.swift`, `HisWorldMore.swift`, plus `Extra/<Deck>Extra.swift` for the questions appended later.
  - `Services/` — `GameCenterService.swift` (+ `GameCenterTransport` and the matchmaker), `LocalMatchStore.swift` (+ `PassAndPlayTransport`), `QuestionHistory.swift`.
  - `Views/` — one file per screen (`HomeView`, `ProfileSetupView`, `TeamSetup`, `MatchView`, `DeckPickView`, `QuestionView`, `RoundRevealView`, `MatchOverView`, `AboutView`) plus the design system: `Theme.swift` (palette, type scale, `Viewport`/`UI.scale`), `HUD.swift` (`Stage`, `GameBackground`, panels, buttons, mascots, `Wordmark`), `Wheel.swift`, `Icons.swift`.
  - `Audio/SoundKit.swift` — the synthesized sound cues. `Support/SeededRNG.swift` — SplitMix64.
  - `PrivacyInfo.xcprivacy`, `OlaBall.entitlements` (Game Center), `Info.plist`, `Resources/Assets.xcassets`.
- `OlaBallTests/` — Swift Testing: `EngineTests.swift` (content structure, draws, scoring, the match flow in both modes, Game Center's first turn), `CrossPlatformExportTests.swift` (writes `content/`), `AppIconTests.swift` (renders the 1024 icon and fails if it is flat or has alpha), `IconSheetTests.swift` (a contact sheet of every drawn icon), `WaitingScreenTests.swift` (renders the online-only waiting screen).
- `OlaBallUITests/PlayThroughUITests.swift` — plays a full pass-and-play match and a full couples match, checks the new-match sheet's placeholders and that privacy and support are reachable; writes screenshots to `build/shots` (override with `TEST_RUNNER_OLABALL_SHOTS=<dir>`).
- `android/` — the Kotlin + Jetpack Compose port. See [Android](#android).
- `content/` — generated by the iOS tests, read by Android: `assets/decks.json` and `golden/*.json`. Never edited by hand.
- `docs/` — `APP-STORE.md` and `PLAY-STORE.md` (submission records and checklists), `STATUS.md` (handoff notes), `SUBMISSION-AUDIT.md` (a pre-submission audit and the response to it), `privacy-policy.md`, `legend-sources.json` (a source URL for each of the Legend questions added on September 21).
- `scripts/` — screenshot and export helpers (see [Scripts](#scripts)).
- `site/` — the public support page (`index.html`) and privacy policy (`privacy.html`).
- `project.yml` — [XcodeGen](https://github.com/yonaskolb/XcodeGen) spec. Edit it and run `xcodegen generate`; never hand-edit the `.xcodeproj`.
- `CLAUDE.md` — the project's working rules: identity, design system, layout and accessibility rules, architecture, and the Android conventions.

## Building

```bash
brew install xcodegen
xcodegen generate
open OlaBall.xcodeproj
```

Tests: `⌘U`, or `xcodebuild test -project OlaBall.xcodeproj -scheme OlaBall -destination 'platform=iOS Simulator,name=iPhone 16 Pro'`. Run the UI tests against an iPad destination and the iPhone SE (3rd generation) to check the largest and shortest layouts.

Online play needs a Game Center account signed in on the simulator or device (Settings > Game Center); pass-and-play covers every other screen.

## How the rules work

All rules live in plain Swift with no UI imports, so they can be tested directly and ported line for line.

- `Engine/MatchEngine.swift` — the constants (15 s, 100 / 50 / 25 points), the tier mix per round, round labels, scoring, and the question draw.
- `Models/MatchState.swift` — everything two phones must agree on: mode, seed, the two sides (`MatchPlayer`, each with one or two `Member`s), rounds with picks and results keyed by member id, status, winner, and which rounds each side has seen revealed. It derives crowns, totals, round winners, what each side still has to answer (`pendingAnswers`) and pick (`pendingPicks`), and settles the match (`evaluate`).
- `Engine/MatchController.swift` — drives one match on screen through stages (`setupTeam`, `handoff`, `intro`, `answering`, `picking`, `roundReveal`, `waiting`, `finished`). `route()` is the single source of what comes next. It talks to a `MatchTransport` (`submitTurn`, `save`), which is either Game Center or the pass-and-play store. A failed submit leaves the turn on the phone with a *Try again*.

**The question draw is deterministic.** Each round's seven questions come from a SplitMix64 generator seeded by the match seed, the round number and the member id. Within each tier, questions this phone has never shown come first, then the ones it showed longest ago (`QuestionHistory`, kept in UserDefaults); questions already used earlier in the match are excluded. If a tier runs dry the draw falls back to a neighbouring tier, and as a last resort repeats rather than showing a short round. Only the answering phone draws, and it writes the question ids into the state, so phones with different histories cannot disagree. Answer order is shuffled per question id with the same generator, so it is identical on every device.

## Questions and decks

18 decks, nine per world, 120 questions each (40 Rookie, 40 Pro, 40 Legend): 2,160 in all.

| Her World | His World |
|---|---|
| Skincare & Beauty | Football 101 |
| Fashion & Style | Ball Sports Grab Bag |
| Rom-Coms & Love Stories | Cars & Engines |
| Reality TV | Grilling, Beer & Whiskey |
| Pop Divas & Their Eras | Games & Superheroes |
| Wedding Season | Tools, Gear & Outdoors |
| Book Club | Action Movies & Dad TV |
| Wellness & Self-Care | Tech & Gadgets |
| Celebrity Couples & Gossip | Fight Night |

Questions are authored in Swift, in `OlaBall/Content/`, one line each:

```swift
Q(tier, "Prompt?", "Correct answer", ["Wrong", "Wrong", "Wrong"], "Optional fact Ola reads after the answer.")
```

Each deck's first 36 questions sit inline in `HerWorld.swift` / `HisWorld.swift` (and the `*More.swift` files); the other 84 are appended from `Content/Extra/<Deck>Extra.swift`. Question ids are positional (`<deck>-<n>`), so **only ever append**: inserting or deleting renumbers every later question. Every question is meant to be a checkable public fact; `ContentTests` checks every deck for 18 decks, 120 questions, 40 per tier, three distinct wrong answers and a correct answer that survives the shuffle.

**Export to Android.** `OlaBallTests/CrossPlatformExportTests.swift` writes every deck, with each question's shuffled options and correct index, to `content/assets/decks.json`. The Android app ships that file as an asset and loads it at launch, so both apps play the same questions from one source. If the file on disk differs from what the Swift code produces, the test rewrites it and fails once; commit the new file and the next run is green.

## Game Center

`Services/GameCenterService.swift`. There is no server of ours: Apple stores each match's data and sends the turn notifications.

- **Sign-in** uses the Apple ID already signed in to Game Center. If it is not, the app says how to sign in under Settings rather than showing GameKit's error.
- **New match** — the system turn-based matchmaker, exactly two players, with an invite line per mode ("Think you know my world? Prove it." / "Think your couple knows more than ours? Prove it.").
- **Match data** is the `MatchState` JSON. Ending a turn encodes it and passes the turn to the other participant (`endTurn`); a save mid-turn uses `saveCurrentTurn`. When the state says the match is finished, each participant's outcome is set to won, lost or tied and the match is ended.
- **The creator's first move.** A brand-new match holds only its creator, so there is nothing to pick or answer yet; the controller submits the seed state straight away so the opponent can join and make the first pick.
- **Turn events** (`GKLocalPlayerListener`) reload the match list and open the match that became active.
- **Removing** a finished match removes this player's copy only; the other player keeps theirs.

`docs/STATUS.md` (September 25) still listed a full Game Center match between two real devices as not yet verified; the simulator cannot exercise these paths, so the unit tests cover the controller logic instead.

## Android

A native Kotlin + Jetpack Compose port lives in `android/` (pass-and-play on one phone; there is no
Game Center on Android, and Google retired Play Games' turn-based multiplayer in 2020). Both modes work on one phone. It ships the same questions from `content/assets/decks.json`, which the iOS
tests export, and proves the same rules with golden fixtures.

- `android/core` — pure Kotlin, no Android imports, tested on the JVM: `SeededRNG`, the models, `MatchEngine`, `MatchController` (a `StateFlow` instead of `@Observable`), the JSON wire format, and the stores.
- `android/app` — the Compose app: `ui/theme`, `ui/hud` (the same design system, icons drawn in `Icons.kt`), `ui/screens` (one file per iOS view), `audio` (the same synthesized sounds; haptics without a permission). Same package prefix `co.nsgsolutions.spinola`, same strings, Ola's lines word for word. Rockwell and DIN Condensed are stood in by bundled Arvo and Barlow Condensed Bold (SIL OFL).
- `android/play` — Play Store icon, feature graphic, screenshots and listing copy.
- No permissions, no accounts, no server, no third-party SDKs (AndroidX and kotlinx only). Target API 36, minimum API 26.

```bash
export JAVA_HOME=/opt/homebrew/opt/openjdk@17/libexec/openjdk.jdk/Contents/Home ANDROID_HOME=$HOME/Library/Android/sdk
cd android && ./gradlew :core:test :app:testDebugUnitTest :app:assembleDebug
```

Release bundles (`./gradlew :app:bundleRelease`) are signed with an upload key that is gitignored and lives only on the build Mac. Build notes in `android/README.md`, the Google Play checklist in `docs/PLAY-STORE.md`.

### Keeping iOS and Android in sync

iOS is the source of truth. `OlaBallTests/CrossPlatformExportTests.swift` writes:

| File | What Kotlin checks against it |
|---|---|
| `content/assets/decks.json` | The questions themselves, and each question's shuffled options |
| `content/golden/engine.json` | SplitMix64 streams, `stableHash`, bounded draws, shuffles, question draws (including the seen-history order), points and round scores |
| `content/golden/match-state.json` | One `MatchState` exactly as Swift's `JSONEncoder` writes it: the wire format |
| `content/golden/match-state-facts.json` | The crowns, totals, status, winner, pending picks and answers, and used questions Kotlin must derive from that state |

The `android/core` tests replay every fixture, so a rule or content change on one platform fails the other's tests until both agree. After editing questions or rules:

```bash
xcodebuild test -project OlaBall.xcodeproj -scheme OlaBall -destination 'platform=iOS Simulator,name=iPhone 16 Pro' -only-testing:OlaBallTests/CrossPlatformExportTests
# commit the rewritten content/ files, then
cd android && ./gradlew :core:test
```

Keep `MatchState`'s JSON stable: it is the contract between the two apps. `content/README.md` has the details.

## Scripts

- `scripts/store_shots.sh` — plays the pass-and-play UI test on a simulator and saves App Store screenshots to `build/shots-store/`. No argument uses an iPhone 16 Pro Max (1320x2868), `ipad` an iPad Pro 13-inch (2064x2752), or pass any simulator UDID. App Store Connect's iPhone slot wants 6.5-inch shots (1284x2778), which come from passing the UDID of an iPhone 14 Plus or 13 Pro Max simulator.
- `scripts/ExportOptions.plist` — `xcodebuild -exportArchive` options that upload straight to App Store Connect. `scripts/ExportOptions-local.plist` — the same, but writes an `.ipa` to `build/export` instead. The archive and export commands are in `docs/APP-STORE.md`.
- `android/scripts/store_shots.sh` — boots the `spinola_phone` emulator (or a named AVD), runs the instrumented play-through and pulls Play Store screenshots.
- `android/play/render.sh` — regenerates the Play Store PNGs from their sources (`icon_vectors.py` draws the vectors from the Swift geometry, `pngtool.py` handles PNG conversion).

## Privacy and support

- No accounts, passwords, or network of our own. Game Center handles identity and match storage. No analytics, no third-party SDKs.
- The profile, pass-and-play matches and question history stay on the device in UserDefaults.
- The site in `site/` is published by GitHub Pages from the public `nsgnoah/ola` repo (this repo stays the source; copy both files over and push to change them): support at [nsgnoah.github.io/ola](https://nsgnoah.github.io/ola/), privacy policy at [nsgnoah.github.io/ola/privacy.html](https://nsgnoah.github.io/ola/privacy.html). The policy is also inside the app under *Privacy & support*.

## App Store readiness

- Privacy manifest declares no tracking and no collected data; UserDefaults use is declared.
- No permissions requested. Portrait on iPhone, all orientations on iPad (universal build). Encryption export flag set. 1024×1024 icon with no alpha.
- Game Center entitlement in `OlaBall/OlaBall.entitlements`. Dynamic Type, Reduce Motion, and 44pt targets throughout.
- iPad is not a stretched phone app: `Viewport` measures the window and scales type, buttons, panels and art against the 393x852 phone design, and every screen is a `Stage` with a full-bleed background behind a centred play column.
- For each upload, bump `CURRENT_PROJECT_VERSION` in `project.yml` (and `MARKETING_VERSION` for a new version).
- The full submission checklist (App Store Connect setup, age rating, review notes, screenshots via `scripts/store_shots.sh`, archive and upload commands) is in `docs/APP-STORE.md`.

## History

The first version of this repo was a 3D football coaching game ("Draw the Play"); it is preserved at the tag `v1-football-draw-the-play`.
