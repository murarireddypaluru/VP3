# VP3 — Virtual Pet (Time-Based State Machine)

The most advanced **Virtual Pet** stage, built with **p5.js** / **p5.play** and a **Firebase
Realtime Database**. On top of feeding (Feed / Add Food buttons and a `Food` class that draws
milk-carton icons), the pet runs a **time-based state machine**: based on how many hours have
passed since the dog was last fed (`FeedTime`), it moves through **hungry → playing →
sleeping → bathing** and swaps the room background (garden / bedroom / washroom) to match.
The current `gameState` is stored in Firebase so it stays in sync across clients.

## State machine

Using `currentTime = hour()` and the stored `lastFed` hour:

| Hours since fed | State      | Room / background |
| --------------- | ---------- | ----------------- |
| exactly +1      | `playing`  | Garden            |
| exactly +2      | `sleeping` | Bedroom           |
| +3 to +4        | `bathing`  | Washroom          |
| otherwise       | `hungry`   | default view      |

## Libraries

- [p5.js](https://p5js.org/) — canvas, DOM buttons, input
- [p5.play](https://molleindustria.github.io/p5.play/) — the dog sprite (`p5.play.js` bundled)
- [Firebase](https://firebase.google.com/) (App + Realtime Database) — state & food persistence

## Controls

- **Feed The Dog** button — feed the dog (decrements stock, records feed time, dog turns happy)
- **Add Food** button — add one unit to the food stock

The pet's playing/sleeping/bathing states change automatically with the clock; there are no
movement keys.

## How to run

This game needs a **Firebase Realtime Database of your own** — the committed config only
contains placeholders.

1. Create a project at <https://console.firebase.google.com> and enable **Realtime Database**.
2. Open `index.html` and replace the placeholders in `firebaseConfig`
   (`YOUR_FIREBASE_API_KEY`, `https://YOUR_PROJECT.firebaseio.com`, etc.) with your own
   project's values.
3. Serve the folder over a local web server (e.g. `python3 -m http.server`) and open it in a
   browser.

## Notes on fixes in this build

- **Bathing state made reachable.** The bathing branch previously used
  `currentTime == lastFed+2 && currentTime == lastFed+4`, which can never be true (a single
  hour cannot equal two different values), so the `bathing` state was dead code. It now uses a
  proper hour-range check, `currentTime >= lastFed+3 && currentTime <= lastFed+4`.
- **Image paths normalized.** Asset references mixed `Images/` and `images/` casing and
  pointed at names that don't exist. They now use the repo's actual image folder,
  `virtual pet images/` (e.g. `Dog.png`, `happy dog.png`, `milk.png`), so assets load on
  case-sensitive hosts.

## Security

The original Firebase credentials were removed and replaced with placeholders. Please read
[SECURITY-NOTE.md](SECURITY-NOTE.md) before running — and remember to lock down your
Realtime Database security rules.
