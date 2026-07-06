# TrueWiki Goal

TrueWiki is a local, provenance-aware wiki compiler for heavily modded game
modpacks.

The first target is a Terraria/tModLoader modpack, specifically the
Infernum-style pack Infernal Eclipse of Ragnarok. The project should not become
Terraria-only: the broader problem is resolving documentation for any modded
game where sources are layered on top of each other.

## Core Promise

TrueWiki should answer one question first:

> What is true in this specific modpack?

The goal is not to mirror upstream wikis. Source wikis, changelogs, game data,
and local notes are inputs. The generated local wiki is compiled output.

Existing wikis usually describe individual mods in isolation. A real modpack can
combine base content, compatibility layers, overhaul modes, config changes, and
pack-specific notes. Most mods do not globally override each other; the useful
work is to classify facts precisely enough that simple scoped rules can decide
which claim applies. TrueWiki should merge and resolve those inputs into a
coherent local wiki for the installed pack.

The source model should usually be hierarchical. A mod or source is often built
on top of another source: vanilla -> Calamity -> Infernum -> modpack notes. The
common case is not "mod X overrides part of mod Y while mod Y also overrides
part of mod X." The common case is that a higher layer provides more specific
truth for some claim scopes.

## Target Sources

Prefer wiki.gg and official or mod-maintained documentation. Avoid Fandom unless
there is no better source.

Initial source candidates include:

- Infernal Eclipse of Ragnarok wiki
- Calamity Mod wiki
- Calamity Infernum wiki
- Thorium Mod wiki
- Thorium Crossmod wiki
- Secrets of the Shadows wiki
- Consolaria wiki
- Ragnarok Mod wiki

Wiki scraping is enough for an early prototype, but it is not enough forever.
High-trust structured facts such as recipes, item stats, shops, drops, shimmer
transformations, NPC data, and progression gates may eventually need to come
from the actual loaded tModLoader pack or another authoritative game-data
export.

## Product Scope

The final wiki should be browsable like a normal wiki, with pages for gameplay
topics such as:

- bosses, enemies, NPCs, shops, drops, and loot tables
- items, weapons, armor, accessories, materials, and crafting stations
- recipes, crafting trees, shimmer transformations, and acquisition paths
- biomes, events, buffs, debuffs, mechanics, and difficulty modes
- progression stages, class setups, role-specific strategy, and multiplayer notes

Correctness matters more than coverage. A small set of trustworthy resolved
pages is more valuable than a large generated wiki full of subtle errors.

## Compiler Model

The intended flow is:

```text
source pages and data
-> raw cached records
-> extracted source fragments
-> structured claims, classifications, and effect candidates
-> resolution using authority rules and context
-> generated local wiki pages with provenance
```

Source pages should not be pasted directly into final pages. They should first
be converted into structured records that the resolver can compare, rank,
reject, or mark uncertain.

The resolver, candidate review loop, provenance model, and conflict handling are
the core product. Scraping is ingestion. The local website is presentation.

## Resolution Requirements

A generated page should be able to explain:

- which sources contributed to the result
- which source won when sources conflicted
- why that source won
- whether a claim is verified, inferred, generated, curated, or uncertain
- whether an apparent conflict is a contradiction or a context-specific override
- whether a recommendation is actually available at the relevant progression point

Resolution should be driven by scoped authority rules over classified claims and
a mostly hierarchical source stack. Rules should usually be simple once
classification is good, for example:

```text
for boss_behavior claims, infernum_wiki outranks calamity_wiki when Infernum is enabled
for recipe claims, game_data outranks wiki text
for reviewed candidates, human review fixes LLM-generated classification before resolution
```

Different fact categories need different authority rules. Initial defaults:

- Human review can approve, reject, correct, or pin LLM-generated candidates.
- Reviewed classifications and candidate corrections outrank generated
  classifications.
- Explicit local corrections can assert pack-specific truth when source material
  is missing or wrong.
- Modpack-specific documentation generally outranks individual mod docs for
  claims in the same scope.
- Addon, overhaul, and compatibility sources generally outrank the sources they
  are built on for the claim scopes they explicitly cover.
- Infernum-specific boss behavior generally outranks base Calamity boss
  behavior when Infernum is active.
- Extracted game data, if added later, generally supersedes wiki text for
  structured facts such as recipes, drops, shops, and item stats.
- Strategy and class setup advice should be constrained by real progression
  availability.
- Generated synthesis should never be treated like verified structured data.

Manual curation is part of the baseline, not a failure. Its main job is to
control generated structure: fix bad classifications, reject bad LLM candidates,
approve useful candidates, and pin decisions that should survive rebuilds.
Curated corrections, local notes, suppressions, and pinned decisions must survive
future source updates until a maintainer explicitly changes them.

## LLM Use

LLMs may help with constrained semantic extraction and classification. Their
main job should be to read messy prose and turn it into structured candidate
records, for example:

- page kind and entity classification
- claim candidates from prose
- conditions such as difficulty mode, enabled mod, biome, or progression stage
- effect candidates such as recipe removal, progression movement, or shop change
- stale or context-limited source claims

LLMs should output structured candidates with provenance. They should not
directly decide final truth, choose winning sources, or write polished final
pages from source text.

The review UI should make those candidates inspectable. A maintainer should be
able to accept, edit, reject, or pin LLM-produced classifications and effect
candidates before they become trusted normalized records.

Avoid this failure mode:

```text
source pages -> LLM -> polished generated page
```

Prefer this:

```text
source pages -> structured candidates -> validated claims -> rule resolver -> audited page
```

## Watch And Rebuild

Source watching and targeted rebuild planning are stretch goals, but the design
should leave room for them.

`Watch sources` should monitor upstream wiki revisions, changelogs, game-data
exports, and modpack metadata. It should produce changed source references for
fetching; it should not decide final truth.

The rebuild planner should map changed records or claims to affected local pages
and review items. It should not blindly replace reviewed classifications,
curated candidate decisions, or explicit local corrections.

Important behavior:

- upstream changes must not automatically override manual curation
- changes touching reviewed candidates or curated topics should create review items
- source freshness and revision information should be preserved
- generated pages should be reproducible from source data, authority rules, and
  reviewed candidate decisions
- local builds should be stable rather than constantly mutating trusted pages

## Failure Modes To Avoid

The project should actively avoid:

- generating pages that look authoritative but are subtly wrong
- merging entities only by display name
- treating all source wikis as equally authoritative
- treating whole mods as global overrides when only specific claim types differ
- modeling source authority as mutual patching when the real relationship is a
  mostly one-way hierarchy
- averaging contradictory claims instead of applying scoped authority rules
- losing source provenance
- allowing upstream updates to erase local corrections
- recommending gear or strategies unavailable in the actual pack progression
- confusing base mod behavior with Infernum or modpack behavior
- hiding uncertainty
- producing broad coverage before the generated information is trustworthy

## Working Principles

- Correctness over coverage.
- Provenance over polish.
- Explicit uncertainty over invented certainty.
- Curated truth over blind automation.
- Reproducibility over one-off generated pages.
- Modpack-specific truth over generic wiki aggregation.
- Semantic resolution over text concatenation.

## First Milestone

Start with a narrow vertical slice. One good target is a single boss or item
page that goes through the full path:

```text
source
-> raw source record
-> source fragment
-> claim
-> resolved claim
-> resolved page model
-> compiled page
```

The architecture should stay flexible until one or two real page kinds prove the
data shapes.
