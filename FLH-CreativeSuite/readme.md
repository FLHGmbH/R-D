# FLH Creative Suite

Interne Web-App der FLH GmbH. Mitarbeitende verwalten Kunden und erzeugen daraus Werbebilder: Produktbilder, Ad-Bilder (REWE / EDEKA) und Speisekarten. Ergebnisse bleiben dem jeweiligen Kunden zugeordnet.

Repo: [github.com/FLHGmbH/FLH-Creative-Suite](https://github.com/FLHGmbH/FLH-Creative-Suite)

## Was sie kann

- Kunden anlegen, suchen, CRM-Link hinterlegen
- Vorlagen: **Produktbild**, **Ad-Bild**, **Speise-Karten**
- Referenzbilder: Produkt, Szene, Mitarbeitende
- KI erzeugt die Bilder und speichert sie beim Kunden
- Übersicht mit letzten Kunden und letzten Generierungen

## Hosting

Alles Produktiv läuft unter dem Vercel-Team **Schulprojekt Maurice**. Datenbank und Login liegen in demselben Supabase-Projekt.

```text
Browser
   │
   ▼
Vercel  ── App + API (Bildverarbeitung)
https://vercel.com/schulprojekt-maurice
   │
   ├── React-Oberfläche
   └── /api  →  Vercel AI Gateway (Bilder)
   │
   ▼
Supabase  ── Datenbank + Auth + Dateien
https://supabase.com/dashboard/project/shcmzqbyfndgrhxeblin
Projekt: Schulprojekt Maurice
```

| Was | Wo |
| --- | --- |
| Oberfläche und API | [Vercel · Schulprojekt Maurice](https://vercel.com/schulprojekt-maurice) |
| Bild-KI | dieselbe Vercel-Instanz, serverseitig über das AI Gateway |
| Datenbank, Login, Bilddateien | [Supabase · Schulprojekt Maurice](https://supabase.com/dashboard/project/shcmzqbyfndgrhxeblin) |

Der AI-Schlüssel bleibt auf dem Server. Der Browser schickt nur den Auftrag an `/api`.

## Stack

| Schicht | Technik |
| --- | --- |
| Oberfläche | React 19, Vite |
| API | Vercel Functions in `api/` |
| Bildgenerierung | Vercel AI SDK → AI Gateway (`openai/gpt-image-2.5-sunburst`) |
| Daten | Supabase Postgres |
| Login | Supabase Auth |
| Dateien | Supabase Storage, Bucket `assets` (privat) |

Lokal: `npm run dev` startet Vite und einen Express-Server. Produktiv übernimmt Vercel die `api/`-Routen.

## Wo der Code liegt

```text
src/          Oberfläche (Seiten, Login, Generierung)
api/          Server-Routen (Kunden, Bilder, Ergebnisse, Session)
lib/          Server-Helfer, u. a. image-gateway.js
supabase/     SQL-Migrationen
public/       Badges, Posen, statische Assets
```

Wichtige Tabellen: `customers`, `employees`, `scenes`, `results`, `menu_card_sources`.

## Neuen User anlegen

Nur über das Supabase-Dashboard, Projekt **Schulprojekt Maurice**:

[Authentication](https://supabase.com/dashboard/project/shcmzqbyfndgrhxeblin/auth/users)

1. **Authentication → Users → Add user**
2. FLH-E-Mail eintragen
3. Ein Startpasswort setzen und den User anlegen
4. Beim neuen User **Passwort zurücksetzen** klicken

Die Person bekommt dann eine Mail und setzt sich selbst ein Passwort.
