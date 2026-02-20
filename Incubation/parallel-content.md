---
Title: Parallel Content
layout: document
Author: Gautier Chomel
date: 02/06/2026
---

# Parallel Content 

## Abstract

This document surveys "parallel content" in EPUB and related publishing workflows, with the goal of clarifying the problem space and identifying practical directions for future standardization or guidance. It focuses on in-document parallels (multiple representations of the same logical content within one publication) and summarizes current mechanisms, their limits, and common implementation workarounds. It does not define new requirements or specifications, and it does not address cross-document synchronization beyond a brief scoping mention.

## Introduction and scope

Multiple discussions in the EPUB Working Group and related projects have highlighted the need to better support “parallel content”: situations where the same logical content is made available in multiple representations that users may want to view side‑by‑side or switch between. 

Use cases include:

- Same content in two languages displayed side‑by‑side. 
- Same content in two media types, such as images and audio or images and text. 
- Same content in two layouts, such as fixed‑layout and reflow. 
- Parallel reading across separate publications, such as two editions or two translations read in synchrony. 

Previous attempts to address these needs as “synchronised content”, “alternate content”, or “fallback” mechanisms did not cover the full range of scenarios. This document surveys existing mechanisms, analyses their strengths and limitations, and proposes directions for future work.

For clarity, we distinguish two categories, only the first one is addressed here: 

- **In‑document parallels**: multiple representations of the same logical content within one publication (e.g., two languages side by side; text + image; image + audio; text + video; fixed + reflow; etc.). 
- **Cross‑document parallels**: two or more separate EPUBs (or items in the same EPUB container) that need synchronised navigation for comparison (e.g., two editions, two translations, article vs preprint). 

## State of the art in and around EPUB

### Existing mechanisms

This section lists problems, and woraround pratcices that are relevant to parallel content, even if they were not designed for that purpose.

#### Contents one after the other

On the authoring side, bilingual EPUBs often embed both languages one after the other without ways to consult them side by side as it is usual in printed editions.  

#### Reflowable versus fixed‑layout publications

EPUB 3.3 defines reflowable content documents as the default model for text‑centric EPUBs, with layout controlled primarily through HTML and CSS. Fixed‑layout (FXL) EPUBs use explicit viewport dimensions and positioning metadata to preserve a page design, and are described in the EPUB 3 Fixed‑Layout Documents note. 

The “EPUB Accessibility – Fixed Layout Challenges and Best Practices” note explains that FXL is commonly used where layout is integral to meaning but also highlights significant accessibility challenges (zoom, reflow, reading order, user overrides). These properties make FXL attractive for visual parallel content (e.g., designed side‑by‑side pages) but problematic from an accessibility standpoint. 

Still one accessibility issue remains witch is the one related to ability to change font family, size, spacing as well as background and text colors. This is a critical issue for persons with low vision or cognitive disabilities. 

Different solutions have been proposed to allow a way to adapt text and layout within a fixed layout EPUB but from a Reading System perspective it will not be possible to implement all solutions. 

This issue tries to list different proposals to allow reflowable text into fixed layout and estimate which ones are the most susceptible to be used in an industrial context. 

#### Manifest [Fallbacks](https://www.w3.org/TR/epub-33/#sec-resource-fallbacks) and alternate resources

EPUB 3.3 and later allow manifest items to declare a `fallback` attribute pointing to another resource, forming a chain used when the primary resource is unsupported. This mechanism is intended for format compatibility and “foreign” resources (e.g., scripted content, non‑HTML media), not explicitly for user‑facing parallel representations. 

Although fallbacks can model “alternate” versions of a resource, the semantics are that the fallback replaces an unsupported primary resource rather than that both are peer representations the user can choose between. Reading systems rarely surface fallbacks as user‑selectable options. 

#### [Multiple Rendition](https://www.w3.org/TR/epub-multi-rend-11/#container) Publications

The EPUB 3 Multiple‑Rendition Publications 1.1 specification defines how a single EPUB package can contain more than one rendition (e.g., fixed and reflow, or multiple languages), with rules to select a rendition at reading time. This supports bundling parallel renditions of the same logical publication inside one container. 

Multiple renditions were removed from the core EPUB 3.3 specification due to limited implementation but retained as a separate specification, with explicit acknowledgement that they may be important for accessibility use cases such as those arising from the European Accessibility Act.

#### [Hybrid](https://idpf.org/charters/2012/layout/ahl.html)
The Hybrid Layout charter explored ways to combine fixed and reflowable presentation models. It is relevant as an early attempt to reconcile visual design with user‑controlled reflow, which is a recurring requirement for parallel content in mixed‑media or mixed‑layout publications.

#### [Guided navigation](https://github.com/readium/architecture/pull/181)
Guided navigation proposes a model to describe a guided or authored reading path through complex layouts (e.g., comics or magazines). This can complement parallel content by providing an explicit navigation sequence that aligns with alternate representations.

#### [Visual to textual ](https://github.com/w3c/publishingcg/issues/23)
The visual‑to‑textual discussion focuses on expressing the logical reading order and textual equivalents of visually rich layouts. It aligns with parallel content needs when a textual stream must be mapped to visual regions.

#### [Region based navigation](https://idpf.org/epub/renditions/region-nav/)
Region‑based navigation allows navigation to named regions within fixed‑layout pages. This supports mapping between a visual panel or region and its textual or audio equivalent, enabling synchronised navigation across representations.

#### Media Overlays for text and audio

EPUB Media Overlays 3.0 defines a synchronisation model between text and audio using SMIL, enabling fine‑grained read‑along experiences. It demonstrates that the EPUB ecosystem can support synchronised parallel representations at the fragment level, but this model is currently limited to the text + audio pairing. 

#### Support for `<img>` in SMIL
The EPUB Working Group PR [#2919](https://github.com/w3c/epub-specs/pull/2919) clarifies that `<img>` can be used as a timed element in SMIL, not just `<text>` and `<audio>`. 

This enables Media Overlays to synchronize image fragments with other media (e.g., audio or video), allowing read‑along or guided‑view experiences where images highlight or switch in step with narration. 

It does not define new UI behavior, but it makes image‑based parallel streams interoperable at the timing model level.

### Implementation examples

The following examples are intentionally minimal, illustrating how existing mechanisms can be combined in practice. All referenced implementations are closely linked to specific applications, offer no interoperability outside those closed ecosystems and cannot currently be ported to other reading systems.

#### Article-level XML with PDF page coordinates

You can make XML and PDF “bi‑relational” by giving each article a stable ID in XML and storing the page coordinates of the corresponding region(s) in that same XML, then using those coordinates in your viewer to jump between PDF tiles and article view. 

This method is known to be used by press and newspaper apps.

Here is a minimal, newspaper-style XML where each article has:
* A stable `@xml:id`
* One or more `<pageRef>` elements with page number and a bounding box in PDF coordinates (e.g. points or pixels)

```xml
<issue id="issue-2026-02-06"
       xmlns="http://example.org/ns/issue">
  <article xml:id="art-001">
    <metadata>
      <title>Government plans new policy</title>
      <section>Politics</section>
      <pubDate>2026-02-06</pubDate>
    </metadata>

    <!-- bi‑relation: link to one region on page 3 of the PDF -->
    <pageRef page="3"
             pdf="issue-2026-02-06.pdf"
             x="120" y="450" width="820" height="520"/>

    <body>
      <p>The government announced today that …</p>
      <p>According to the minister …</p>
    </body>
  </article>

  <article xml:id="art-002">
    <metadata>
      <title>Local team wins cup</title>
      <section>Sports</section>
      <pubDate>2026-02-06</pubDate>
    </metadata>

    <!-- split across two regions (continued on page 8) -->
    <pageRef page="5" pdf="issue-2026-02-06.pdf"
             x="90" y="200" width="860" height="400"/>
    <pageRef page="8" pdf="issue-2026-02-06.pdf"
             x="100" y="150" width="840" height="380"/>

    <body>
      <p>In a dramatic finale …</p>
      <p>The coach said …</p>
    </body>
  </article>
</issue>
```

How it is used:
* Replica → article: When the user taps inside a PDF tile at coordinates (*px, py*) on page 5, you lookup which `<pageRef>` bounding box contains that point and open `art-002` in your article view.
* Article → replica: In article mode, a “View on page” button reads the `<pageRef>` for the current article and scrolls/zooms the PDF viewer to the corresponding page and region.


#### Fixed-layout + reflowable files side by side

Leveraging similar IDs and JavaScript capabilities, a fixed-layout (FXL) and a reflowable (RFL) file are bundled together in an app without using the Multiple-Rendition specification.

This method is known to have been used by educational publishers, at least in France and Italy.

Below is a minimal, concrete example that links one chunk of a fixed-layout page to the corresponding reflowable fragment.

```html
<!-- fxl/content/page001.xhtml -->
<section id="chunk-001" data-sync="chunk-001" class="fxl-page">
    <div class="fxl-text left" style="position:absolute; top:80px; left:60px; width:320px;">
    <div aria-label="Text">
        <span class="glyph" style="position:absolute; left:0px; top:0px;">T</span>
        <span class="glyph" style="position:absolute; left:8px; top:0px;">e</span>
        <span class="glyph" style="position:absolute; left:12px; top:0px;">x</span>
        <span class="glyph" style="position:absolute; left:20px; top:0px;">t</span>
    </div>
    </div>
    <div class="fxl-text right" style="position:absolute; top:80px; left:420px; width:320px;">
    <div aria-label="More text">
        <span class="word" style="position:absolute; left:0px; top:0px;">More</span>
        <span class="word" style="position:absolute; left:44px; top:0px;"> </span>
        <span class="word" style="position:absolute; left:104px; top:0px;">text</span>
    </div>
    </div>
</section>
```

```html
<!-- rfl/content/granule00001.xhtml -->
<section id="chunk-001" data-sync="chunk-001">
    <h2>Chapter 1</h2>
    <p>Text for the same chunk.</p>
</section>
```

```javascript
// app/parallel-sync.js (reading-system-specific)
function syncToChunk(chunkId) {
    openFXL(`fxl/content/page001.xhtml#${chunkId}`);
    openRFL(`rfl/content/granule00001.xhtml#${chunkId}`);
}

// Example: user taps a fixed-layout region
document.addEventListener("click", (e) => {
    const target = e.target.closest("[data-sync]");
    if (target) syncToChunk(target.dataset.sync);
});
```

The reading system interface has a button to switch between both views.


#### POC — Thorium Reader: sign‑language video synchronised with text (Media Overlays)

Thorium Reader includes a proof‑of‑concept that uses EPUB 3 Media Overlays timing to keep a sign‑language video in sync with text. A short demo is available here: https://www.youtube.com/watch?v=RjKjFNHhtQY

Key characteristics of the POC:
- The text remains in a standard XHTML flow, with Media Overlays providing the timing map.
- A sign‑language video is displayed alongside (or over) the text and follows the same timing sequence as text highlighting.
- This is reading‑system‑specific behavior that extends the usual text+audio use case; interoperability is not guaranteed.

Illustrative (non‑normative) structure:

```xml
<!-- SMIL timing (Media Overlays) -->
<smil xmlns="http://www.w3.org/ns/SMIL" version="3.0">
  <body>
    <par>
      <text src="chapter.xhtml#p01"/>
      <!-- POC: video fragment used as the parallel stream for this par -->
      <video src="sign-language.mp4#t=0,5"/>
    </par>
    <par>
      <text src="chapter.xhtml#p02"/>
      <video src="sign-language.mp4#t=5,11"/>
    </par>
  </body>
</smil>
```

Notes:
- The POC demonstrates how Media Overlays timing can drive a video stream for sign‑language alongside text, but it is not a standard, portable feature today.

#### Prototype — Parallel content with an alternate spine and rendition mapping

This prototype was explored by @HadrienGardeur for EDRLab. It models bilingual parallel content inside a single EPUB by combining an alternate spine declared via `collection` with a mapping document that pairs resources or fragments.

Key ideas:
- The primary spine lists the default language.
- A `collection` with role `alternate` lists the parallel resources for another language.
- A `resource-map` navigation document expresses the alignment between documents or sentence‑level fragments.

Illustrative OPF (alternate spine):

```xml
<package xmlns="http://www.idpf.org/2007/opf" xmlns:epub="http://www.idpf.org/2007/ops" xmlns:rendition="http://www.idpf.org/vocab/rendition/#" version="3.0" xml:lang="en" unique-identifier="pub-id">
  <metadata xmlns:dc="http://purl.org/dc/elements/1.1/">
    <dc:title>Poetry in English</dc:title>
    <dc:identifier id="pub-id">urn:uuid:019c572c-8681-7319-8111-1038097a1897</dc:identifier>
    <meta property="dcterms:modified">2026-02-13T00:00:00Z</meta>
    <dc:language>en</dc:language>
  </metadata>
  <manifest>
    <item id="poem1" href="poem1.xhtml" media-type="application/xhtml+xml"/>
    <item id="poem2" href="poem2.xhtml" media-type="application/xhtml+xml"/>
    <item id="poem3" href="poem3.xhtml" media-type="application/xhtml+xml"/>
    <item id="poem1-fr" href="poem1-fr.xhtml" media-type="application/xhtml+xml"/>
    <item id="poem2-fr" href="poem2-fr.xhtml" media-type="application/xhtml+xml"/>
    <item id="poem3-fr" href="poem3-fr.xhtml" media-type="application/xhtml+xml"/>
    <item id="mapping" properties="mapping" href="mapping.xhtml" media-type="application/xhtml+xml"/>
  </manifest>
  <spine>
    <itemref idref="poem1"/>
    <itemref idref="poem2"/>
    <itemref idref="poem3"/>
  </spine>
  <collection role="alternate">
    <metadata>
      <dc:language>fr</dc:language>
    </metadata>
    <link href="poem1-fr.xhtml"/>
    <link href="poem2-fr.xhtml"/>
    <link href="poem3-fr.xhtml"/>
  </collection>
</package>
```

Illustrative mapping document (resource‑level alignment):

```html
<html xmlns="http://www.w3.org/1999/xhtml">
  <head>
    <meta charset="utf-8"/>
  </head>
  <body>
    <nav epub:type="resource-map">
      <ul>
        <li><a href="poem1.xhtml"/></li>
        <li><a href="poem1-fr.xhtml"/></li>
      </ul>
      <ul>
        <li><a href="poem2.xhtml"/></li>
        <li><a href="poem2-fr.xhtml"/></li>
      </ul>
      <ul>
        <li><a href="poem3.xhtml"/></li>
        <li><a href="poem3-fr.xhtml"/></li>
      </ul>
    </nav>
  </body>
</html>
```

Fragment‑level alignment can follow the same pattern using fragment identifiers (e.g. `#sentence1` ↔ `#phrase1`).

Pros:
- Reuses existing EPUB structures (`collection`, `nav`, manifest), so it is easy to prototype without new syntax.
- Scales from document‑level to fragment‑level alignment with the same mapping pattern.
- Keeps both language resources first‑class in the package, which helps distribution and preservation.

Cons:
- `collection` roles for alternate spines are not widely implemented, so reading systems may ignore the parallel stream.
- `resource-map` has limited adoption and unclear UI expectations, which reduces interoperability.
- Maintaining a fragment‑level map is authoring‑intensive and can be brittle if text changes.

#### Example — Accessible EPUB Comics 

[The Accessible EPUB Comics repository](https://github.com/HadrienGardeur/accessible-epub-comics/tree/main) provides a complete, working example that ties together image regions, text alternatives, and audio narration for each panel and speech bubble.

Key characteristics:
- Uses Media Fragments (`#xywh`) to identify panel and bubble regions inside comic pages.
- Provides parallel HTML text equivalents and MP3 audio for each region.
- Uses `<img>` in SMIL to synchronise image regions with the parallel text/audio stream, based on the EPUB Working Group clarification for images in Media Overlays.


## References and links

- EPUB WG discussion: “Ways to publish and consume parallel contents” (Discussion #2829)  
  https://github.com/w3c/epub-specs/discussions/2829 

- EPUB 3.4 specification  
  https://www.w3.org/TR/epub-34/ 

- EPUB 3.4 Authoring (Editor’s draft)  
  https://w3c.github.io/epub-specs/epub34/authoring/ 

- EPUB 3 Fixed‑Layout Documents  
  https://idpf.org/epub/fxl/ 

- EPUB Accessibility – Fixed Layout Challenges and Best Practices  
  https://www.w3.org/TR/epub-fxl-a11y/ 

- EPUB Multiple‑Rendition Publications 1.1  
  https://w3c.github.io/epub-specs/epub33/multi-rend/ 

- EPUB 3.3 issue: “Remove multiple renditions from EPUB 3.3”  
  https://github.com/w3c/epub-specs/issues/1436 

- EPUB Media Overlays 3.0  
  https://idpf.org/epub/30/spec/epub30-mediaoverlays-20110215.html

- Thorium Reader discussion: “Parallel reading and comparison of books/papers”  
  https://github.com/edrlab/thorium-reader/discussions/2788 

- Thorium EPUB3 Media Overlays sign language video synchronised with text.  https://www.youtube.com/watch?v=RjKjFNHhtQY
