# 📚 Vokabeltest – Business in Context 3 · Topic 6

Ein interaktiver Vokabeltest zum Wortschatz „Transport and the environment" (105 Vokabeln aus dem Schulbuch). Jede:r Schüler:in bekommt **genau 24 Vokabeln**, zufällig gezogen und gemischt aus allen – die Auswahl ist also bei jedem Durchlauf anders.

Die App ist eine **einzelne HTML-Datei** (`vokabeltest.html`) – kein Build, keine Installation. Läuft in jedem Browser und direkt über GitHub Pages.

## ✨ Funktionen

- **Namenseingabe** vor dem Start (Pflichtfeld)
- **24 Vokabeln pro Test**, zufällig gemischt aus allen 105
- **Vier Übungstypen**, bunt gemischt und in beide Richtungen (EN→DE und DE→EN):
  - **Multiple Choice** – Wort anzeigen, vier Übersetzungen zur Auswahl
  - **Übersetzen (Tippen)** – Übersetzung selbst eingeben
  - **Lückentext** – das fehlende englische Wort in einem Beispielsatz ergänzen
  - **Wort schreiben** – deutsches Wort zeigen, englisches Wort tippen (mit Anfangsbuchstaben-Tipp)
- **1 Punkt pro richtiger Antwort**, am Ende Punktzahl, Prozent und Liste der Fehler zum Nachlernen
- **Tolerante Prüfung** – Groß-/Kleinschreibung und Umlaute egal; mehrere richtige Lösungen (Synonyme) werden akzeptiert; „to" bei Verben optional
- **Lehrer-Ansicht** (passwortgeschützt) mit **allen** Ergebnissen (Name, Punkte, Prozent, Datum/Uhrzeit), sortierbar und als **CSV/Excel exportierbar**

> Ohne Einrichtung läuft alles sofort – die Ergebnisse werden dann aber **nur lokal auf dem jeweiligen Gerät** gespeichert. Damit du als Lehrkraft die Ergebnisse **aller** Schüler:innen siehst, einmalig Supabase einrichten (siehe unten).

## 🚀 Lokal starten

`vokabeltest.html` einfach im Browser öffnen.

## 🌐 Auf GitHub Pages veröffentlichen

Wie beim GEO-Quiz: Datei ins Repository hochladen und GitHub Pages aktivieren (Settings → Pages → Deploy from a branch → `main` / root). Erreichbar dann unter:
`https://DEIN-BENUTZERNAME.github.io/REPO-NAME/vokabeltest.html`

## 🔑 Lehrer-Ansicht & Passwort

Unten auf der Startseite gibt es den Link **„🔒 Lehrer-Ansicht"**. Das Passwort legst du oben in `vokabeltest.html` fest:

```js
const TEACHER_PASSWORD = "lehrer2026";   // <- bitte ändern
```

> Hinweis: Dieses Passwort schützt nur die Anzeige in der App und ist keine echte Datensicherheit – es hält neugierige Schüler:innen ab, mehr nicht. Für eine reine Lern-/Testanwendung in der Klasse ist das in Ordnung.

## 📊 Ergebnisse aller Schüler:innen sammeln (Supabase, kostenlos)

Damit alle Ergebnisse – geräteübergreifend – bei dir landen, brauchst du denselben kostenlosen Speicher wie beim GEO-Quiz. Falls du Supabase schon eingerichtet hast, kannst du **dasselbe Projekt** verwenden und musst nur die Ergebnis-Tabelle anlegen.

### 1. Supabase-Projekt
Konto auf [supabase.com](https://supabase.com) anlegen und ein Projekt erstellen (Region **Central EU**). Falls schon vorhanden: vorhandenes Projekt nutzen.

### 2. Tabelle für die Ergebnisse anlegen
Links **SQL Editor → New query**, einfügen und **Run**:

```sql
create table vocab_results (
  id bigint generated always as identity primary key,
  name text not null,
  points int not null,
  total int not null,
  percent int not null,
  created_at timestamptz default now()
);

alter table vocab_results enable row level security;

create policy "eintragen erlaubt" on vocab_results
  for insert to anon with check (true);

create policy "lesen erlaubt" on vocab_results
  for select to anon using (true);
```

### 3. Zugangsdaten eintragen
**Project Settings → API**: **Project URL** und **anon public** key kopieren und oben in `vokabeltest.html` einsetzen:

```js
const SUPABASE_URL = "https://abcdxyz.supabase.co";
const SUPABASE_KEY = "eyJhbGciOiJI...DEIN_LANGER_KEY";
```

Speichern, Datei zu GitHub hochladen – fertig. Ab jetzt landet jedes abgeschlossene Testergebnis automatisch in der Tabelle, und du siehst in der **Lehrer-Ansicht** alle Namen samt Punkten (oder direkt im Supabase **Table Editor → vocab_results**).

### Ergebnisse exportieren
In der Lehrer-Ansicht auf **„⬇ CSV/Excel"** klicken – die Datei lässt sich direkt in Excel öffnen (Spalten: Name, Punkte, von, Prozent, Datum/Uhrzeit).

## ✏️ Vokabeln ändern

Alle Vokabeln stehen oben im `<script>` in der Liste `VOCAB` im Format
`{"en": "...", "de": "...", "ex": "Beispielsatz"}`.
Der Beispielsatz (`ex`) wird für die Lückentexte verwendet; fehlt er oder kommt das Wort darin nicht vor, nutzt die App für diese Vokabel automatisch einen anderen Übungstyp.

Anzahl der Vokabeln pro Test ändern: oben `const NUM_QUESTIONS = 24;`.

---

*Lernhilfe für den Unterricht. Wortschatz aus dem Schulbuch (Veritas, Business in Context 3, Topic 6).*
