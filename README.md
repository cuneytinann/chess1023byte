# chess1023byte

A two-player chess game in **1,023 bytes** of HTML + JavaScript. One file, no libraries, no build step, no server. Download `index.html`, double-click, play.

Part of the [Golfstack](https://www.fidelite.art/) project.

## Play

- [cuneytinann.github.io/chess1023byte](https://cuneytinann.github.io/chess1023byte/)
- [fidelite.art/special/L1/js1024.html](https://www.fidelite.art/special/L1/js1024.html) — same file, mirrored on the project site as the packed `L1` build

The name of the budget: 1,024 bytes. This lands 1 byte under it.

**Zoom in.** Cells are 22×24 px, which is tiny on a modern display. Use the browser's zoom — `Ctrl` `+`, or `⌘` `+` on macOS; around **300%** is comfortable. `Ctrl` `0` resets it. Nothing breaks on the way up: the cells are sized in HTML attributes and the pieces are text glyphs, so the whole board scales cleanly at any zoom level.

---

## What's in it

- **All piece movement**, geometry derived from arithmetic — no direction tables, no offset arrays.
- **Full legality.** A move that leaves your own king in check is never accepted. Every candidate is played on a cloned board and the king is queried.
- **Castling**, both sides, with every condition: rights still held, rook path clear, king not in check, king not crossing an attacked square.
- **En passant**, implemented as a *ghost*: the capture square is written into the board array itself as piece code `1`, which renders blank. There is no `e` state variable.
- **Promotion with a picker.** Queen, rook, bishop, knight; the move isn't completed until you choose.
- **Board flip.** After each move the board turns to the perspective of the side to move.
- **Game-over indicator.** When the side to move has no legal move, every square turns brown. That single test covers checkmate and stalemate alike, so no separate mate detection is written.

## What's not in it

No clock, no 50-move rule, no repetition counter, no insufficient-material test, no draw offers, no result codes, no bot. Mate and stalemate are not told apart — both simply end the game. For the full FIDE arbiter with all of that, see [fidelite.art](https://www.fidelite.art/).

---

## Files

| file | size | what |
| ---- | ---- | ---- |
| `index.html` | 1,023 B | the game, packed and playable |
| `chess.js` | 1,165 B | the plain source, one line, unpacked |

## Unpacking

`index.html` is self-extracting. The script is a RegPack decompression loop ending in `eval(_)`. To recover the plain source, replace that call:

```js
eval(_)   →   console.log(_)
```

The loop itself runs no game code, so this is safe to do in Node. `chess.js` in this repo is exactly what comes out.

## Packing

[RegPack 5.0.1](https://github.com/Siorki/RegPack). These settings reproduce `index.html` **byte for byte** from `chess.js`:

| option | value |
| ------ | ----- |
| `reassignVars` | `false` |
| `crushGainFactor` | `0` |
| `crushLengthFactor` | `0` |
| `crushCopiesFactor` | `0` |
| `crushTiebreakerFactor` | `0` |
| `withMath` | `false` |
| `wrapInSetInterval` | `false` |
| `useES6` | `true` |

Winning stage is 2, the regexp character class. Byte layout:

```
28 B  <center><table id=T><script>
986 B  packed payload
 9 B  </script>
----
1023 B
```

Only the script is packed; the HTML shell is not. Note that shortening the source does **not** reliably shorten the output — the packer pays for repeated substrings, so a longer source with more repetition often packs smaller. Several edits in this build are deliberately longer than they need to be for exactly that reason.

---

## Deprecated on purpose

The markup is legacy throughout, because legacy is shorter: `<center>`, `bgcolor`, `width` and `height` on `<td>`, unquoted attribute values, no closing `</tr>` or `</td>`, and no `<html>`, `<head>` or `<body>` at all.

There is no doctype either, which puts the page in **quirks mode** — deliberately. That is what keeps the presentational attributes rendering, and it is also why every cell carries its own `<center>`: in quirks mode a table does not inherit `text-align` from its ancestors, so the outer `<center>` alone will not centre the glyphs.

`id=T` is enough to reach the table from script; the browser exposes it as a global.

Piece glyphs are Unicode `U+2654`–`U+265F`. No image, no font download, no CDN request.

---

## More

The engine's design, the full rule coverage and a line-by-line walkthrough of the larger builds are at **[fidelite.art](https://www.fidelite.art/)**.
