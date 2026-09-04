---
Title: Parallel Content Use Cases
layout: document
Author: Gautier Chomel, Publishing Community Group contributors
date: 02/27/2026
---

# Parallel Content Use Cases

## Purpose

This document gathers concrete use cases for representation mapping in EPUB publications. It is intended to support implementation-focused work on simplifying Multiple-Rendition and related mechanisms.

It complements:

- [Parallel content survey](./parallel-content.md)
- [Explainer — Simplifying EPUB Multiple-Rendition for representation mapping](./multiple-rendition-simplification-explainer.md)
- [Publishing CG plenary minutes (26 February 2026)](../Contributing/Minutes/2026-02-26-publishingcg.html)

## Scope

Included:

- in-document representation mapping,
- switch use cases (reader moves from one representation to another),
- co-presentation use cases (reader consumes two mapped representations together),
- mapping at document, section, fragment, and region granularity.

Excluded for now:

- cross-document synchronization across separate EPUB files,
- UI standardization (the exact controls and layouts remain reading-system decisions).


## Detailed use cases

### UC-01 — Fixed-layout to reflowable equivalence

**Actors:** learner, low-vision reader, educator.

**Scenario:**
1. Reader opens a fixed-layout page designed for visual fidelity.
2. Reader activates “reflow mode”.
3. Reading system opens the corresponding reflowable fragment and preserves logical reading position.

**User need:** retain comprehension and position while gaining text customization (font size, spacing, contrast, line length).

**Required mapping capability:** region or fragment mapping from FXL source to reflowable target.

**Success criteria:**

- Position transfer is deterministic and stable.
- Reader can return to the source representation at equivalent position.
- Reading order remains coherent after switching.

---

### UC-02 — Bilingual side-by-side reading

**Actors:** multilingual readers, students, translators.

**Scenario:**
1. Reader opens the publication with language A as primary.
2. Reader enables language B pane.
3. Reader navigates sentence-by-sentence while both languages stay aligned.

**User need:** compare equivalent content in two languages without losing alignment.

**Required mapping capability:** sentence/paragraph-level mapping between language streams.

**Success criteria:**

- Navigation in one pane updates the aligned location in the other pane.
- Reader can change primary language without losing current logical position.
- Unmapped segments are surfaced predictably.

---

### UC-03 — Braille and text/TTS publication

**Actors:** braille readers, accessibility producers, libraries for blind readers.

**Scenario:**
1. Reader opens pre-converted braille representation.
2. Reader switches to text or TTS for a section.
3. Reader returns to braille at equivalent location.

**User need:** high-quality braille workflows with lossless switching to other accessible representations.

**Required mapping capability:** document-level equivalence minimum; fragment-level mapping preferred.

**Success criteria:**

- Switching does not require unreliable back-conversion.
- Equivalent location is maintained across modes.
- Metadata clearly identifies modality and language.

---

### UC-04 — Newspaper replica and article view

**Actors:** general readers, press publishers.

**Scenario:**
1. Reader taps a region in page replica view.
2. Reading system opens mapped article view.
3. Reader uses “show on page” to return to the originating region.

**User need:** fast bidirectional navigation between visual layout and readable article flow.

**Required mapping capability:** region-to-article mapping, bidirectional links.

**Success criteria:**

- Region hit-testing maps to correct article.
- Return navigation restores page and viewport focus.
- Multi-region continuation (article spans pages) is supported.

---

### UC-05 — Comics panel, text, and audio alignment

**Actors:** comic readers, accessibility users, publishers.

**Scenario:**
1. Reader navigates panel-by-panel in visual layout.
2. Reading system exposes aligned text alternative and optional narration.
3. Reader switches between guided and free navigation.

**User need:** preserve visual storytelling while enabling accessible alternatives.

**Required mapping capability:** panel/bubble region mapping plus optional timed synchronization.

**Success criteria:**

- Panel selection resolves to correct text/audio equivalents.
- Guided navigation follows authored sequence.
- Fallback behavior is defined when only partial mapping exists.

---

### UC-06 — Sign-language video with text

**Actors:** Deaf and hard-of-hearing readers, educational publishers.

**Scenario:**
1. Reader plays sign-language video alongside text.
2. Text highlighting and video segments remain aligned.
3. Reader jumps to a text fragment and video seeks to mapped segment.

**User need:** synchronized multimodal reading experience.

**Required mapping capability:** fragment mapping with timed segment alignment.

**Success criteria:**

- Seeking in either stream preserves synchronization.
- Drift handling is predictable.

---

### UC-07 — Education: accessibility and mobile readability

**Actors:** students (including those with dyslexia or low vision), teachers, educational publishers.

**Scenario:**
1. Reader opens a textbook spread optimized for large screens (fixed-layout).
2. Reader activates a reflowable or accessible mode to change font (e.g., OpenDyslexic) and use visual helpers like reading rules or syllabus coloration.
3. On small viewports, the reader uses the same mapping to ensure readability.

**User need:** preserve pedagogical structure while enabling accessibility adjustments (custom fonts, reading aids) and mobile readability.

**Required mapping capability:** spread/region mapping to section/fragment targets.

**Success criteria:**

- Students can apply dyslexia-friendly fonts and visual aids without losing their place.
- Small-screen usability improves without content loss.
- Classroom workflows remain consistent across devices and individual accessibility needs.

---

### UC-08 — Language-learning aligned readers

**Actors:** language learners and instructors.

**Scenario:**
1. Reader follows source text sentence-by-sentence.
2. Reader reveals mapped translation only on demand.
3. Reader can pin both streams for side-by-side study.

**User need:** controlled reveal and comparison to support learning.

**Required mapping capability:** sentence-level mapping with optional one-to-many alignment.

**Success criteria:**

- Reveal/hide actions preserve alignment state.
- One-to-many mappings are represented without ambiguity.
- Reader progress tracking remains representation-independent.
