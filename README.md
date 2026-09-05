# Thomas wird 68 — Countdown

Eine Countdown-Seite auf den **30. Oktober 2026, 00:00 Uhr** (Berliner Zeit).
Wenn der Countdown abläuft, verwandelt sich die Seite von selbst in eine
Gratulation mit Konfetti — man muss nichts anklicken.

**Online:** https://primevestconsulting.github.io/countdown-t68/
**Losungswort für den Familienbereich: `Rita`**

Widmung, Fotos und die Geschenk-Karte sind verschlüsselt. Ohne das Wort steht
in `index.html` nur unlesbares Zeichenwirrwarr — auch für jeden, der das
GitHub-Repository durchsucht.

---

## Seite ansehen

Doppelklick auf `index.html`. Das genügt; auch die Entschlüsselung funktioniert
so, ohne Internet.

Die Geschenk-Karte erscheint normalerweise erst am Geburtstag. Zum Schreiben
und Anschauen vorher:

```
index.html?preview=1
```

Also die Datei öffnen und `?preview=1` hinten an die Adresszeile hängen.

---

## Etwas ändern

Alles Änderbare liegt im Ordner `inhalte/`:

| Was | Wo |
|---|---|
| Geschenktext | `inhalte/geschenk.txt` |
| Widmung | `inhalte/widmung.txt` |
| Fotos | `inhalte/bilder/` |
| Bildunterschriften | `inhalte/bilder/texte.txt` |
| Losungswort | Zeile `export PASSWORT=` in `bauen.sh` |

Danach **immer** neu bauen:

```bash
./bauen.sh
```

Das verkleinert die Bilder, entfernt Aufnahmeort und Kameradaten (EXIF/GPS),
verschlüsselt alles neu und schreibt `index.html`.

### Fotos tauschen oder ergänzen

Bilder einfach in `inhalte/bilder/` legen, löschen oder ersetzen — JPG oder PNG.

* Die **Reihenfolge** ergibt sich aus dem Dateinamen: `01-…`, `02-…`, `03-…`
* Das Bild mit dem Präfix **`00-`** wird das runde Portrait ganz oben
* Hoch- und Querformat erkennt das Skript selbst; Querformate bekommen im
  Raster die doppelte Breite
* Unterschriften optional in `inhalte/bilder/texte.txt`, je Zeile:
  `dateiname | Text`

Dann `./bauen.sh` — das Raster ordnet sich neu, egal ob 3 oder 12 Bilder.

---

## Online stellen

```bash
./bauen.sh
git add index.html robots.txt README.md
git commit -m "Aktualisiert"
git push
```

Nach ein bis zwei Minuten ist die neue Fassung unter
https://primevestconsulting.github.io/countdown-t68/ sichtbar.
Falls der Browser noch die alte Fassung zeigt: einmal hart neu laden
(Cmd+Shift+R).

**Es geht nur `index.html` ins Repo.** Die Ordner `original/` und `inhalte/`
mit den unverschlüsselten Fotos und Texten bleiben durch `.gitignore` auf
diesem Rechner.

---

## Wie der Schutz funktioniert

* Fotos und Texte werden mit **AES-256-CBC** verschlüsselt und als Base64 in
  `index.html` eingebettet
* Der Schlüssel entsteht im Browser aus dem Losungswort
  (**PBKDF2-SHA256, 250 000 Runden**, zufälliges Salt) — über die im Browser
  eingebaute WebCrypto-Schnittstelle, ohne fremde Bibliotheken
* Ein einmal eingegebenes Wort merkt sich der Browser bis zum Schließen des
  Tabs, damit man nicht ständig tippen muss

Das ist kein Banktresor — wer das Wort hat, hat die Bilder. Es hält die Fotos
aber zuverlässig aus Suchmaschinen und von Zufallsbesuchern fern. Verschick den
Link und das Wort am besten getrennt.

---

## Ordner

```
index.html      die fertige Seite — nur diese Datei geht online
vorlage.html    Gestaltung und Programmlogik (Grundlage für index.html)
bauen.sh        erzeugt index.html
inhalte/        Texte und Fotos im Klartext — bleibt lokal
original/       die ursprünglich gelieferten Bilddateien — bleibt lokal
```
