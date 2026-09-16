**[English](#chess1023byte)** · **[Türkçe](#turkce)**

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

---
---

<a id="turkce"></a>

# chess1023byte (Türkçe)

HTML + JavaScript ile **1.023 bayt** içinde yazılmış iki kişilik bir satranç oyunu. Tek dosya, kütüphane yok, derleme adımı yok, sunucu yok. `index.html` dosyasını indirin, çift tıklayın, oynayın.

[Golfstack](https://www.fidelite.art/) projesinin bir parçasıdır.

## Oyna

- [cuneytinann.github.io/chess1023byte](https://cuneytinann.github.io/chess1023byte/)
- [fidelite.art/special/L1/js1024.html](https://www.fidelite.art/special/L1/js1024.html) — aynı dosya, proje sitesinde paketlenmiş `L1` sürümü olarak yansıtılmış hâli

Hedeflenen sınır 1.024 bayt; bu sürüm onun 1 bayt altında kalıyor.

**Yakınlaştırın.** Kareler 22×24 piksel; modern bir ekranda çok küçük kalıyor. Tarayıcının yakınlaştırmasını kullanın — `Ctrl` `+`, macOS'ta `⌘` `+`; **%300** civarı rahattır. `Ctrl` `0` sıfırlar. Büyütürken hiçbir şey bozulmaz: kare boyutları HTML özniteliklerinde tanımlı, taşlar da metin karakterleri olduğu için tahta her yakınlaştırma düzeyinde temiz ölçeklenir.

---

## İçinde neler var

- **Tüm taş hareketleri**, geometrisi aritmetikten türetilmiş — yön tablosu yok, ofset dizisi yok.
- **Tam yasallık kontrolü.** Kendi şahınızı şah altında bırakan bir hamle asla kabul edilmez. Her aday hamle kopyalanmış bir tahtada oynanır ve şahın durumu sorgulanır.
- **Rok**, iki yöne de, tüm koşullarıyla: rok hakkı hâlâ duruyor, kale yolu açık, şah şah altında değil, şah saldırı altındaki bir kareden geçmiyor.
- **Geçerken alma (en passant)**, bir *hayalet* olarak uygulanmıştır: alınacak kare, tahta dizisinin içine `1` taş koduyla yazılır ve boş görünür. Ayrı bir `e` durum değişkeni yoktur.
- **Seçicili terfi.** Vezir, kale, fil, at; siz seçim yapana kadar hamle tamamlanmaz.
- **Tahta çevirme.** Her hamleden sonra tahta, sırası gelen tarafın bakış açısına döner.
- **Oyun sonu göstergesi.** Sırası gelen tarafın yasal hamlesi kalmadığında tüm kareler kahverengiye döner. Bu tek test hem şah matı hem de pat durumunu kapsar; bu yüzden ayrı bir mat tespiti yazılmamıştır.

## İçinde neler yok

Saat yok, 50 hamle kuralı yok, tekrar sayacı yok, yetersiz materyal testi yok, beraberlik teklifi yok, sonuç kodları yok, bot yok. Mat ile pat birbirinden ayırt edilmez — ikisi de oyunu bitirir, o kadar. Tüm bunları içeren eksiksiz FIDE hakemi için [fidelite.art](https://www.fidelite.art/) adresine bakın.

---

## Dosyalar

| dosya | boyut | ne |
| ----- | ----- | -- |
| `index.html` | 1.023 B | oyun, paketlenmiş ve oynanabilir |
| `chess.js` | 1.165 B | paketlenmemiş kaynak kod, tek satır |

## Paketi açma

`index.html` kendi kendini açan bir dosyadır. Betik, `eval(_)` ile biten bir RegPack açma döngüsüdür. Paketlenmemiş kaynak kodu elde etmek için bu çağrıyı değiştirin:

```js
eval(_)   →   console.log(_)
```

Döngünün kendisi hiçbir oyun kodu çalıştırmaz, bu yüzden bunu Node'da yapmak güvenlidir. Bu depodaki `chess.js` tam olarak bu işlemin çıktısıdır.

## Paketleme

[RegPack 5.0.1](https://github.com/Siorki/RegPack). Aşağıdaki ayarlar `chess.js` dosyasından `index.html` dosyasını **bayt bayt aynı** şekilde yeniden üretir:

| seçenek | değer |
| ------- | ----- |
| `reassignVars` | `false` |
| `crushGainFactor` | `0` |
| `crushLengthFactor` | `0` |
| `crushCopiesFactor` | `0` |
| `crushTiebreakerFactor` | `0` |
| `withMath` | `false` |
| `wrapInSetInterval` | `false` |
| `useES6` | `true` |

Kazanan aşama 2, yani regexp karakter sınıfı aşamasıdır. Bayt düzeni:

```
28 B  <center><table id=T><script>
986 B  paketlenmiş yük
 9 B  </script>
----
1023 B
```

Yalnızca betik paketlenir; HTML kabuğu paketlenmez. Şunu not edin: kaynağı kısaltmak çıktıyı **güvenilir biçimde kısaltmaz** — paketleyici kazancını tekrar eden alt dizelerden elde eder, bu yüzden daha çok tekrar içeren daha uzun bir kaynak çoğu zaman daha küçük paketlenir. Bu sürümdeki bazı düzenlemeler tam da bu nedenle, gerekenden bilerek daha uzun tutulmuştur.

---

## Bilinçli olarak eski usul

İşaretleme baştan sona eski usul yazılmıştır, çünkü eski usul daha kısadır: `<center>`, `bgcolor`, `<td>` üzerinde `width` ve `height`, tırnaksız öznitelik değerleri, kapanış `</tr>` veya `</td>` yok, `<html>`, `<head>` ya da `<body>` hiç yok.

Doctype da yok; bu da sayfayı **quirks mode**'a sokar — bilerek. Görünümle ilgili eski özniteliklerin hâlâ işlemesini sağlayan budur; her karenin kendi `<center>` etiketini taşımasının nedeni de budur: quirks mode'da bir tablo `text-align` değerini üst öğelerinden devralmaz, bu yüzden dıştaki `<center>` tek başına taşları ortalamaya yetmez.

Tabloya betikten ulaşmak için `id=T` yeterlidir; tarayıcı onu global bir değişken olarak sunar.

Taş karakterleri Unicode `U+2654`–`U+265F` aralığındadır. Görsel yok, yazı tipi indirme yok, CDN isteği yok.

---

## Daha fazlası

Motorun tasarımı, kuralların eksiksiz kapsamı ve daha büyük sürümlerin satır satır açıklaması **[fidelite.art](https://www.fidelite.art/)** adresinde.
