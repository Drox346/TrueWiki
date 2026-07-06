# Implementation order

The project should start with data-shape research, but the architecture should
not be fully locked before a real vertical slice exists.

The goal is to make the system concrete enough to build against, then let real
page kinds harden the data shapes and interfaces.

## Recommended order

1. Research the data shapes needed for the first target page kinds.
2. Define a small first version of the core data shapes.
3. Sketch the architecture and component interfaces around those shapes.
4. Add glue code and temporary mocks so the whole pipeline can run end to end.
5. Build one real vertical slice, such as one boss page or one item page.
6. Update the data shapes and interfaces based on what the slice proves.
7. Repeat with more vertical slices, one page kind at a time.

## Important rule

Do not fully lock the architecture or interfaces before a real slice works.

Treat them as versioned drafts until at least one or two real page kinds have
gone through the full flow:

```text
source
-> source record
-> fragment
-> claim
-> resolved claim
-> resolved page model
-> compiled page
```

## Why

The first real extractor and resolver will reveal details that are hard to see
from diagrams alone:

- which fields are actually needed
- which page kinds share structure
- where fragments are too source-shaped
- where claims are too vague or too specific
- where prose should become effect candidates instead of ordinary claims
- which interfaces are useful and which are ceremony
- where LLM-assisted classification and manual candidate review need to fit

The architecture should become stricter as vertical slices prove it, not before.
