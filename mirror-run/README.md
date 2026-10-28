# Mirror Run

A one-thumb split-screen runner. The screen is split into a light **day** side and a dark **night** side, with a runner on each. You steer both with one thumb: the day runner follows your finger and the night runner moves as its reflection. Each side has different walls, so you have to find the one spot that gets both of them through.

## How to play

- **Drag anywhere** to steer. It works like a trackpad, so your thumb never covers the runners. Arrow keys work on desktop.
- The night runner **mirrors** the day runner. When an amber line crosses the screen, the game switches to **parallel** mode and both runners move the same way until the next line flips it back. The divider turns amber while parallel mode is on.
- Speed goes up over time, safe gaps get narrower, and the walls get closer together.
- Grab gold **sparks** for bonus points. Squeezing past a wall with very little room to spare counts as a **close call** and pays extra.

## Progression

- Missions such as "Survive 2 mirror flips" or "5 close calls in one run" earn stars.
- Spend 1 star on **New mirror** to clear the walls ahead and keep a run going (once per run).
- The **Daily** mode gives everyone the same walls for the day.
- Your best distance, stars and missions are saved in the browser with `localStorage`.

## Running it

It's a single self-contained file with no build step. Open `index.html` in a browser, or serve the folder:

```sh
npx serve .
```

The only external files it loads are two Google Fonts (Archivo and Martian Mono).

## Wrapping it as an app

The game already handles touch input, safe-area insets and haptics through `navigator.vibrate`, so it can be packaged with Capacitor:

```sh
npm init -y
npm i @capacitor/core @capacitor/cli
npx cap init "Mirror Run" com.example.mirrorrun --web-dir .
npx cap add android   # or ios
npx cap copy
```

For a real release, move `index.html` into a `www/` folder and point `--web-dir` at it so Capacitor doesn't pick up the project files.

## Tuning

The main knobs are near the top of the script and in `update()`:

| What | Where |
| --- | --- |
| Runner size, wall thickness | `R`, `BH` |
| Speed ramp | `G.speed = Math.min(1.25, .5 + G.t*.011)` |
| Gap width over time | `safeWidth()` |
| Wall spacing over time | `spacing()` |
| When mirror flips start, how often | `spawn()` (after 12 walls, then every 10) |
