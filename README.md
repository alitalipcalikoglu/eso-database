# ESO Database

A ready-to-use, offline dataset of **The Elder Scrolls Online** game content — skills, sets, achievements, collectibles, furniture, motifs, food, dyes, leads and more — shipped as plain JSON plus the matching icon and tooltip images.

No build step, no API key, no server. Clone the repo (or fetch a single file over raw URLs) and read the JSON.

- **13 content categories**
- **16,600+ top-level records** (plus ~3,500 nested achievements and ~7,400 nested motif parts)
- **52,000+ image files** (~26,300 icons, ~25,800 tooltip cards)
- **~1.5 GB** total

---

## Contents

- [Dataset overview](#dataset-overview)
- [Repository layout](#repository-layout)
- [Asset conventions](#asset-conventions)
- [Data schemas](#data-schemas)
- [Usage](#usage)
- [Known quirks](#known-quirks)
- [Source & attribution](#source--attribution)
- [License](#license)

---

## Dataset overview

| Category | Records | Icons | Tooltips | Size |
|---|---:|---:|---:|---:|
| [`achievements/`](achievements) | 45 categories → 196 subcategories → 3,510 achievements | 3,508 | 3,508 | 155 MB |
| [`buffs-debuffs/`](buffs-debuffs) | 69 | — | — | 12 KB |
| [`collectibles/`](collectibles) | 4,875 (4,221 unique) | 4,221 | 4,221 | 140 MB |
| [`dyes-colors/`](dyes-colors) | 261 | — | 522 | 14 MB |
| [`food-drinks/`](food-drinks) | 571 | 571 | 571 | 18 MB |
| [`housing/`](housing) | 8,020 (7,950 unique) | 7,950 | 7,950 | 204 MB |
| [`leads/`](leads) | 584 (573 unique) | 573 | — | 4.4 MB |
| [`motifs-outfit-styles/`](motifs-outfit-styles) | 430 styles → 7,399 parts | 7,797 | 7,397 | 217 MB |
| [`sets/`](sets) | 758 (677 unique) | 677 | 677 | 32 MB |
| [`skills/`](skills) | 932 (931 unique) | 931 | 931 | 49 MB |
| [`status-effect/`](status-effect) | 8 | 8 | — | 100 KB |
| [`traits/`](traits) | 33 | 33 | — | 284 KB |
| [`zone/`](zone) | 52 | 52 | — | 3.8 MB |

Where "unique" is lower than the record count, the same entry is listed once per category it belongs to — see [Known quirks](#known-quirks).

**What's inside each category**

| Category | Description |
|---|---|
| `achievements` | Every achievement, grouped by category (Character, PvP, Crafting, chapters/DLC…) and subcategory. |
| `buffs-debuffs` | Major/Minor buffs and debuffs with their current numeric effects. |
| `collectibles` | Mounts, pets, costumes, emotes, houses, personalities, memento, Tales of Tribute cards, appearance items — 98 categories. |
| `dyes-colors` | Dye names with their exact `rgb()` value and the achievement that unlocks them. |
| `food-drinks` | Provisioning food and drinks with buff text, quality and category. |
| `housing` | Furniture and house items across 111 categories, with quality tiers. |
| `leads` | Antiquities leads: which zone they drop in, which item they unlock, and where to find them. |
| `motifs-outfit-styles` | Crafting motifs / outfit styles with every individual style piece. |
| `sets` | Gear sets with full bonus text, type, category (Trial, Dungeon, Mythic, Monster…) and granted buffs. |
| `skills` | All class, weapon, armor, guild, world, alliance war, racial and craft skills including morphs, passives and Scribing. |
| `status-effect` | The eight elemental status effects. |
| `traits` | Armor, weapon and jewelry traits. |
| `zone` | Zone list with header images. |

---

## Repository layout

Every category follows the same shape:

```
<category>/
├── data/
│   └── raw.json        # the dataset
├── icons/              # 64×64 PNG, one per entry   (when available)
└── tooltips/           # rendered tooltip card PNG  (when available)
```

For example:

```
skills/
├── data/raw.json
├── icons/dragonknight-standard.png
└── tooltips/dragonknight-standard.png
```

---

## Asset conventions

- **Filenames are the record `id`.** `item.id + ".png"` resolves to the file in both `icons/` and `tooltips/`, so no lookup table is needed.
- **Icons** are 64×64 RGBA PNGs — the in-game ability/item icon.
- **Tooltips** are pre-rendered tooltip cards (variable size, roughly 420–670 px wide) showing the entry as it appears in game.
- **Zone "icons"** are the exception: they are 1600×300 zone header banners, and although they carry a `.png` extension the bytes are **JPEG**. Detect the format by content, not by extension.
- **Dye tooltips** come in two flavours per dye: `<id>.png` (the colour swatch card) and `<id>-achievement.png` (the unlock achievement card).
- **Motif tooltips** exist for the individual style *parts*, not for the parent style.
- Each record also keeps the original remote `icon` URL and a `link` back to the source page.

---

## Data schemas

All files are a top-level JSON **array**. Fields are strings unless noted.

### `skills/data/raw.json`

```json
{
  "id": "dragonknight-standard",
  "name": "Dragonknight Standard",
  "description": "Call down a battle standard, dealing 870 Flame Damage every 1 second…",
  "icon": "https://eso-hub.com/storage/icons/ability_dragonknight_006.png",
  "category": "Dragonknight",
  "subcategory": "Ardent Flame",
  "type": "Ultimate",
  "link": "https://eso-hub.com/en/skills/dragonknight/ardent-flame/dragonknight-standard",
  "buffs": "Major Defile"
}
```

`category`: `Arcanist`, `Dragonknight`, `Necromancer`, `Nightblade`, `Sorcerer`, `Templar`, `Warden`, `Weapon`, `Armor`, `Guild`, `World`, `Alliance-war`, `Racial`, `Craft`
`type`: `Active` (525), `Passive` (290), `Ultimate` (105), `Scribing` (12)

### `sets/data/raw.json`

```json
{
  "id": "archers-mind",
  "name": "Archer's Mind",
  "description": "(2 items) Adds 1096 Maximum Stamina\n(3 items) Adds 657 Critical Chance\n…",
  "icon": "https://eso-hub.com/storage/icons/gear_bosmer_medium_head_d.png",
  "link": "https://eso-hub.com/en/sets/archers-mind",
  "types": "Weapons,Medium Armor,Jewels",
  "category": "Arena",
  "buffs": "",
  "bonusses": "Maximum Stamina"
}
```

`category`: `Arena`, `Class Sets`, `Craftable`, `Dungeon`, `Monster Set`, `Mythic`, `Overland`, `PvP`, `Trial`, `Unknown`
`types` and `bonusses` are comma-separated lists inside a single string. Set bonuses live in `description`, one per line (`\n`-separated).

### `achievements/data/raw.json`

Three levels deep:

```json
{
  "id": "character",
  "name": "Character",
  "subcategories": [
    {
      "id": "anniversary",
      "name": "Anniversary",
      "achievements": [
        {
          "id": "adventurer-across-a-decade",
          "name": "Adventurer Across a Decade",
          "icon": "https://eso-hub.com/storage/icons/u42_achievement_10thanniversary.png",
          "link": "https://eso-hub.com/en/achievements/adventurer-across-a-decade",
          "description": "Journey through a decade of characters and stories…"
        }
      ]
    }
  ]
}
```

### `motifs-outfit-styles/data/raw.json`

Two levels deep — a style and its craftable pieces:

```json
{
  "id": "abahs-watch",
  "name": "Abah's Watch",
  "description": "Acquired by completing repeatable activities from the Thieves Guild Tip Board…",
  "icon": "https://eso-hub.com/storage/icons/gear_abahswatch_light_head_a.png",
  "link": "https://eso-hub.com/en/fashion-outfits/abahs-watch",
  "parts": [
    {
      "id": "abahs-watch-arm-cops",
      "name": "Abah's Watch Arm Cops",
      "icon": "https://eso-hub.com/storage/icons/gear_abahswatch_medium_shoulders_a.png",
      "link": "https://eso-hub.com/en/fashion-outfits/abahs-watch/abahs-watch-arm-cops"
    }
  ]
}
```

### `collectibles/`, `housing/`, `food-drinks/data/raw.json`

A shared shape. `housing` and `food-drinks` add `quality`:

```json
{
  "id": "alabaster-honey-rum",
  "name": "Alabaster Honey Rum",
  "description": "Increase Health Recovery by 619 for 35 minutes.",
  "icon": "https://eso-hub.com/storage/icons/crafting_spirits_003.png",
  "link": "https://eso-hub.com/en/food-drinks/alcoholic-drinks/alabaster-honey-rum",
  "quality": "Fine",
  "category": "Alcoholic Drinks"
}
```

`quality`: `Normal`, `Fine`, `Superior`, `Epic`, `Legendary`

### `dyes-colors/data/raw.json`

```json
{
  "id": "abyssal-beryl",
  "name": "Abyssal Beryl",
  "color": "rgb(56, 153, 196)",
  "link": "https://eso-hub.com/en/dye/abyssal-beryl",
  "achievement": "Maw of Lorkhaj Completed",
  "achievementLink": "https://eso-hub.com/en/achievements/maw-of-lorkhaj-completed"
}
```

### `leads/data/raw.json`

```json
{
  "id": "ancestral-orc-gloves",
  "name": "Ancestral Orc: Gloves",
  "icon": "https://eso-hub.com/storage/icons/quest_letter_002.png",
  "zone": "Alik'r Desert",
  "item": "Ancestral Orc Gloves\nAncestral Orc Bracers\nAncestral Orc Gauntlets",
  "location": "Treasure Map Chests\n Click here to view on map"
}
```

`item` may list several unlocked items, `\n`-separated.

### `buffs-debuffs/`, `status-effect/`, `traits/`, `zone/data/raw.json`

Flat and small:

```json
{ "id": "empower", "name": "Empower", "description": "Increases the damage of your Heavy Attacks by 70%." }

{ "id": "burning", "name": "Burning", "icon": "https://…/ability_mage_062.png",
  "description": "Target burns for additional damage over 4 seconds." }

{ "id": "divines-armor", "name": "Divines", "icon": "https://…/crafting_accessory_sp_names_001.png",
  "description": "Increases Mundus Stone effects by 9.1%.", "type": "Armor" }

{ "id": "alikr-desert", "name": "Alik'r Desert", "link": "https://eso-hub.com/en/zones/alikr-desert",
  "icon": "https://eso-hub.com/storage/headers/alikr-desert-zone-e-s-o-header--j4gd-u-z.jpg" }
```

`traits.type`: `Armor`, `Weapon`, `Jewelry` (11 each).

---

## Usage

### Node.js

```js
import sets from './sets/data/raw.json' with { type: 'json' };

const mythics = sets.filter(s => s.category === 'Mythic');
console.log(mythics.length, mythics[0].name);

// Local asset paths are derived from the id
const icon = `./sets/icons/${mythics[0].id}.png`;
const tooltip = `./sets/tooltips/${mythics[0].id}.png`;
```

### Python

```python
import json

with open('skills/data/raw.json', encoding='utf-8') as f:
    skills = json.load(f)

ultimates = [s for s in skills if s['type'] == 'Ultimate' and s['category'] == 'Warden']
for s in ultimates:
    print(s['name'], '->', f"skills/icons/{s['id']}.png")
```

### jq

```sh
# All Monster Sets
jq -r '.[] | select(.category == "Monster Set") | .name' sets/data/raw.json

# Flatten the achievement tree
jq '[.[] | .subcategories[] | .achievements[]]' achievements/data/raw.json

# Every Legendary furniture in the Lighting categories
jq -r '.[] | select(.quality == "Legendary" and .category == "Lanterns") | .name' housing/data/raw.json
```

### Fetching a single file

The JSON files are small enough to pull directly without cloning the ~1.5 GB of images:

```sh
curl -O https://raw.githubusercontent.com/alitalipcalikoglu/eso-database/main/sets/data/raw.json
```

---

## Known quirks

Worth knowing before you index this data:

- **`id` is not globally unique in every file.** An entry that belongs to several categories is repeated once per category with the same `id` — e.g. *Shadowghost Guar* appears under `Bipedals`, `Exotic` and `Non-Combat Pets`. Affected: `collectibles` (648 ids), `sets` (81), `housing` (70), `leads` (10). Key on `id + category` if you need uniqueness, or de-duplicate on `id` and collect the categories into an array.
- **`skills` has one genuine `id` collision**: `executioner` is both a Nightblade Assassination passive and a Two Handed active. Use `id + category + subcategory` as the key.
- **13 records in `food-drinks/data/raw.json` contain only a `category` field** — no `id`, `name` or anything else. Filter them out (`'id' in record`). Their 13 icons are still present in `food-drinks/icons/`.
- **`motifs-outfit-styles/tooltips/` covers parts only**, not parent styles. 30 filenames overlap with a style id because that style shares an id with one of its parts.
- **`zone/icons/*.png` are JPEG files** (1600×300 headers), not PNGs.
- **Not every category ships both asset kinds.** `buffs-debuffs` has no images at all, `dyes-colors` has tooltips but no icons, and `leads`, `status-effect`, `traits` and `zone` have icons but no tooltips — check the [overview table](#dataset-overview) before assuming a path exists.
- **Numbers in descriptions are snapshot values** taken at CP160 / level 50 with no gear or champion points applied. Treat them as tooltip text, not as a balance source of truth.
- Descriptions use `\n` for line breaks; several list-like fields (`types`, `bonusses`, `buffs`) are comma-separated strings rather than arrays.

---

## Source & attribution

Game data and images were collected from [ESO-Hub](https://eso-hub.com); the original page is preserved in each record's `link` field, and the tooltip images carry the ESO-Hub watermark.

The Elder Scrolls Online is a trademark of ZeniMax Media Inc. All game content, names, descriptions and artwork belong to ZeniMax Online Studios / Bethesda Softworks. This repository is an unofficial fan project, is not affiliated with or endorsed by ZeniMax or ESO-Hub, and is intended for non-commercial, community use (tools, addons, guides, wikis). If you are the rights holder and want something removed, open an issue.

Data reflects the game state at the time of collection (2025) and is not updated with every patch — verify anything balance-sensitive against the live game.

## Contributing

Issues and pull requests are welcome — corrections to individual records, missing entries, or refreshed data after a patch. Keep the existing file layout and field names so downstream consumers don't break.

## License

The repository's code and data compilation are released under the [MIT License](LICENSE) © 2025 Ali Talip ÇALIKOĞLU. This does not extend to the underlying game content and artwork, which remain the property of their respective rights holders — see [Source & attribution](#source--attribution).
