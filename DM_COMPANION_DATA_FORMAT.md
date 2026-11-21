# DM Companion — JSON Import Format

This document tells a person or an AI agent how to write a JSON file that **DM Companion** can import (sidebar or campaign screen → **📥 Import JSON**, or drag & drop the file onto the app).

When the file is imported, the user chooses:

- **Merge into current campaign**: adds the entries to the open campaign. Entries with the same name/title (sessions: same number) are updated, not duplicated. Nothing is deleted. **This is the normal choice for agent-generated files.**
- **Add as new campaign(s)**: keeps everything that exists and adds the campaign(s) from the file.
- **Replace everything**: restores a full backup and deletes everything else.

---

## 1. Rules for writing the file

1. Output **one valid JSON document**, UTF-8, no comments, no trailing commas.
2. Top level: `{ "campaigns": [ { …campaign… } ] }`. A single campaign object without the wrapper is also accepted.
3. **Only include what you know.** Omit unknown fields entirely. Do not write placeholders like `"unknown"` or `"N/A"` into number fields.
4. **IDs are optional.** Missing `id`s are generated on import. You may use your own short ids (e.g. `"npc-varra"`) for cross-references, but names work everywhere too (see §4).
5. Entries refer to each other **by name** (a character's `location` is a location's `name`, a quest's `chars` are character names, a `sessionId` may be the session number). Spell names exactly the same everywhere.
6. Text fields support **Markdown** (`**bold**`, `*italic*`, `- lists`, tables).
7. Numbers are JSON numbers (`15`, not `"15"`). Modifiers are plain integers (`3`, `-1`).
8. Use only the **allowed values** listed for enum fields (§3). Unknown values are shown as raw text or default to "other".
9. Do **not** invent stat blocks for monsters. Use `statblockRef` (§2.1) for creatures from the 5e SRD, otherwise fill `stats` only with values from the source.

---

## 2. Campaign object

| Field | Type | Notes |
|---|---|---|
| `name` | string | **Required.** With *Merge*, the campaign name in the file is ignored. |
| `system` | string | e.g. `"D&D 5e"` |
| `desc` | string | Short description |
| `calendarSystem` | enum | `faerun`, `greyhawk`, `eberron`, `exandria`, `shire`, `roshar` |
| `calendarCurrentDate` | date | `{ "year": 1492, "month": 4, "day": 10 }` (month is 1-based) |
| `npcs` | array | **PCs and NPCs** (§2.1) |
| `locations` | array | §2.2 |
| `sessions` | array | §2.3 |
| `quests` | array | §2.4 |
| `factions` | array | §2.5 |
| `encounters` | array | §2.6 |
| `items` | array | §2.7 |
| `lore` | array | §2.8 |
| `handouts` | array | §2.9 |
| `activityLog` | array | Adventure log (§2.10) |
| `calendarEvents` | array | §2.11 |
| `quickNotes` | array | §2.12 |
| `dmRef` | array | DM reference / screen entries (§2.13) |

Do not write `combats`, `maps`, `mapFiles` or `_files` (fights, map images and PDFs are created in the app; their structure is documented in §7 for completeness).

**Fields shared by most entries:** `id` (string, optional), `tags` (comma-separated string; calendar events use a string array), `imageUrl` (https URL), `createdAt` (ISO timestamp, optional), and `links` (array, §2.14).

### 2.1 Characters (`npcs`): player characters and NPCs

Player characters and NPCs share one list. Set `"isPC": true` for player characters.

| Field | Type | Notes |
|---|---|---|
| `name` | string | **Required.** Unique; used for matching on merge. |
| `isPC` | boolean | `true` = player character |
| `class` | string | Class/role, e.g. `"Ranger (Gloom Stalker)"`, `"Innkeeper"` |
| `race` | string | PCs: ancestry/species |
| `level` | number | PCs: 1–20 |
| `player` | string | PCs: player's name |
| `sheetUrl` | string | PCs: link to a character sheet |
| `location` | string | Current location (a location `name`) |
| `relation` | enum | NPCs: `friendly`, `neutral`, `hostile`, `unknown` (PCs: always `friendly`) |
| `status` | enum | `alive`, `dead`, `missing`, `underground`, `imprisoned`, `unknown_status` |
| `desc` | markdown | Appearance, personality |
| `secrets` | markdown | DM-only notes |
| `goals` | markdown | PCs: goals and motivation |
| `backstory` | markdown | PCs: backstory |
| `family` | string[] | Family and friends (names) |
| `aliases` | string[] | Other names |
| `quests` | string[] | Quest titles |
| `factions` | string[] | Faction names |
| `tags` | string | Comma-separated: `"villain, bandit"` |
| `imageUrl` | string | `https://…` image |
| `stats` | object | Combat stats and condition (below) |
| `locationHistory` | array | `[{ "location": "Thornwood", "sessionRef": "2", "date": "…", "note": "…" }]`: where the character has been |
| `statblock` | object | Advanced: a full custom stat block in the format of §7.3. Usually use `statblockRef` instead. |
| `statblockRef` | string | Name of a **5e SRD monster** (e.g. `"Bandit Captain"`, `"Veteran"`, `"Cult Fanatic"`, `"Adult Red Dragon"`). On import the full stat block is attached and `stats` is filled from it if `stats` is missing. If the name is not in the SRD it is ignored. |

**`stats` object:** every field is optional.

| Field | Type | Example |
|---|---|---|
| `ac` | number | `15` |
| `maxHp` | number | `36` |
| `hp` | number | current HP; defaults to `maxHp` |
| `tempHp` | number | `0` |
| `initMod` | number | initiative modifier, `3` |
| `speed` | string | `"30 ft., climb 30 ft."` |
| `pp` | number | passive Perception, `14` |
| `dc` | number | spell save DC, `13` |
| `ab` | number[6] | ability **scores** in order STR, DEX, CON, INT, WIS, CHA: `[10,16,14,10,14,8]`. An object `{"str":10,"dex":16,…}` is also accepted. |
| `saves` | string | `"Dex +5, Wis +4"` |
| `skills` | string | `"Perception +5, Stealth +7"` |
| `resist` | string | `"Resistant: fire; Immune: poison"` |
| `senses` | string | `"darkvision 60 ft.; Common, Elvish"` |
| `features` | markdown | class features, traits, attacks |
| `spells` | markdown | slots and known/prepared spells |
| `equipment` | markdown | weapons, armor, gold, items |
| `conditions` | string[] | Only these names: `Blinded`, `Charmed`, `Deafened`, `Frightened`, `Grappled`, `Incapacitated`, `Invisible`, `Paralyzed`, `Petrified`, `Poisoned`, `Prone`, `Restrained`, `Stunned`, `Unconscious` |
| `exh` | number | exhaustion level 0–6 |
| `conc` | boolean | concentrating on a spell |
| `inspiration` | boolean | PCs |
| `deathS`, `deathF` | number | death save successes / failures (0–3) |

### 2.2 Locations (`locations`)

| Field | Type | Notes |
|---|---|---|
| `name` | string | **Required**, unique |
| `type` | enum | `city`, `dungeon`, `region`, `building`, `tavern`, `other` |
| `parent` | string | Name of the containing location (builds the hierarchy) |
| `featured` | boolean | Gets its own tab |
| `desc` | markdown | Player-facing description |
| `notes` | markdown | DM notes |
| `tags` | string | comma-separated |
| `imageUrl` | string | |

### 2.3 Sessions (`sessions`)

| Field | Type | Notes |
|---|---|---|
| `num` | number | **Session number**; used for matching on merge |
| `title` | string | |
| `date` | string | Real-world date `"YYYY-MM-DD"` |
| `calendarDate` | date | In-game date `{ "year", "month", "day" }` |
| `planned` | boolean | `true` = not yet played |
| `notes` | markdown | Session summary |
| `journal` | markdown | Player journal entry |
| `journalAuthor` | string | |
| `chars` | string[] | PC names present |
| `npcChars` | string[] | NPC names met |
| `locs` | string[] | Location names visited |
| `factionRefs` | string[] | Faction names |
| `tags` | string | comma-separated |

### 2.4 Quests (`quests`)

`title` (**required**), `status` (`active`, `completed`, `failed`), `priority` (`high`, `medium`, `low`, `sidequest`), `giver` (character name), `desc`, `progress` (markdown, e.g. a checklist), `chars` (string[] names), `locs` (string[] location names), `tags`.

### 2.5 Factions (`factions`)

`name` (**required**), `type` (`guild`, `cult`, `noble`, `military`, `criminal`, `religious`, `political`, `other`), `alignment` (`good`, `neutral`, `evil`, `unknown`), `headquarters` (location name), `desc`, `goals`, `members` (text), `notes` (DM), `tags`.

### 2.6 Encounters (`encounters`)

`name` (**required**), `type` (`combat`, `social`, `exploration`, `trap`, `other`), `sessionId` (see §4), `location` (name), `desc`, `enemies` (text, e.g. `"3× Goblin, Bandit Captain"`; SRD names here are turned into combatants by **🎲 Run in Combat Tracker**), `chars` (string[] names), `factionRefs` (string[]), `outcome`, `notes`, `tags`.

### 2.7 Items (`items`)

`name` (**required**), `itemType` (`weapon`, `armor`, `wondrous`, `ring`, `rod`, `staff`, `wand`, `scroll`, `potion`, `other`), `rarity` (`common`, `uncommon`, `rare`, `very_rare`, `legendary`, `artifact`), `holder` (character name), `attunement` (boolean), `attunedTo` (character name), `desc`, `properties`, `curse`, `tags`.

### 2.8 Lore (`lore`)

`title` (**required**), `category` (`history`, `magic`, `religion`, `politics`, `geography`, `legend`, `other`), `content` (markdown), `tags`.

### 2.9 Handouts (`handouts`)

`title` (**required**), `type` (`letter`, `clue`, `map`, `riddle`, `prophecy`, `contract`, `other`), `content` (markdown, player-facing text), `isRevealed` (boolean: already given to players), `receivedBy` (names, text), `sessionId`, `dmNotes`, `tags`.

### 2.10 Adventure log (`activityLog`)

| Field | Type | Notes |
|---|---|---|
| `text` | markdown | **Required.** What happened |
| `category` | enum | `general`, `combat`, `travel`, `social`, `quest`, `discovery`, `loot`, `rest`, `character` |
| `ts` | string | ISO timestamp `"2026-11-07T20:15:00Z"` (orders the entries) |
| `sessionId` | string | see §4 |
| `calDate` | date | in-game date |

Log entries are always added on merge (they are never matched as duplicates), so don't import the same log twice.

### 2.11 Calendar events (`calendarEvents`)

`title` (**required**), `era` (`campaign`, `historical`, `mythological`), `type` (`world`, `campaign`, `npc`, `reminder`, `travel`, `festival`, `backstory`), `visibility` (`all`, `dm`, `players`), `date` (`{year,month,day}`; not for mythological), `endDate`, `yearOnly` (boolean, historical), `repeatYearly` (boolean), `mythologicalDate` (text, mythological era), `location` (name), `npcs` (string[]: NPC names), `pcs` (string[]: PC names), `entityLinks` (`[{ "type": "quest", "id": "The Missing Villagers", "label": "…" }]`; types as in §2.14), `description`, `tags` (**string array** here).

### 2.12 Quick notes (`quickNotes`)

`text` (**required**), `category` (`general`, `npc`, `location`, `event`, `reminder`).

### 2.13 DM reference (`dmRef`)

`title` (**required**), `category` (`travel`, `costs`, `combat`, `magic`, `social`, `tables`, `homebrew`, `other`), `content` (markdown, tables work well), `tags`, `pinned` (boolean).

### 2.14 Links (`links` on characters, locations, sessions, quests, encounters, factions, lore, handouts, items, DM reference)

```json
"links": [
  { "type": "url", "value": "https://example.com/handout.png", "label": "Original art" },
  { "type": "location", "value": "Sunless Crypt" },
  { "type": "npc", "value": "Mother Ilse" }
]
```

`type` is one of `url`, `npc`, `location`, `quest`, `encounter`, `faction`, `handout`, `lore`, `file`. For entity types, `value` is the entry's id **or its name/title** (converted to the id on import). `label` is optional. `file` links contain a base64 data URL in `value` (plus `mimeType`); avoid them because they count against the ~5 MB browser storage limit.

---

## 3. Allowed values (summary)

| Field | Values |
|---|---|
| character `relation` | friendly · neutral · hostile · unknown |
| character `status` | alive · dead · missing · underground · imprisoned · unknown_status |
| `stats.conditions` | Blinded · Charmed · Deafened · Frightened · Grappled · Incapacitated · Invisible · Paralyzed · Petrified · Poisoned · Prone · Restrained · Stunned · Unconscious |
| location `type` | city · dungeon · region · building · tavern · other |
| quest `status` / `priority` | active · completed · failed / high · medium · low · sidequest |
| faction `type` / `alignment` | guild · cult · noble · military · criminal · religious · political · other / good · neutral · evil · unknown |
| encounter `type` | combat · social · exploration · trap · other |
| item `itemType` / `rarity` | weapon · armor · wondrous · ring · rod · staff · wand · scroll · potion · other / common · uncommon · rare · very_rare · legendary · artifact |
| lore `category` | history · magic · religion · politics · geography · legend · other |
| handout `type` | letter · clue · map · riddle · prophecy · contract · other |
| log `category` | general · combat · travel · social · quest · discovery · loot · rest · character |

## 4. IDs and references

Most fields store **names** directly (a character's `location`, a quest's `chars`, an item's `holder` …). A few fields are stored internally as **ids**, but on import you may write **either the id or the name**; names are converted to ids automatically:

| Field | Write |
|---|---|
| `encounters[].sessionId`, `handouts[].sessionId`, `activityLog[].sessionId`, `quickNotes[].sessionId` | session `id` **or session number** (e.g. `3`) |
| `links[].value` (entity types) | entry id or name/title |
| `calendarEvents[].npcs` / `.pcs` | character ids or names |
| `calendarEvents[].entityLinks[].id` | entry id or name/title |

Example with your own ids:

```json
"sessions": [ { "id": "s3", "num": 3, "title": "Into the Crypt" } ],
"activityLog": [ { "sessionId": "s3", "category": "combat", "text": "Defeated 4 skeletons." } ]
```

The same works with `"sessionId": 3`. On merge, ids that already exist in the campaign are remapped automatically and all references follow (sessions, links, calendar characters, fights, map markers).

## 5. Merge behaviour (what happens on import)

- Matching keys: characters, locations, factions, encounters, items: `name` · quests, lore, handouts, DM reference, calendar: `title` · sessions: `num` (all case-insensitive).
- With "update existing" on (the default), fields present in the file overwrite the existing ones. Empty or missing fields keep the old value. `stats` are merged field by field.
- New entries are added. Nothing is ever deleted by a merge.

### Normalization on import

The importer repairs common mistakes, so a file does not have to be perfect:

- A single campaign object or a bare array of campaigns is accepted instead of `{ "campaigns": [...] }`.
- Missing `id`s and `createdAt` are generated. Sessions without `num` are numbered in file order.
- Comma-separated strings become lists for list fields (`family`, `aliases`, `quests`, `factions`, `chars`, `npcChars`, `locs`, `factionRefs`). Tag arrays become comma-separated strings (except calendar `tags`).
- In `stats`: numbers written as text (`"15"`, `"+2"`) are converted, `hp` defaults to `maxHp`, `ab` may be an object (`str`, `dex` …), list-valued text fields are joined with line breaks, condition names are matched case-insensitively and **unknown conditions are dropped**, `exh` is clamped to 0–6.
- Missing defaults: quest `status` = `active`, `priority` = `medium`; character `status` = `alive`, `relation` = `friendly` for PCs and `unknown` for NPCs; location `type` = `other`; unknown log categories become `general`.
- `statblockRef` is resolved to the SRD stat block (removed afterwards). If `stats` is missing, it is filled from the stat block.

## 6. Minimal complete example

```json
{
  "campaigns": [
    {
      "name": "Curse of the Ashen Vale",
      "system": "D&D 5e",
      "npcs": [
        {
          "name": "Finnith",
          "isPC": true,
          "player": "Anna",
          "race": "Wood Elf",
          "class": "Ranger (Gloom Stalker)",
          "level": 4,
          "location": "Millbrook Crossing",
          "goals": "Find out who burned her home grove.",
          "stats": {
            "ac": 15, "maxHp": 36, "hp": 29, "initMod": 3, "speed": "35 ft.", "pp": 15, "dc": 13,
            "ab": [10, 16, 14, 10, 14, 8],
            "saves": "Str +2, Dex +5",
            "skills": "Perception +5, Stealth +5, Survival +4",
            "senses": "darkvision 60 ft.; Common, Elvish",
            "features": "- **Favored Enemy:** humanoids\n- **Dread Ambusher**",
            "spells": "Slots: 1st 3/3\nHunter's Mark, Cure Wounds",
            "equipment": "Longbow, 2 shortswords, studded leather, 23 gp",
            "conditions": ["Poisoned"],
            "exh": 0
          }
        },
        {
          "name": "Varra Cinderhook",
          "class": "Bandit captain",
          "relation": "hostile",
          "location": "Thornwood",
          "factions": ["Cinder Wolves"],
          "desc": "Scarred half-orc with a burned hook for a left hand.",
          "statblockRef": "Bandit Captain"
        }
      ],
      "locations": [
        { "name": "Millbrook Crossing", "type": "city", "desc": "Market town around a stone bridge." },
        { "name": "Thornwood", "type": "region", "parent": "Millbrook Crossing" }
      ],
      "factions": [
        { "name": "Cinder Wolves", "type": "criminal", "alignment": "evil", "headquarters": "Thornwood" }
      ],
      "quests": [
        { "title": "The Missing Villagers", "status": "active", "priority": "high", "giver": "Captain Hale", "chars": ["Finnith"] }
      ],
      "sessions": [
        { "id": "s1", "num": 1, "title": "Smoke over Millbrook", "date": "2026-10-24", "notes": "The party arrives in Millbrook.", "chars": ["Finnith"] }
      ],
      "activityLog": [
        { "sessionId": "s1", "category": "travel", "ts": "2026-10-24T19:10:00Z", "text": "Arrived in **Millbrook Crossing**." }
      ]
    }
  ]
}
```

## 7. Full backup structure (written by the app)

A file exported by the app (**💾 Export JSON** or the sync file) has this shape. Agents normally do not write these parts, but they are described here so the format is complete.

```text
{
  "campaigns": [ campaign, … ],
  "_files": { "<fileId>": { "name": "Crypt.pdf", "type": "application/pdf", "data": "<base64>" } }   // only when "Include map files" is on
}
```

### 7.1 Additional campaign fields

| Field | Content |
|---|---|
| `id`, `createdAt` | Campaign id and creation time |
| `isDemo` | `true` for the demo campaign |
| `combats` | Fights (§7.2) |
| `maps` | Map View images: `[{ "id", "name", "imageUrl" (https or data URL), "markers": [{ "locId", "x", "y" }] }]`. `x`/`y` are percentages (0–100) of the image width/height; `locId` is a location id. |
| `customCategories` | User-defined categories per type: `{ "encounterType" \| "itemType" \| "loreCategory" \| "handoutCategory" \| "locationCategory" \| "factionType" \| "noteCategory" \| "dmRefCategory": [{ "value", "label", "color"? }] }`. Their `value`s are also valid in the matching enum fields. |
| `calendarCustom` | A fully custom calendar (`calendarSystem: "custom"`): name, abbreviation, months with days, weekdays |
| `calendarOverrides` | Edits to a built-in calendar system (month/weekday names, festival days) |
| `locations[].mapFiles` | `[{ "id", "name", "type" (MIME), "size" (bytes), "addedAt", "notes" (markdown), "areas": [{ "id", "label", "page" (number or null), "text" }] }]`. The file content lives in the browser database and, in exports, in `_files` under the same `id`. |
| `npcs[].notes` | Older per-session note entries (kept for compatibility) |

### 7.2 Fights (`combats`)

```text
{ "id", "name", "status": "active" | "ended", "round": 0 (not started) | 1…,
  "turnId": combatant id whose turn it is, "encounterId", "sessionId",
  "createdAt", "startedAt", "endedAt", "summary" (markdown, set when ended),
  "log": [ { "t": ISO time, "r": round, "x": text } ],
  "combatants": [ {
      "id", "name", "kind": "pc" | "npc" | "monster" | "custom",
      "refId": character id (pc/npc; HP and conditions are written back to it),
      "init": number | null, "initMod", "ac", "hp", "maxHp", "tempHp",
      "conditions": [ { "n": condition name, "r": rounds left | null } ],
      "exh", "conc", "deathS", "deathF", "notes", "defeated",
      "sb": stat block (§7.3) | null } ] }
```

### 7.3 Stat block format (`statblock` on characters, `sb` on combatants)

Compact keys (the same format as the built-in SRD data):

| Key | Meaning | Key | Meaning |
|---|---|---|---|
| `n` | name | `sv` | saving throws, text |
| `sz` | size | `sk` | skills, text |
| `t` / `st` | type / subtype | `dv` / `dr` / `di` / `ci` | damage vulnerabilities / resistances / immunities, condition immunities (text) |
| `al` | alignment | `sn` | senses, text (incl. passive Perception) |
| `ac` / `acd` | armor class / armor description | `lg` | languages |
| `hp` / `hd` | average HP / hit dice (e.g. `"7d10"`, CON bonus is added for display) | `tr` | traits: `[["Name","Description"], …]` |
| `spd` | speed, text | `ac2` | actions `[["Name","Description"], …]` |
| `ab` | ability scores `[STR, DEX, CON, INT, WIS, CHA]` | `ba` / `re` | bonus actions / reactions (same format) |
| `cr` | challenge rating as a number (`0.25` = 1/4) | `la` / `lad` | legendary actions (same format) / intro text |
| `src` / `srcName` | `"srd"`, `"o5e"` (Open5e) or `"custom"` / source name | | |

Descriptions can contain `"+5 to hit"` and `"(2d6 + 3)"`; the app makes these clickable dice rolls.

### 7.4 Where the data lives

The app keeps campaign data in the browser's **localStorage** (key `dmtool_v2`, about 5 MB limit) and map files in **IndexedDB** (`dmtool_files`). An exported or synced JSON file is the complete, portable copy. See the manual, section "Where Your Data Is Stored".

## 8. Prompt you can give an agent

> Read `DM_COMPANION_DATA_FORMAT.md` and the attached PDF. Create one JSON file that follows this format exactly. Put every player character and NPC from the PDF into `npcs` (`"isPC": true` for player characters) with all stats the PDF gives (AC, HP, ability scores, saves, skills, features, spells, equipment, conditions). For NPCs that are standard 5e SRD monsters, set `statblockRef` to the monster's name. Also add locations, factions, quests, items and sessions that the PDF mentions. Leave out anything the PDF does not state. Output only the JSON.
