# Terraria wiki page kinds

This is a working development taxonomy for the local wiki compiler. The point is
to implement and verify one page kind at a time, then split each kind into
overwriteable source fragments and normalized claims with provenance.

Sources checked:

- Vanilla Terraria Wiki: `https://terraria.wiki.gg`
- Calamity Mod Wiki: `https://calamitymod.wiki.gg`
- Thorium Mod Wiki: `https://thoriummod.wiki.gg`
- Infernum Mod Wiki: `https://infernummod.wiki.gg`
- Ragnarok Wiki: `https://ragnarokmod.wiki.gg`

The categories below come from live wiki.gg MediaWiki category trees and Cargo
table discovery. Ragnarok currently exposes little content taxonomy through
categories, so it will need page sampling/manual mapping later.

Canonical page/fact kind keys:

- `boss`
- `enemy_npc`
- `friendly_npc_vendor`
- `critter`
- `item`
- `recipe`
- `crafting_station`
- `drop_loot_table`
- `shop`
- `buff_debuff_empowerment`
- `class_role`
- `class_setup_progression`
- `biome_environment_structure_layer`
- `event_weather`
- `mechanic_system`
- `quest_achievement_bestiary`
- `history_version_status`
- `media_support`

## High-level page kinds to implement

### Boss pages

Treat bosses as a first-class page kind, even though upstream wikis categorize
them under enemy NPCs.

Source cues:

- `Category:Boss NPCs`
- `Category:Boss Part NPCs`
- `Category:Boss Summoning NPCs`
- `Category:Miniboss NPCs` / `Category:Mini-boss NPCs`
- boss strategy guide pages named like `Guide:<boss> strategies`

Fragment/claim candidates:

- identity: canonical title, aliases, source mod, entity ids
- progression: tier, required prior bosses, biome/time/event requirements
- summon/access: natural spawn, summon item, arena/location, despawn rules
- stats: life, damage, defense, DR, knockback, modes, phases, parts
- AI/behavior: attacks, phase thresholds, movement, special mechanics
- difficulty overrides: Expert/Master/Revengeance/Death/Infernum/etc.
- drops: normal/expert/master/mode-specific loot, trophy/relic/mask/pet
- NPC/shop/unlocks: town NPC changes, world state changes, recipe unlocks
- strategies: arena, gear, class-specific advice, multiplayer notes
- provenance/conflicts: base boss vs Infernum or modpack-specific behavior

### Enemy NPC pages

Non-boss hostile NPCs, including segmented/part NPCs and projectile NPCs when
they have pages.

Source cues:

- `Category:Enemy NPCs`
- `Category:Aquatic NPCs`
- `Category:Burrowing Enemy NPCs`
- `Category:Flying Enemy NPCs`
- `Category:Projectile NPCs`
- `Category:Slime NPCs`
- `Category:Undead Enemy NPCs`
- `Category:Enemy NPCs by environment`
- `Category:Enemy NPCs by AI type`
- `Category:Unspawnable NPCs`

Fragment/claim candidates:

- identity and source mod
- spawn conditions: biome, layer, time, event, progression, rarity
- stats and mode variants
- AI/behavior and attacks
- immunities and debuff interactions
- drops and banners
- bestiary/flavor text where present
- related entities: summons, projectiles, variants, boss parts

### Friendly NPC and vendor pages

Town NPCs, friendly NPCs, pets/utility NPCs, and NPC-like furniture pages.

Source cues:

- `Category:NPC NPCs`
- `Category:Furniture NPCs`
- vendor/shop sections on NPC pages
- happiness/shop/progression sections on vanilla NPC pages

Fragment/claim candidates:

- identity and arrival/spawn requirements
- housing and happiness/preferences
- shop inventory by condition/progression/biome/time
- services, dialogue, quests, attack stats
- death/respawn behavior
- modpack shop overrides

### Critter pages

Passive catchable or ambient NPCs.

Source cues:

- `Category:Critter NPCs`
- bait/fishing/catching sections

Fragment/claim candidates:

- spawn conditions
- catchability, bait power, item conversion
- use in recipes, quests, cages, displays
- bestiary text

### Item pages

This is the largest page kind. Implement item extraction generically, then add
sub-kind handlers for weapons, armor, accessories, placeables, consumables, and
materials.

Common source cues:

- `Category:Items`
- item infobox templates
- vanilla Cargo table `Items`
- recipe/drop/shop categories and sections

Common fragment/claim candidates:

- identity: canonical item name, aliases, item id/internal name, source mod
- classification: item type, class, rarity, value, stack, research count
- acquisition: recipes, drops, shops, fishing, worldgen, shimmer, gifts, loot
- progression availability and gating
- crafting usage: used in recipes, crafting tree, station requirements
- tooltips/flavor
- history/version changes

Important item sub-kinds:

- weapons: melee, ranged, magic, summon, rogue, classless, throwing, radiant,
  symphonic, true-damage, misc
- weapon subtypes: swords, spears, flails, yoyos, boomerangs, bows, guns,
  launchers, repeaters, spellbooks, magic guns, staves, wands, minions, sentries,
  whips, bombs, daggers, javelins, spiky balls
- armor: armor pieces, armor sets, set bonuses, vanity, developer/donator sets
- accessories: combat, mobility, defense, information, immunity, shield/boots
- tools: pickaxes, drills, axes, chainsaws, hammers, fishing poles, hooks,
  mounts/minecarts where represented as items
- ammo: arrows, bullets, rockets, darts, consumable projectiles
- consumables: potions, food/drink, healing/mana, permanent upgrades, grab bags,
  treasure bags, crates, boss/event summon items, keys
- materials: ores, bars, souls, fragments, essences, crafting materials
- placeables: blocks, walls, furniture, crafting stations, mechanisms, storage,
  light sources, paints, seeds, plants
- special/status buckets: challenge items, dedicated items, fished items,
  vendor items, bag-loot items, unobtainable items, historical/removed items

### Recipe pages and crafting trees

Recipes are often structured data rather than standalone article pages, but they
need their own resolver because they are heavily overwritten in modpacks.

Source cues:

- `Category:Recipes`
- Cargo table `Recipes`
- Calamity/Thorium pages named `<item>/Crafting tree`
- categories like `Category:Crafting trees`
- categories like `Category:Items crafted at`

Fragment/claim candidates:

- recipe result and amount
- ingredients and quantities
- crafting station
- version/history flags
- recipe groups/substitutions
- progression gating
- source priority: game data > modpack curation > wiki Cargo/text
- conflict handling for recipe additions, removals, and replacements

### Crafting station pages

Stations are item pages, but they also act as recipe indexes and progression
gates.

Source cues:

- `Category:Crafting station items`
- recipe station fields in Cargo
- crafting station sections on item pages

Fragment/claim candidates:

- station identity and placement
- acquisition
- recipes enabled
- station equivalence/substitution
- progression tier

### Drop and loot-table fragments/claims

Drops usually live as fragments or claims on boss/NPC/item pages, not as
standalone pages.
Keep a dedicated fact kind anyway.

Source cues:

- Cargo table `Drops`
- drop sections on NPC/boss pages
- grab bag/treasure bag/crate pages
- `Category:Drop items`, `Category:Bag loot items`, `Category:Grab bag items`

Fragment/claim candidates:

- source entity/container
- item, quantity, chance
- mode/progression/player-count conditions
- first-kill or one-time flags
- expert/master/revengeance/death/infernum variants

### Shop fragments/claims

Shops are normally fragments or claims on NPC pages, but modpacks frequently
override them.

Source cues:

- shop sections on town NPC/vendor pages
- vendor item categories
- item acquisition text

Fragment/claim candidates:

- vendor
- item, price/currency, stack/limit
- conditions: progression, biome, time, moon phase, happiness, events
- additions/removals/replacements from mods or modpack curation

### Buff, debuff, and empowerment pages

Thorium adds Empowerments as a distinct mechanic family.

Source cues:

- `Category:Buffs`
- `Category:Debuffs`
- `Category:Empowerments`
- buff/debuff infobox templates

Fragment/claim candidates:

- effect text and exact numeric effects
- duration and source items/NPCs/projectiles
- immunities/resistances
- stacking and cancellation rules
- class or mode interactions

### Class and role pages

These pages define playable build roles, especially for modded classes.

Source cues:

- `Category:Classes`
- Thorium class categories: `Bard`, `Healer`, `Thrower`
- Calamity class setup data: melee, ranged, magic, summoner, rogue
- guide pages like `Guide:Bard`, `Guide:Healer`, `Guide:Class setups`

Fragment/claim candidates:

- class definition and damage/resource mechanics
- available weapons/armor/accessories by stage
- class-specific buffs/resources
- crossmod support and gaps
- progression-stage recommendations

### Class setup and progression guide pages

These are recommendation pages, not source-of-truth item pages. They must be
validated against actual availability.

Source cues:

- `Guide:Class setups`
- Calamity subpages: `Guide:Class setups/Pre-Hardmode`, `/Hardmode`,
  `/Post-Moon Lord`
- Calamity Cargo table `ClassSetups`
- Thorium class guide pages
- pages named `Guide:Mod progression`, `Guide:Game progression`, walkthroughs

Fragment/claim candidates:

- progression stage
- class/role
- recommended weapons, armor, accessories, buffs, mounts, tools
- availability proof for each recommendation
- annotations: optional, difficult to obtain, upgrade path, multiplayer role
- conflict handling between vanilla/mod/modpack advice

### Biome, environment, structure, and layer pages

Includes natural biomes, generated structures, layers, micro-biomes, and
modded regions.

Source cues:

- `Category:Environments`
- pages such as vanilla `Biomes`, `Layers`, `Dungeon`, `The Underworld`
- Calamity pages such as `Abyss`, `Sulphurous Sea`, `Sunken Sea`,
  `Brimstone Crag`, `Astral Infection`, `Planetoids`

Fragment/claim candidates:

- biome identity and detection rules
- generation/location/layer
- enemies, critters, bosses, events
- drops/resources/fishing/shops
- hazards and mechanics
- progression changes and world-state transformations

### Event and weather pages

Includes invasions, moon events, seasonal events, weather, and special
encounters.

Source cues:

- `Category:Events`
- `Category:Random events`
- `Category:Seasonal events`
- `Category:Summoned events`
- environment pages like Blood Moon, Rain, Sandstorm, Lunar Events

Fragment/claim candidates:

- trigger/spawn conditions
- event progression and waves
- enemies/bosses
- drops/shops/unlocks
- strategy and arenas
- mode/modpack overrides

### Mechanics and system pages

Pages that explain rules rather than a single entity.

Source cues:

- `Category:Game mechanics`
- vanilla subcategories: Buffs, Debuffs, Liquids, Modifiers, Recipes,
  Secret/Special world seeds, Data IDs
- Calamity mechanics like Rage Meter, Adrenaline Meter, difficulty-mode content
- Thorium mechanics like Classes and Empowerments

Fragment/claim candidates:

- rule definition
- exact formulas/values
- affected entities/items
- mode/config/progression conditions
- source authority rules

Specific mechanic families to expect:

- difficulty/mode mechanics: Expert, Master, Revengeance, Death, Malice,
  Infernum, boss rush or challenge modes where present
- resource meters: rage, adrenaline, bard/healer resources, empowerment systems
- modifiers/reforging/rarity/value
- liquids, shimmer/transmutations, fishing, luck, spawn rates
- world seeds and special world rules
- data IDs/internal names

### Quest, achievement, and bestiary fragments/claims

These are lower priority for the first modpack resolver, but they exist in the
vanilla ecosystem.

Source cues:

- `Category:Achievement-related elements`
- `Category:Quest rewards`
- bestiary sections/images/filter categories
- Angler quest pages/sections

Fragment/claim candidates:

- requirement/trigger
- reward/unlock
- related item/NPC/biome
- source-game version

### History, version, removed, and exclusive-content pages

These are usually provenance/context, not final resolved gameplay truth.

Source cues:

- `Category:History`
- `Category:Versions`
- `Category:Version content`
- `Category:Historical content`
- `Category:Unobtainable content`
- `Category:Exclusive content`
- `Category:Legacy platform differences`
- Calamity `History` namespace

Fragment/claim candidates:

- version introduced/changed/removed
- platform/mode/source restrictions
- stale-content warning
- changelog links
- effect on current modpack truth

### Media, sound, file, template, module, help, and maintenance pages

These are not gameplay page kinds for early resolver work.

Keep them in ingestion only as supporting material:

- images and audio for presentation/provenance
- templates/modules for extracting infobox semantics
- categories for page-kind detection
- redirects/disambiguations for title resolution

## Suggested implementation order

1. Boss pages
2. Generic item pages, starting with weapons/accessories/armor
3. Recipes and crafting trees
4. Enemy NPC pages
5. Drops and loot tables
6. Friendly NPC shops
7. Buffs/debuffs/empowerments
8. Class setups and progression pages
9. Biomes/environments/events
10. Mechanics/modes
11. Quests/achievements/bestiary
12. History/version/exclusive/removed content

This order follows risk and usefulness: bosses, items, recipes, drops, and shops
are the highest-value places where modpack overrides will be visible and costly
if wrong.

## Notes for extractor design

- Do not merge entities by display name alone. Use source wiki, page title,
  namespaces, categories, infobox type, ids, and redirects.
- Page kind detection should allow multiple labels. Example: a boss page is both
  `boss` and `enemy_npc`; a crafting station is both `item` and
  `crafting_station`.
- Some important facts are claims, not page kinds. Recipes, drops, shops, and
  class setup rows need independent provenance and override rules.
- Mode-specific categories are not page kinds by themselves. They are conditions
  or overlays on boss/item/NPC/mechanic facts.
- The generated local wiki can expose different page kinds than upstream wikis.
  For example, a resolved local boss page may combine upstream boss, NPC, drops,
  recipes, guide, and mode pages into one audited output page.
