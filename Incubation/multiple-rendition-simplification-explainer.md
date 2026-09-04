---
Title: Explainer — Simplifying EPUB Multiple-Rendition for representation mapping
layout: document
Author: Gautier Chomel, Publishing Community Group contributors
date: 02/26/2026
---

# Explainer — Simplifying EPUB Multiple-Rendition for representation mapping

## Participate

- Discussion and issues: https://github.com/w3c/publishingcg/issues
- Related EPUB WG discussion: https://github.com/w3c/epub-specs/discussions/2829

## Introduction

This explainer proposes a focused simplification path for EPUB Multiple-Rendition, centered on one practical need: mapping equivalent or complementary representations of the same logical content so reading systems can either switch between them or present them together.

The goal is not to replace existing EPUB mechanisms, but to define a lighter, implementable profile that preserves core value (multiple representations + mapping) while reducing packaging and processing complexity that limited adoption of the original approach.

## User-facing problem

Readers need reliable ways to navigate equivalent content across different representations:

- bilingual or multilingual reading (side-by-side or quick switch),
- fixed-layout to reflowable switch for accessibility,
- text and braille equivalents,
- text with synchronized audio or video streams,
- region-based visual content with textual alternatives.

Today, many products implement this in proprietary ways. The result is fragmented behavior and weak interoperability for users and publishers.

### Goals

- Define a simple interoperable model for representation mapping.
- Support both modes:
  - consume together (parallel/synchronized presentation),
  - switch between equivalents (alternate presentations).
- Support multiple mapping granularities (document, section, sentence, region).
- Reuse as much of existing EPUB infrastructure as possible.
- Lower implementation and authoring burden versus legacy Multiple-Rendition practice.

### Non-goals

- Defining reading-system UI requirements (buttons, split views, menus).
- Solving all cross-document synchronization scenarios in this first step.
- Replacing Media Overlays timing semantics.
- Introducing a fully new packaging model unrelated to EPUB.

## Evidence and implementation reality

The Publishing CG plenary discussion on 26 February 2026 confirmed active demand in education, accessibility, multilingual markets, comics, and newspaper workflows.

Meeting participants reported that:

- mapping is the core technical need across many use cases,
- current behavior is mostly app-specific,
- Multiple-Rendition is valuable conceptually but perceived as difficult to deploy in practice,
- implementation-focused simplification is now more important than purely conceptual incubation.

[Minutes](../Contributing/Minutes/2026-02-26-publishingcg.html)

## Motivating use cases

### 1. Fixed-layout + reflowable equivalence

A learner opens a designed textbook spread (FXL) and switches to a reflowable version for custom text settings (font, spacing, contrast, zoom).

Need:
- one logical location mapped between the FXL region and reflowable fragment,
- predictable switch without losing reading position.

### 2. Bilingual side-by-side reading

A reader compares source and translation sentence-by-sentence.

Need:
- mapping at sentence/paragraph level,
- ability to show both panes or switch primary language quickly.

### 3. Braille + text/TTS publication

A user reads pre-converted braille and switches to text/TTS as needed; in some contexts, concurrent use is also valuable.

Need:
- representation-level equivalence,
- optional finer-grained mapping,
- no lossy back-conversion requirement.

### 4. Newspaper replica + article view

A user taps a region in a PDF-like page and jumps to reflowed article text; from article mode, user can return to page region.

Need:
- region-to-article mapping,
- bidirectional navigation.

## Why previous Multiple-Rendition adoption was limited

Based on implementer experience shared in EPUB and Publishing CG discussions, the main blockers were:

1. **Packaging complexity**
   - Multiple OPFs and container-level orchestration were perceived as heavy for common workflows.

2. **Metadata and processing ambiguity**
   - Tooling and implementers faced uncertainty around metadata scope, duplication, and processing expectations.

3. **Mapping model friction**
   - The mechanism existed conceptually, but practical authoring and implementation guidance for granular mapping remained insufficient.

4. **Implementation economics**
   - Reading systems prioritized other roadmap items; without clear minimal profile and interoperability targets, adoption stalled.

5. **Chicken-and-egg standardization pressure**
   - Limited independent implementations reduced momentum for broader conformance investment.

These are ecosystem and product-delivery constraints, not evidence that the underlying user need disappeared.

## Proposed approach

Define a **Simplified Representation Mapping Profile** that keeps the useful parts of Multiple-Rendition while reducing surface area.

### Core model

- One publication package may contain multiple representations of the same logical work.
- Each representation has clear metadata (language, modality, layout, accessibility characteristics).
- A mapping resource links equivalent targets across representations.

### Minimal conformance surface

- Required:
  - representation identifiers,
  - at least document-level mapping,
  - deterministic reading-position transfer rules.
- Optional:
  - finer granularity (fragment/region),
  - synchronized co-presentation hooks.

### Mapping format direction

The profile should evaluate one primary mapping syntax and keep authoring complexity low. Candidate serializations can include HTML, XML, or JSON, but interoperability should converge on a constrained profile rather than multiple fully-equivalent syntaxes.

### Relationship to existing EPUB mechanisms

- Reuse existing EPUB packaging and manifest principles where possible.
- Preserve compatibility with Media Overlays for timed synchronization cases.
- Allow staged adoption: switch use cases first, co-presentation next.

## Alternatives considered

### A. Keep current Multiple-Rendition unchanged

Pros:
- no new work required.

Cons:
- does not address known implementation friction;
- unlikely to improve adoption quickly.

### B. Use only Media Overlays for all mapping

Pros:
- existing timing model and some tooling.

Cons:
- optimized for temporal synchronization, not general equivalence mapping;
- awkward fit for many non-timed switch scenarios.

### C. Proprietary app-level mapping only

Pros:
- immediate product flexibility.

Cons:
- continued ecosystem fragmentation;
- no interoperable baseline for users and publishers.

## Accessibility, internationalization, privacy, security

### Accessibility

This work directly solve known accessible reading frictions by enabling robust switching and/or co-presentation between equivalent formats (e.g., reflow, braille, audio/video).

### Internationalization

This work improves multilingual reading by enabling explicit language-to-language mapping and aligned navigation.

### Privacy and security

No new network-facing or high-risk primitives are proposed in this explainer. Privacy and security impact should remain low if mapping remains publication-local.

## Stakeholder feedback / opposition

Current signal from discussions:

- Positive on user need and use-case urgency.
- Positive on mapping as the core abstraction.
- Concern that prior Multiple-Rendition packaging/process complexity hindered deployment.
- Open question on optimal mapping syntax and minimum profile scope.

## Open questions

- What is the smallest profile that yields two independent interoperable implementations?
- Which mapping granularity must be mandatory for initial interoperability?

## Next steps

1. Build a use-case/requirements matrix from current implementations.
2. Propose a constrained profile draft and processing rules.
3. Prototype in at least two reading systems.
4. Validate authoring workflows with publishers and accessibility producers.

## References and acknowledgements

- Publishing CG minutes (26 February 2026): ../Contributing/Minutes/2026-02-26-publishingcg.html
- Parallel content survey draft: ./parallel-content.md
- EPUB WG discussion #2829: https://github.com/w3c/epub-specs/discussions/2829
- EPUB Multiple-Rendition Publications 1.1: https://www.w3.org/TR/epub-multi-rend-11/
- Writing Effective Explainers: https://www.w3.org/TR/explainer-explainer/

Thanks to participants in the Publishing Community Group and EPUB ecosystem discussions for the use cases and implementation feedback.
