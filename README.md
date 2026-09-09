# Mein Hub – echte Online-Version

Diese Version benutzt **GitHub Pages + Supabase**:

- echte Registrierung/Login über Supabase Auth
- echte PostgreSQL-Datenbank
- Daten sind nicht mehr an einen einzelnen Browser gebunden
- To-Do und K-Pop sind für eingeloggte Nutzer gemeinsam
- Kalender bleiben pro Nutzer getrennt und können über Einladungen geteilt werden
- Dev-Rechte werden serverseitig über Row Level Security geschützt

GitHub Pages ist weiterhin nur das Hosting für HTML/CSS/JS. Die eigentliche Authentifizierung und Datenbank laufen bei Supabase. GitHub Pages selbst ist ein statischer Hostingdienst. 

## 1. Supabase-Projekt erstellen

Erstelle ein Projekt bei https://supabase.com/.

Danach:

**SQL Editor → New query → Inhalt von `supabase.sql` einfügen → Run**

Das SQL erstellt Tabellen, Trigger und Row-Level-Security.

## 2. Deinen Dev-Account erstellen

1. Starte die Website nach Schritt 3.
2. Registriere dich mit deiner E-Mail und deinem gewünschten Benutzernamen.
3. Öffne in Supabase den SQL Editor.
4. Führe aus:

```sql
update public.profiles
set role = 'dev',
    tabs = '{"todo":true,"kpop":true,"calendar":true}'::jsonb
where lower(username) = lower('DEIN_BENUTZERNAME');
```

Ersetze `DEIN_BENUTZERNAME`.

Danach auf der Website abmelden und wieder anmelden.

## 3. Supabase-Schlüssel in `script.js`

Öffne `script.js` ganz oben:

```js
const SUPABASE_URL = "DEINE_SUPABASE_URL";
const SUPABASE_PUBLISHABLE_KEY = "DEIN_SUPABASE_PUBLISHABLE_KEY";
```

Trage deine Werte aus:

**Supabase → Project Settings → API**

ein.

### Wichtig

Im Browser darf nur der **Publishable/Anon-Key** verwendet werden.

**NIEMALS** den `service_role` Key in `script.js`, GitHub oder eine andere öffentliche Datei kopieren. Supabase weist ausdrücklich darauf hin, dass der `service_role` Key nicht im Browser exponiert werden darf.

## 4. E-Mail-Bestätigung

Supabase kann bei der Registrierung eine Bestätigungs-E-Mail verlangen.

Für Tests kannst du in Supabase unter:

**Authentication → Providers → Email**

die E-Mail-Bestätigung konfigurieren.

Für eine echte öffentliche Seite solltest du die Bestätigung eingeschaltet lassen.

## 5. Auf GitHub veröffentlichen

Lade diese Dateien in dein Repository:

- `index.html`
- `style.css`
- `script.js`
- `supabase.sql`
- `README.md`

Dann:

**Repository → Settings → Pages → Deploy from a branch → main → / (root) → Save**

GitHub Pages veröffentlicht statische Dateien aus deinem Repository.

## Was jetzt online geteilt wird

### Gemeinsam
- To-Do-Liste
- K-Pop-Liste

### Persönlich
- eigener Login
- eigene Kalendertermine

### Kalender teilen
Ein Nutzer gibt Benutzername + Nutzer-ID ein. Der andere Nutzer bekommt eine Einladung und kann sie annehmen/ablehnen.

## Hinweis zur bisherigen Version

Die alte Version hatte Benutzer, Passwörter und Daten in `localStorage`. Das war nur Browser-Speicher und keine echte Online-Datenbank.

Die neue Version speichert Passwörter nicht selbst. Supabase Auth übernimmt Login/Passwortverwaltung.

## Sicherheit

Die Datenbank verwendet Row Level Security (RLS). Dadurch werden Schreibrechte nicht nur über die Oberfläche versteckt, sondern zusätzlich von Supabase auf Datenbankebene kontrolliert.

Trotzdem gilt: Veröffentliche niemals den Supabase `service_role` Key.
