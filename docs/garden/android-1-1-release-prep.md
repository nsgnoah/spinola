# Android 1.1 release prep

## Choice
Item 1 of "Next" in `docs/STATUS.md`: Android players are a release behind iOS. The live Play build
(1.0, versionCode 1, built September 25) has 36-question decks and no question history. Both
features are already in `android/`, but nobody has prepared a release, and the Play listing still
promises 648 questions.

## Why this one
- Of the items on the roadmap, this one does the most for players right now. Android players get
  3.3x the questions and fewer repeats with no new code.
- It can be finished in one run without a Mac. The other items need real devices (Game Center),
  App Store Connect (notification copy), or new verified trivia plus Swift and Kotlin changes that
  can't be compiled on this Linux runner (stats, new decks).

## What changes
- `android/app/build.gradle.kts`: versionCode 1 → 2, versionName 1.0 → 1.1 (matches iOS 1.1).
- `android/play/listing/en-US/changelogs/2.txt`: release notes, word for word the iOS 1.1 What's New.
- `android/play/listing/en-US/full_description.txt`: 2,160 questions, 120 per deck, plus one
  sentence about the per-phone history. Checked against `MatchEngine.questions` (unseen first within
  each tier, then the ones shown longest ago).
- `docs/STATUS.md`, `docs/PLAY-STORE.md`: record the prepared release and add the changelog step to
  the release checklist.

## Not done here
Building, testing, signing and uploading. These need the Mac with the upload key.
