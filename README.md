# World Cup 2026 Predictor ⚽🏆

**Live:** https://taomri.github.io/mondial-2026/

Enter match scores and the whole 2026 FIFA World Cup builds itself. Group tables, the third-place ranking and the full knockout bracket all update automatically, down to the champion.

The 2026 tournament is the first with 48 teams, so predicting it by hand is hard. The 8 best third-placed teams (out of 12 groups) go through, and where each one lands in the Round of 32 depends on *which* groups they come from. This app works all of that out for you using FIFA's official rules.

## Features

- **Group stage**: all 12 groups and 72 matches, with live standings using FIFA tie-breakers
- **Schedule**: all 104 matches by date, with host city
- **Third places**: ranks the 12 third-placed teams and highlights the 8 that qualify
- **Knockout bracket**: from the Round of 32 to the final. Pairings fill in automatically, and a drawn match opens penalty inputs
- **Multiple predictions**: create, duplicate, rename and delete separate predictions, then switch between them
- **AI prediction**: one click adds a ready-made prediction for all 104 matches (a fixed set based on Opta forecasts, betting markets and FIFA rankings as of June 2026)
- **Community**: publish a prediction under your name, browse everyone else's in real time and open any of them read-only. You can delete your own posts
- **Export / import**: save all your predictions to a JSON file and load them on another device

## How it works

**Group standings.** Wins are worth 3 points and draws 1. Teams are ranked by points, then goal difference, then goals scored. Teams that are still level are re-ranked on a mini-table of only the matches between them (head-to-head), and the draw order breaks any tie that remains.

**Third-place slotting (FIFA Annex C).** Once every group is complete, the letters of the 8 qualifying groups (e.g. `ABCDEFGH`) form the key to a 495-entry lookup table. The table says which third-placed team each group winner (1A, 1B, 1D, 1E, 1G, 1I, 1K, 1L) plays in the Round of 32.

**Bracket resolution.** Every knockout slot is a short code: `1A` / `2B` (a group position), `3` (an Annex C slot), or `W89` / `L101` (the winner or loser of an earlier match). Slots resolve recursively, so the entire bracket is described by 32 lines of data.

Fair-play points and FIFA-ranking tie-breakers are not calculated.

## Tech stack

- Plain HTML, CSS and JavaScript in one file, with no framework and no build step
- Firebase Firestore for published predictions, with a realtime listener
- Firebase Anonymous Auth so only the author can delete a post
- `localStorage` for private predictions
- GitHub Pages for hosting

## Project structure

```
index.html                  The whole app (markup, styles, data and logic)
firebase-config.js          Firebase web config (public by design)
firebase-config.example.js  Template for your own Firebase project
firestore.rules             Firestore security rules
```

## Running locally

Open `index.html` in a browser. Your predictions are saved in your browser's `localStorage`. The Community tab needs an internet connection and a configured Firebase project.

## Firebase setup (for your own copy)

1. Create a project in the [Firebase Console](https://console.firebase.google.com).
2. **Build → Firestore Database → Create database.**
3. **Project settings → Your apps → Web**, then copy the config into `firebase-config.js` (see `firebase-config.example.js`).
4. **Firestore → Rules**: paste the contents of `firestore.rules` and click **Publish**.
5. **Authentication → Sign-in method → Anonymous → Enable.**
6. Deploy (for example with GitHub Pages).

The security rules let anyone read posts and check each new post's shape: name 1–40 characters, title 1–80 characters, at most 104 matches, and a server-side timestamp. Only signed-in users can create posts, only the original author can delete one, and posts can't be edited.
