# ⚔️ DM Companion

**A campaign workspace for Dungeon Masters, in a single HTML file.**
Sessions, characters, quests, locations, maps, fights and notes in one place. No server, no account, no install, and it works offline.

![Dashboard](docs/screenshots/dashboard.png)

---

## Features

- **Campaign management:** sessions with summaries and player journals, NPCs and PCs, locations with sub-locations, quests with priorities and progress, factions, lore, handouts, magic items, encounters, quick notes and a DM reference screen.
- **Character stats & conditions:** AC, HP, ability scores, saves, skills, spells, equipment, the 14 SRD conditions, exhaustion, concentration, inspiration and death saves. Includes a party overview and a one-click long rest.
- **Combat tracker:** initiative, rounds, damage/healing/temp HP and conditions with durations. It also shows concentration DCs, handles death saves and massive damage, and keeps a combat log. PC/NPC HP and conditions are written back to the character.
- **Offline bestiary:** all 325 monsters of the D&D 5e SRD 5.1 with full stat blocks. Click attacks, damage and abilities to roll dice. Optionally search Open5e online for more monsters.
- **Adventure log:** a running record of what the party did, grouped by session, with automatic entries for fights, quests, character changes and rests. Exports to Markdown.
- **Maps:** a world map with location pins, plus PDF or image maps per location with general notes and room/area notes. Area notes can jump to a PDF page.
- **Calendar:** Faerûn, Greyhawk, Eberron, Exandria, Shire, Roshar or a custom calendar, with campaign, historical and mythological events and DM-only visibility.
- **Relationship map, global search, tags** and Markdown in almost every text field.
- **Import / export / sync:** JSON backups, automatic sync to a file in iCloud, Dropbox or Google Drive (Chrome/Edge), and merge-import of partial updates.
- **AI-friendly data format:** a documented JSON format, so tools or AI agents can turn a PDF or session notes into importable data.
- **Demo campaign:** one click loads a fully filled example campaign.

| Combat Tracker | Bestiary |
|---|---|
| ![Combat tracker](docs/screenshots/combat-tracker.png) | ![Bestiary](docs/screenshots/bestiary.png) |
| **Party overview** | **Adventure Log** |
| ![Party](docs/screenshots/party.png) | ![Adventure log](docs/screenshots/adventure-log.png) |
| **Location map with notes** | |
| ![Map notes](docs/screenshots/map-notes.png) | |

---

## Quick Start

1. Download `dm-companion.html`.
2. Open it in a browser. Chrome or Edge is recommended; Firefox and Safari work except for automatic file sync.
3. Click **🎲 Load Demo Campaign** to explore everything, or **+ New Campaign** to start your own.

There's nothing to install or build.

### Hosting on GitHub Pages (optional)

Rename `dm-companion.html` to `index.html` (or add an `index.html` that links to it) and enable **Settings → Pages** for the repository. Each visitor's data stays in their own browser. Data saved while using the local file is **not** visible on the hosted version, and vice versa; use Export/Import to move it.

---

## Where Data Is Stored

Everything stays **in your browser on your computer**:

| Data | Storage |
|---|---|
| Campaign data (sessions, characters, quests, fights, log, calendar …) | `localStorage`, key `dmtool_v2` (about 5 MB limit) |
| Location map files (PDF / images) | IndexedDB `dmtool_files` |
| SRD monsters and condition rules | Built into the HTML file |

- Another browser, a private window or another computer starts empty.
- Clearing browsing data deletes your campaigns. **Make backups** with 💾 Export JSON, or connect a sync file.
- Data only leaves your computer if you use the **AI features** (sent to Anthropic, with your own API key) or the **online Open5e search** (sends only the search text).

---

## Backup, Sync and Merging

- **Export / Import:** 💾 Export JSON creates a complete backup, including map files unless disabled. Import offers three options:
  - **Merge into current campaign**: adds new entries and updates existing ones with the same name. Nothing is deleted.
  - **Add as new campaign(s)**
  - **Replace everything**: restores a full backup.
- **Cloud Sync (Chrome/Edge):** Settings → Cloud Sync → connect a JSON file in a synced folder. Every change is written to it automatically.
- **Session updates:** import a small JSON containing only what changed in a session (new NPCs, HP, quest status, log entries) with **Merge**. Then export, or let sync update the full campaign file.

---

## Creating Data with Other Tools or AI Agents

[`DM_COMPANION_DATA_FORMAT.md`](DM_COMPANION_DATA_FORMAT.md) describes the import format completely. It covers every field, allowed values, references by name, merge rules and a complete example, and ends with a ready-made prompt.

Give an AI agent that document plus your material (for example a PDF with all characters). It can then produce a JSON file you import with **Merge**. Monsters can be referenced by SRD name (`"statblockRef": "Bandit Captain"`) and get their full stat block on import.

---

## Documentation

| File | Content |
|---|---|
| [`DM_COMPANION_MANUAL.md`](DM_COMPANION_MANUAL.md) | User manual: every feature, combat rules, storage, sync, import |
| [`DM_COMPANION_DATA_FORMAT.md`](DM_COMPANION_DATA_FORMAT.md) | JSON import/backup format for tools and AI agents |

## Repository Layout

```
dm-companion.html              the complete app (HTML, CSS, JS and SRD data in one file)
DM_COMPANION_MANUAL.md         user manual
DM_COMPANION_DATA_FORMAT.md    data format specification
docs/screenshots/              images used in this README
README.md
```

## Browser Support

| Browser | Status |
|---|---|
| Chrome, Edge | Full support, including automatic file sync |
| Firefox, Safari | Everything except automatic file sync (use Export/Import or auto-export) |
| Mobile browsers | Usable; the layout adapts to small screens |

---

## Credits & License

- **DM Companion** was created by **Gregor Vogt**: concept, campaign workflow and design direction, built with AI assistance. Homepage: <https://www.gregorvogt.com/dnd-companion/>
- **Version:** 0.9.0
- **Monster stat blocks and condition rules:** *System Reference Document 5.1* © Wizards of the Coast LLC, licensed under [CC-BY-4.0](https://creativecommons.org/licenses/by/4.0/). The full *Monster Manual* is **not** included.
- Optional online monster search via [Open5e](https://open5e.com/).
- Dungeons & Dragons is a trademark of Wizards of the Coast. This project is not affiliated with or endorsed by Wizards of the Coast.

**License:** _TODO: choose a license for the app code (e.g. MIT) and add a `LICENSE` file._ The SRD content stays under CC-BY-4.0 regardless of the license you choose.
