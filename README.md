# Range Card

A one page range card for indirect fire. Paste your gun coordinates, paste your targets, read the distance to each one. Nothing else.

**[Open it here](ADD_YOUR_GITHUB_PAGES_LINK)**

## Why

Existing artillery calculators are crowded with maps, weapon tables, azimuth dials, mil charts and settings you never touch. Most of the time all you want is a number: how far is that target. This does that, and stops there.

One HTML file, no build step, no dependencies, no tracking, no backend. Open it from a folder or host it anywhere static.

## What it does

* Gun position on top, target list below, distance next to every target
* Your gun position is remembered between sessions, so you paste it once and only change targets
* Multiple targets at once, one per line, numbered
* Paste a block of several coordinates at once and it splits into rows
* Enter adds a new row, Backspace on an empty row removes it
* Change the gun position and every target is recalculated at once
* Works offline, everything is stored in your own browser

## Input formats

Paste whatever your game puts on the clipboard. The parser strips everything that is not a number and keeps the first two values, so all of these work:

```
x68.53, y104.12
10 20
x10 y10
X: 80.00 Y: 70.00
83.00/74.00
(68.53, 104.12)
-12.5, 7
x10.5 y20.3 z5.0     third value is ignored
```

Commas are handled both ways: `10,20` reads as two values (10 and 20), while `10,5 20,3` reads as 10.5 and 20.3.

## Units and scale

Distance is plain Euclidean distance between the two points, multiplied by a scale factor:

```
distance = scale * sqrt((x2 - x1)^2 + (y2 - y1)^2)
```

The scale is currently fixed at `100`, which matches WARDOGS, where one map coordinate unit equals 100 meters. If your game reports coordinates in meters already, set it to `1`. If it reports centimeters, set it to `0.01`.

To change it, edit the multiplier in `index.html`:

```js
var d = Math.round(Math.hypot(b[0] - a[0], b[1] - a[1]) * 100);
```

Sanity check for WARDOGS: a gun at `x80.00, y70.00` firing at `x83.00, y74.00` gives exactly 500 m.

## Run it

Download `index.html` and open it. That is the whole installation.

To host it, put the file at the root of a repository and turn on GitHub Pages in Settings, Pages, deploying from the main branch.

## Privacy

There is no server and no analytics. Your gun position and target list live in your browser's local storage on your own machine, and are never sent anywhere. Anyone you share the page with gets an empty card, not your coordinates.

## License

MIT
