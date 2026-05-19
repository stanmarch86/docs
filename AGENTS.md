# Heimspiel Helpcenter — Agent Instructions

Dieses Repository ist das interne Helpcenter von **Heimspiel**, gebaut mit [Mintlify](https://mintlify.com).  
Alle Seiten sind MDX-Dateien mit YAML-Frontmatter. Die Navigation ist in `docs.json` definiert.

Für Mintlify-Komponentenwissen (Cards, Steps, Callouts etc.) empfiehlt sich:
```bash
npx skills add https://mintlify.com/docs
```

---

## Projektstruktur

```
docs/
├── docs.json                        # Navigation, Branding, Theme
├── support/                         # IT-Support Bereich
│   ├── index.mdx                    # Übersicht: Projekte, Monitoring, Kontakte
│   ├── hintergrundinfos.mdx         # DB-Struktur (HomeCourt, GlobalSports, Matching-DBs)
│   ├── bugs.mdx                     # Bekannte Bugs & Lösungswege
│   ├── staticmon.mdx                # Staticmon Recovery-Prozedur
│   ├── video-clipping.mdx           # ARD/ZDF Video-Clipping Workflow
│   ├── sportalch.mdx                # Sportal.ch Import
│   ├── kunden/                      # Kundenspezifische Integrations-Steckbriefe
│   │   ├── ringier.mdx              # Ringier / sport.ch (Kicker-API, sportalch)
│   │   └── msu-dpa.mdx              # MSU SAS / DPA (Ergebnisdienst, GFL)
│   └── runbooks/                    # Symptom-basierte Diagnose-Checklisten
│       ├── liveticker-einlauf.mdx   # Liveticker läuft nicht ein
│       └── fehlende-ergebnisse.mdx  # Ergebnisse fehlen / kein Endergebnis
└── ... (Mintlify-Standardseiten: essentials/, api-reference/, agent-ready/)
```

---

## Navigation (docs.json)

Die Navigation hat zwei Tabs:

| Tab | Inhalt |
|---|---|
| **IT Support** | Übersicht, Bugs & Lösungen, Workflows, Kunden-Steckbriefe, Runbooks |
| **Guides** | Mintlify-Standarddokumentation (Getting started, Writing content, API reference) |

---

## Inhaltsbereiche und ihre Logik

### `support/kunden/` — Kunden-Steckbriefe
Eine Seite pro Kunde. Enthält:
- Ansprechpartner + Kontakte
- Technische Schlüsselwerte (IDs, API-Keys, Syndicate-Namen, Job-IDs)
- Relevante Wettbewerbe
- Bekannte Eigenheiten und Einschränkungen
- Links zu Redmine-Referenztickets

**Wann anlegen:** Wenn ein Kunde wiederholt Support-Anfragen stellt oder eine komplexe Integration hat.

### `support/runbooks/` — Runbooks
Eine Seite pro Symptomtyp. Enthält:
- Symptombeschreibung (so wie der Kunde es meldet)
- Diagnose-Checkliste als `<Steps>` mit SQL-Queries und Feed-URLs
- Schnell-Fix-Rezept
- Verweis auf Kunden-Steckbrief + Redmine-Ticket

**Wann anlegen:** Nach jedem gelösten Support-Vorfall bei dem die Diagnose nicht offensichtlich war.

---

## Schlüsselkonzepte und Terminologie

| Begriff | Bedeutung |
|---|---|
| `match_meta` | Key-Value-Metadaten zu einem Spiel (match_id + provider_id + kind + content) |
| `tickertext_ringier` | match_meta-Kind zur Aktivierung des Kicker-API-Imports für Ringier |
| `match_date_end` | Endzeit-Markierung im Match-Datensatz — fehlt dieser, wird kein Endergebnis publiziert |
| `match_source` | Verknüpfung eines Spiels mit einer externen Datenquelle (z. B. source=55 für Kicker) |
| `syndicate` | Ziel-Kanal für Liveticker-Texte (z. B. `sportalch`, `heimspiel`, `apa`) |
| `live_status` | Datentiefe eines Spiels: `goals` = nur Tore, `full` = alle Events inkl. Karten/Wechsel |
| `provider_id=17` | HS-Tickerplanung |
| 17er DB | GlobalSports (GS) — Hauptdatenbank, alle Sportarten |
| 19er DB | global_sports_m — Matching GS-IDs ↔ externe Lieferanten-IDs |
| 23er DB | Staticmon — Job-Queue und Customer-Jobs |
| DSV-Import | Automatischer Datenimport aus dem DSV-System |
| Staticmon | Job-Verwaltungssystem für automatisierte Importe und Datenpipelines |

---

## Neue Seite hinzufügen

1. MDX-Datei im passenden Unterordner anlegen
2. Frontmatter setzen:
   ```mdx
   ---
   title: "Titel der Seite"
   description: "Kurze Beschreibung"
   icon: "icon-name"  # https://fontawesome.com/icons
   ---
   ```
3. Seite in `docs.json` unter der richtigen Gruppe eintragen
4. Commit erstellen

### Neuen Kunden anlegen
→ `support/kunden/{kundenname}.mdx` + Eintrag in `docs.json` unter "Kunden"

### Neues Runbook anlegen
→ `support/runbooks/{symptom}.mdx` + Eintrag in `docs.json` unter "Runbooks"

---

## Stil

- Sprache: **Deutsch** (dieses Helpcenter ist intern)
- Tabellen für strukturierte Daten (Kontakte, IDs, SQL-Parameter)
- `<Steps>` für Diagnose-Checklisten und Prozeduren
- `<Warning>` für häufige Fehler oder destruktive Aktionen
- `<Note>` für ergänzende Hinweise
- SQL-Queries immer mit Kommentar welche DB (`-- 17er DB`, `-- 19er DB`, `-- 23er DB`)
- Redmine-Tickets verlinken: `[#12345](https://redmine.endstand.de/issues/12345)`

---

## Lokale Vorschau

```bash
# In cmd (nicht PowerShell — Execution Policy blockiert npx.ps1)
cd C:\Heimspiel\docs
npx mintlify dev
```

Öffnet sich unter: http://localhost:3000
