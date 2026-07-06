# Packages

Reusable TrueWiki packages live here.

Packages should expose focused capabilities and avoid owning full project
workflow. The workflow glue belongs in `apps/`.

Current package:

- `source-cache/` fetches upstream source material and stores immutable raw
  cache records.

Likely future packages:

- `wiki-compiler/` for extraction, normalization, resolution, and static output.
- `contracts/` for shared schemas if package boundaries need them.

