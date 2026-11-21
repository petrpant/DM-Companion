# DM Companion Manual

DM Companion is a local campaign workspace for Dungeon Masters. It keeps sessions, characters, locations, quests, fights, handouts, lore, calendar events, items, notes and maps in one place. The whole app is a single HTML file; there is no server and no account.

The data format for importing (e.g. data created by an AI agent from a PDF) is described in the separate file **`DM_COMPANION_DATA_FORMAT.md`**.

---

## 1. Getting Started

Open the HTML file in a browser (Chrome or Edge recommended; Firefox and Safari work except for automatic file sync). On the campaign screen you can:

- **+ New Campaign**: create an empty campaign.
- **🎲 Load Demo Campaign**: create a fully filled example campaign (see §2).
- **📥 Import JSON**: load a backup or a generated file (see §13).

A campaign contains all of its data: sessions, characters, calendar, maps, fights, log and notes. You can have several campaigns. Switch with **← Campaigns** at the bottom of the sidebar.

### Sidebar

| Section | Page | Purpose |
|---|---|---|
| Overview | **Dashboard** | Counters, quick note, campaign date, recent sessions, upcoming events, active quests and characters, factions, unrevealed handouts, new lore |
| | **DM Reference** | Your DM screen: rules tables, prices, homebrew. Entries can be pinned to the top |
| | **Search** | Searches the whole campaign (see §12) |
| Content | **PCs** | Player characters with a party overview table |
| | **Sessions** | Session summaries, journals and links. Each session has tabs, including a **Log** tab |
| | **Adventure Log** | Running log of what the party did (§5) |
| | **Calendar** | In-game calendar with month, year, timeline, agenda and myth views |
| | **NPCs** | Non-player characters, filterable by relationship and faction |
| | **Locations** | Places, sub-locations, map view with markers, PDF/image maps with notes (§8) |
| | **Quests** | Quests with status, priority, progress and linked characters |
| | **Encounters** | Planned or past encounters; **🎲** runs one in the Combat Tracker |
| | **Combat Tracker** | Initiative, HP, conditions, stat blocks (§6) |
| | **Bestiary (SRD)** | 325 monsters with full stat blocks, offline (§7) |
| | **Factions, Lore, Handouts, Items** | Supporting material |
| Views | **Relationship Map** | Graph of PCs, NPCs, factions and locations |
| Tools | **AI-Import, Quick Notes, Tools, Settings** | See §11 and §14 |

Most text fields support **Markdown**: headings, `**bold**`, `*italic*`, lists, links and simple tables. Tags are comma-separated and can be clicked in tag clouds to filter.

---

## 2. Demo Campaign

**🎲 Load Demo Campaign** (campaign screen, or Settings → Danger Zone) creates *Demo: The Ashen Vale*, an original setting that uses every feature:

- 4 level-4 PCs with full stats, spells and equipment; some have conditions, exhaustion, concentration or inspiration
- 8 NPCs, several with full SRD stat blocks (Veteran, Priest, Bandit Captain, Cult Fanatic, Mage)
- 4 sessions (3 played, 1 planned), 5 quests, 3 factions, 4 encounters, 3 lore entries, 3 handouts (1 unrevealed), 4 items, quick notes, pinned DM reference tables, calendar events in all eras
- 16 Adventure Log entries
- 2 fights: one finished, one **in progress** (round 2, Brother Morrow's turn), ready for **Next Turn**
- A world map with markers (Locations → Map View) and two location maps with notes: a town image (*Millbrook Crossing*) and a 2-page PDF dungeon (*Sunless Crypt*)

The demo is an ordinary campaign. Edit it freely or delete it in Settings → Danger Zone. Loading it again asks before creating a second copy.

---

## 3. Basic Workflow

1. Create or select a campaign.
2. Add PCs (with stats), important NPCs, locations, factions and quests.
3. Create a session before or after play and link the characters, locations and factions involved.
4. During play: run fights in the Combat Tracker and jot events into the Adventure Log.
5. Advance the campaign date in the calendar (Settings → Calendar Settings, ±1/±7 days).
6. Use tags to group and filter related material.

---

## 4. Characters (PCs and NPCs)

PCs and NPCs share one form. **⭐ Player Character** enables the extra fields: player, ancestry, level, sheet link, goals and backstory. NPCs have a relationship to the party (friendly, neutral, hostile, unknown) and a status (alive, dead, missing, underground, imprisoned, unknown).

### Stats, Hit Points & Conditions

Every character has an expandable **⚔️ Stats, Hit Points & Conditions** section:

- AC, max/current/temporary HP, initiative modifier, speed, passive Perception, spell save DC
- Ability scores (modifiers are shown automatically), saving throws, skills, resistances/immunities, senses & languages
- Conditions (the 14 SRD conditions), exhaustion level 0–6, concentrating, inspiration
- Features & abilities, spells & slots, equipment & inventory (Markdown)

**📥 Load stat block from SRD** picks a monster (e.g. *Bandit Captain*), fills these fields from it and keeps the full stat block on the character. It is shown in the character's detail view and in the Combat Tracker.

### Detail view (👁)

- **HP:** type an amount, then **− Dmg**, **+ Heal** or **Temp** (Enter = damage, Shift+Enter = heal). Damage removes temporary HP first. Healing never exceeds max HP.
- **Conditions:** click to toggle. The rules text of each active condition is shown.
- **Death saves:** shown for PCs at 0 HP. Healing above 0 resets them.
- **🏕️ Long Rest:** HP to max, temporary HP to 0, death saves reset, exhaustion −1, *Unconscious* removed. Adds a log entry.
- **🎲 Add to current fight:** puts the character into the open fight (or a new one).
- Blue numbers (ability scores, attacks, damage) roll dice when clicked.

The **PCs** page starts with a party overview table (AC, HP bar, initiative, passive Perception, spell DC, conditions). Cards show AC, HP and conditions at a glance.

---

## 5. Adventure Log

A chronological record of what the party did.

- **Quick add:** text, category (General, Combat, Travel, Social, Quest, Discovery, Loot & Rewards, Rest, Character) and session (defaults to the latest played session). **Ctrl+Enter** adds. The current in-game date is stored with each entry.
- Entries are grouped by session. You can filter by category and session, search, hide automatic entries, reverse the sort order, and edit or delete entries.
- **⬇ Export .md** downloads the whole log as Markdown (useful as a recap for players).
- Every session's detail page has a **📓 Log** tab with that session's entries and its own quick-add field.

**Automatic entries** (marked *auto*) are created when:

- a fight starts or ends (the end summary lists rounds, defeated enemies, PCs who went down, and XP in total and per PC)
- a quest is created or its status changes
- an NPC's or PC's status changes (e.g. to *Dead*) or a PC's level increases
- a character takes a long rest
- you click **📓 → Adventure Log** in the Combat Tracker

---

## 6. Combat Tracker

### Setting up a fight

- **+ New Fight** (page header), or **🎲** on an encounter. The encounter's *Opponents* text (e.g. `3× Goblin, Bandit Captain (CR2), 2x Wolves`) is turned into SRD monsters. Quantities, `×`/`x` and plural names are understood, and text in brackets is ignored. Names that are not in the SRD are reported so you can add them manually. NPCs listed under *Involved Characters* are added, and you are asked whether to add all PCs.
- **⭐ + Party** adds all living PCs. **🧙 + Character** adds any PC/NPC. **🐉 + Monster (SRD)** opens the bestiary with quantity and HP mode (average or rolled from the hit dice). **+ Custom** asks for name, HP, AC and initiative modifier.
- Several monsters of the same kind are numbered automatically (*Goblin 1*, *Goblin 2* …).
- Each fight can be linked to an encounter and a session. The encounter's detail view lists its fights.

### Running a fight

- Enter player initiative in the **Init** fields. **🎲 Roll Init** rolls d20 + modifier for every non-PC without a value. The order sorts automatically (ties: higher modifier first).
- **⚔️ Start Combat** (warns if someone has no initiative), then **Next Turn ▶** / **⏮ Prev**. Keyboard: **N** = next turn, **Shift+N** = previous.
- **HP:** type an amount, then **−** (damage), **+** (heal), **T** (temporary HP). Enter = damage, Shift+Enter = heal. Click the HP numbers to edit max HP, the AC badge to edit AC, the name to rename.
- **Conditions:** **+ condition** adds an SRD condition with an optional duration in rounds. Durations count down when that creature's turn ends, and expired conditions are removed and logged. The same menu has *Exhaustion +1*, *Concentration* (toggle) and *Custom…* (e.g. *Hasted*, *Blessed*). **✕** on a chip removes it.
- **Rules handled automatically:**
  - Damage to a concentrating creature shows the **Concentration DC** (10 or half the damage, whichever is higher).
  - A monster or NPC at 0 HP is marked **defeated** and skipped in the turn order.
  - A PC at 0 HP becomes *Unconscious* and gets **death save** dots. Damage at 0 HP adds a failed save. Three failures mark the PC dead, three successes mark them stable. Damage that exceeds the PC's remaining HP by at least their max HP warns about **massive damage**.
  - Healing a PC above 0 removes *Unconscious* and resets death saves.
- **Row buttons:** ☠️ defeat/revive, 📝 notes, ⧉ add another of the same monster, ✕ remove.
- **Side panel:** shows the selected creature (or the one whose turn it is) with its notes, the rules of its active conditions and its **full stat block**. PCs/NPCs without a stat block show their stats, features, spells and equipment. Blue values roll dice; results appear as a toast and in the combat log.
- **Combat log:** every action is recorded with its round. You can add notes, and **📓 → Adventure Log** also copies a note into the campaign log.
- **PC/NPC hit points, conditions, exhaustion, concentration and death saves are saved back to the character** while you play.
- **🏁 End Fight** writes a summary (rounds, defeated enemies, downed PCs, XP total and per PC) to the Adventure Log. Ended fights stay in the fight list and can be reopened (**↺ Reopen**) or deleted.

---

## 7. Bestiary (SRD) and Monster Data

- The **Bestiary** contains all **325 monsters of the 5e System Reference Document 5.1**, built into the app, so it **works offline**. Search by name or type, and filter by creature type and CR range.
- Each stat block is complete: AC, HP and hit dice, speed, abilities, saves, skills, resistances/immunities, senses, languages, challenge with XP and proficiency bonus, traits, actions, reactions and legendary actions. Blue values roll dice.
- From the bestiary page you can **add monsters to the current fight** or **create an NPC** from a stat block.
- **🌐 Open5e (online)** additionally searches the free Open5e database (needs internet). This adds third-party OGL monsters (e.g. *Tome of Beasts*); SRD duplicates are hidden.
- **Note:** the *Monster Manual* itself is copyrighted and not included. The SRD covers most of its common monsters (goblins, orcs, dragons, undead, giants …); anything else needs Open5e, a custom combatant, or stats typed into an NPC.
- **Tools → D&D 5e SRD Search** is the older Open5e search that imports monsters, items and spells as encounters, NPCs, items or lore.

---

## 8. Locations and Maps

### Locations

Locations have a type, an optional parent (*Part of*), a description, DM notes, tags and links. *Featured* locations get their own tab. Sub-locations are listed under their parent.

### Map View (pins on an image)

**Locations → 🗺 Map View** shows map images with location markers. Click the map to place a marker, right-click a marker to remove it, scroll or pinch to zoom, drag to pan. Map images are managed in **Settings → Campaign Settings → Manage Maps** (local file or image URL; local images should stay under ~3 MB because they are stored in browser storage).

### Maps & Files (PDFs and images with notes)

Open a location (**👁**) → **📄 Maps & Files** → **+ Add PDF / Image map** (several files at once are possible). Location cards show a chip per map file for quick access.

The **viewer** shows the map on the left and notes on the right:

- **Map Notes:** general Markdown notes, saved automatically while typing (**👁 Preview** shows them rendered).
- **Area / Room Notes:** name, optional PDF page and text (e.g. *Room 4 · p.2 · 3 skeletons, DC 14 trap*). The **p.N →** button jumps the PDF to that page (supported by the built-in PDF viewers of Chrome, Edge and Firefox).
- Rename by editing the title. **↗ New Tab** opens the file in its own browser tab, **⬇ Download** saves it, **📝 Notes** hides or shows the notes panel. Images toggle between fit and full size when clicked.
- If a map file is missing in this browser (e.g. on a second computer whose data came from a sync file without files), the viewer offers **Re-upload**. Notes are kept.

Map files are stored in the browser's database (IndexedDB), not in normal browser storage, so large PDFs are fine. Files over 60 MB trigger a warning because they make backups large.

---

## 9. Sessions

Sessions store the real-world date, the in-game date, summary notes, a player journal entry, linked PCs, NPCs, locations and factions, tags, an image and links. Planned sessions are marked as such. The session detail page has tabs for Overview, Characters, Locations, Quests, Encounters, Handouts, Journal and **📓 Log**. Use the previous/next buttons to move through the campaign timeline. New names mentioned in a session can be created as entries with one click.

---

## 10. Calendar

The calendar uses the campaign's current date. Choose the calendar system (Faerûn, Greyhawk, Eberron, Exandria, Shire, Roshar or custom) and set or advance the date in **Settings → Calendar Settings**. You can also edit a system's months, weekdays and festival days.

Events have an era (campaign, historical, mythological), a type (world, campaign, NPC, reminder, travel, festival, backstory), a visibility (**All**, **DM** = DM-only, **Players**), dates, location, characters, links and tags. Campaign events can repeat yearly.

Views: Month, Year, Timeline, Agenda (next 14 days to 1 year) and Mythos. Filters: type, visibility, tags, text. In the Timeline and Mythos views, **Export** exports the currently filtered view and **Export All** exports everything, including DM-only entries.

---

## 11. Other Sections

- **Handouts:** letters, clues, maps, riddles, prophecies, contracts. Mark whether they have been **revealed** to players; unrevealed ones appear on the dashboard.
- **Factions, Lore, Items:** items can have a holder and attunement; they appear on that character's card and detail view.
- **Quick Notes:** fast notes with a category, optionally linked to a session or entity. Several notes can be merged.
- **Relationship Map:** interactive graph of PCs, NPCs, factions and (optionally) locations.
- **Tools:** Open5e SRD search, **Story Summary** (AI), **Data Health** check (AI). The data check finds missing links, duplicates, stale quests and similar problems, with one-click fixes.
- **AI-Import:** turns free text (journals, notes) into NPCs, locations, quests and more. *This feature is not finished yet.* All AI features need your own Anthropic API key (campaign screen → ⚙ Settings).

---

## 12. Search and Tags

Use tags consistently to connect related material. Tag clouds on each page filter by tag. The **global search** covers characters, locations, quests, factions, sessions, lore, quick notes, Adventure Log entries and map files (file names, map notes and area notes).

---

## 13. Import and Export

### Export

- **💾 Export JSON** (sidebar) downloads a complete backup (`dnd-companion.json`) of **all campaigns**.
- Map files (PDFs/images) are embedded in the export unless **Settings → Cloud Sync → Include map files** is turned off.

### Import

**📥 Import JSON** (sidebar or campaign screen), or drag & drop a `.json` file onto the app. You then choose:

| Option | What it does |
|---|---|
| **🔀 Merge into current campaign** | Adds the entries to the open campaign. Entries with the same name or title (sessions: same number) are **updated** when "Update entries that already exist" is ticked, otherwise skipped. Nothing is deleted. Only available while a campaign is open. |
| **➕ Add as new campaign(s)** | Adds the file's campaign(s) next to the existing ones. Duplicate names get "(imported)". |
| **♻️ Replace everything** | Restores a full backup. **All campaigns in this browser are replaced** (asks for confirmation). |

Import is forgiving: missing ids are created, numbers written as text are converted, comma-separated text becomes lists, condition names are corrected, and references may use names instead of ids. Monsters can be referenced by SRD name (`statblockRef`). Adventure Log entries are always added and never matched, so importing the same file twice duplicates them.

To let another tool or an **AI agent** create importable data (e.g. all characters from a PDF), give it **`DM_COMPANION_DATA_FORMAT.md`**. Section 8 of that file contains a ready-made prompt.

---

## 14. Where Your Data Is Stored

Everything is stored **in your browser on this computer**. There is no server. The HTML file itself contains no campaign data, only the app and the built-in SRD data.

| Data | Location in the browser |
|---|---|
| All campaign data: sessions, characters with stats, quests, locations, fights, log, calendar, notes, map markers | **localStorage**, key `dmtool_v2` (one JSON block) |
| Images and files attached via 📎 links, Map View images | Inside the same localStorage JSON (as embedded data) |
| Location map files (PDF / images) | **IndexedDB** database `dmtool_files` |
| Connected sync file (Chrome/Edge) | IndexedDB database `dmtool_fs` |
| Settings: API key, font size, auto-export, include-map-files, last export time | Small localStorage entries `dmtool_apikey`, `dmtool_fontsize`, `dmtool_autoexport`, `dmtool_includeFiles`, `dmtool_lastExport` |
| SRD monsters and condition rules | Built into the HTML file (read-only) |

What this means:

- Data belongs to **one browser on one computer**. Another browser, a private window or another PC starts empty.
- **Clearing browsing data / site data deletes everything.**
- localStorage holds roughly **5 MB**. Big images in Map View or as 📎 attachments fill it quickly; save fails if it is full. Map files in *Maps & Files* do not count toward this limit.

What leaves your computer:

- **AI features** (AI-Import, Story Summary, Data Health) send campaign text to Anthropic, and only if you entered an API key.
- **Open5e searches** send only the search text you type.
- Nothing else is sent anywhere.

---

## 15. Sync and Backup

**Recommended (Chrome / Edge):**

1. Open **Settings → Cloud Sync**.
2. **Connect File** (existing JSON) or **Create New File**.
3. Put that file in iCloud Drive, Dropbox, Google Drive or another synced folder.
4. On another computer, connect the same file.

Every change is written to the file automatically (half a second after the change). When the window regains focus and the file was changed elsewhere, it is reloaded. After a browser restart the file may need **🔄 Restore Sync** (a permission click).

**Other browsers (Firefox / Safari):** use **Export** and **Import**, or tick *Auto-export after each change* (downloads a JSON file after each change).

**Include map files in export & sync file** (on by default) embeds PDFs and images in the JSON so that backups and other computers get them too. The sync file is rewritten on every change, so with many large PDFs it gets big. Turn the option off if syncing becomes slow, and then back up map files separately.

---

## 16. Important Notes

- Keep regular backups of your JSON file. Browser storage is convenient but not a backup.
- Mark DM-only material with DM visibility or keep it in DM notes / secrets.
- Check filters and visibility before sharing calendar exports with players.
- The AI-supported input feature is not finished yet.

## 17. Changes in this version

- Interface fully in English (remaining German texts translated, including AI prompts).
- New: character stats & conditions, party overview, Adventure Log, Combat Tracker, offline SRD Bestiary, PDF/image location maps with notes, demo campaign, merge import, import format spec.
- Fixed: clickable name chips (characters, locations, quest givers, item holders) did not work; the dashboard failed when an unrevealed handout existed; editing a quest reset its creation date; editing a location or character could drop fields not shown in the form.

## About

DM Companion  
Version: 0.9.0  
Build Date: 2026-05-02  
Created by Gregor Vogt  
Monster and condition data: System Reference Document 5.1 © Wizards of the Coast LLC, licensed under CC-BY-4.0  
Homepage: https://www.gregorvogt.com/dnd-companion/
