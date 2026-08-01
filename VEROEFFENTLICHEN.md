# FushaCards für Android — Download-Seite veröffentlichen

Dieser Ordner (`android-site/`) ist die komplette öffentliche Download-Seite.
Sie kommt in ein **eigenes, neues** öffentliches GitHub-Repository und läuft
dort über GitHub Pages. Einmal einrichten — danach besteht jedes Update nur
noch aus: APK ersetzen, `latest.json` anpassen, committen.

---

## ⚠️ WARNUNG — ZUERST LESEN

> **NIEMALS das private Haupt-Repository (`Arabisch-App`) forken, kopieren
> oder öffentlich stellen.**
>
> Im Haupt-Repo liegt der **Android-Signatur-Keystore samt Passwort**. Wer
> beides hat, kann signierte Updates unter dem Namen der App verbreiten —
> auf die Geräte aller Nutzer. Ein Fork oder eine Kopie würde außerdem die
> **gesamte Git-Historie** mitveröffentlichen; einmal geleakte Schlüssel
> lassen sich nicht zurückholen.
>
> Deshalb: Ein **frisches, leeres** Repository anlegen und **nur die Dateien
> aus diesem Ordner** hineinlegen. Nichts anderes.

---

## Was in das öffentliche Repo gehört

```
index.html            die Download-Seite
latest.json           die Versions-Wahrheit (liest auch der Update-Check in der App)
icon-1024.png         das App-Icon
fonts/                alle woff2-Dateien (8 Stück)
downloads/
  FushaCards.apk      die signierte APK (kommt von dir dazu)
```

Sonst nichts. Kein `web/`, kein `app/`, keine Keystore-Dateien.

---

## Einmalige Einrichtung

### 1. Neues öffentliches Repository anlegen

Auf github.com → **New repository**:

- Name: z. B. `fushacards-android` (der Name ist egal, siehe Hinweis unten)
- Sichtbarkeit: **Public**
- **Nicht** „Fork" — ein leeres, neues Repo
- Ohne README/„Add .gitignore" starten ist am einfachsten

### 2. Dateien hineinlegen

Im Terminal auf dem Mac (Ordner anpassen, falls das neue Repo anders heißt).
Vorher einmal `git pull`, damit `android-site/` lokal da ist:

```bash
cd "/Users/praxishorn/KI Systeme/Iphone APP/FushaCards-2.0"
git pull origin claude/i18n-en-es
```

Dann das neue Repo daneben anlegen und befüllen:

```bash
cd "/Users/praxishorn/KI Systeme"
git clone https://github.com/DEIN-GITHUB-NAME/fushacards-android.git
cd fushacards-android
cp -R "/Users/praxishorn/KI Systeme/Iphone APP/FushaCards-2.0/android-site/." .
rm -f VEROEFFENTLICHEN.md downloads/.gitkeep
```

`VEROEFFENTLICHEN.md` ist nur eine Anleitung für dich — sie muss nicht mit
veröffentlicht werden (kann aber, sie enthält nichts Geheimes).

### 3. APK dazulegen

Die fertig signierte APK (aus dem Android-Build) als
**`downloads/FushaCards.apk`** in das neue Repo kopieren — exakt dieser
Dateiname, denn `latest.json` zeigt darauf:

```bash
cp "/pfad/zur/gebauten/FushaCards.apk" downloads/FushaCards.apk
```

APK-Größen um 20–30 MB sind für GitHub unproblematisch (Grenze: 100 MB
pro Datei).

### 4. Committen und pushen

```bash
git add -A
git commit -m "FushaCards Android Download-Seite"
git push origin main
```

### 5. GitHub Pages aktivieren

Im neuen Repo auf github.com:

**Settings → Pages → Build and deployment**
- Source: **Deploy from a branch**
- Branch: **main**, Ordner **/ (root)**
- **Save**

Nach 1–2 Minuten ist die Seite erreichbar unter:

```
https://DEIN-GITHUB-NAME.github.io/fushacards-android/
```

Diese URL ist der Link, den du weitergibst (WhatsApp usw.).

---

## Update-Ablauf für jede neue Version

Nur drei Handgriffe, sonst nichts:

1. **APK ersetzen** — die neue signierte APK wieder als
   `downloads/FushaCards.apk` (gleicher Name, alte überschreiben).
2. **`latest.json` anpassen** — alle Felder aktualisieren:
   - `versionName`: neue Versionsnummer, z. B. `"2.3.0"`
   - `versionCode`: **immer um 1 erhöhen** (der Update-Check in der App
     vergleicht diese Zahl — bleibt sie gleich, merkt niemand das Update)
   - `sizeMb`: Größe der neuen APK in MB (aufgerundet reicht)
   - `date`: Erscheinungstag als `JJJJ-MM-TT`
   - `notes`: Was-ist-neu-Text in **allen drei Sprachen** (`de`, `en`, `es`)
3. **Committen und pushen:**

```bash
cd "/Users/praxishorn/KI Systeme/fushacards-android"
git add -A
git commit -m "Version 2.3.0"
git push origin main
```

GitHub Pages veröffentlicht automatisch neu (1–2 Minuten). Die Seite zeigt
Besuchern, die schon einmal da waren, von selbst „Neue Version seit deinem
letzten Besuch" — dafür muss nichts weiter getan werden.

---

## Hinweise

- **`apkUrl` bleibt relativ** (`"./downloads/FushaCards.apk"`). Dadurch ist
  der Download-Link stabil und funktioniert unter **jedem** Repo-Namen und
  jeder Domain — auch wenn das Repo später umbenannt wird oder eine eigene
  Domain davor kommt. Nicht auf eine absolute URL ändern.
- **`latest.json` wird doppelt gelesen**: von der Download-Seite **und** vom
  Update-Check in der Android-App. Format nicht verändern, keine Felder
  umbenennen, `versionCode` nie zurücksetzen.
- Die Seite lädt `latest.json` mit `cache: no-store`, ein frisches Update
  erscheint also sofort — höchstens die GitHub-Pages-Verteilung selbst
  braucht ein paar Minuten.
- Fonts und Icon liegen mit Absicht **lokal im Repo** (keine CDNs): Die
  Seite funktioniert dadurch eigenständig, ohne Zugriff auf das Haupt-Repo
  und ohne Drittanbieter.
