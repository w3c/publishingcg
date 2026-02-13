# Web Publications: 2026 state of usages

In january 2026 the publishing community held a plenary session to collect information about the use and potentials of the web publication format. This was the occasion to build this state of usages document. 

## Overview

A **Web Publication (WPUB)** is a collection of one or more web resources (HTML, audio, images, styles, scripts) organized by a manifest that describes metadata, default reading order, and resource list. It is exposed through a primary entry page at a stable URL, which acts as the canonical identifier and human entry point.

Web Publications are intended to be presentable as a bounded, book-like work (with navigation, table of contents, etc.) while remaining fully web-native. This model supports many kinds of publications on the Web: long-form articles, books, journals, corporate documents, educational material, audiobooks, and more.

### Key Characteristics

- **Set of Resources**: One or more web resources organized together
- **Manifest-Based**: Includes metadata, default reading order, and resource list
- **Stable Entry Point**: A primary URL serving as canonical identifier
- **Web-Native**: Built on Open Web Platform technologies (HTML, URLs, web app manifest, service workers, etc.)
- **Bounded Presentation**: Presentable as a coherent publication with navigation and table of contents
- **Offline Capable**: Can be cached using Service Workers for offline access

## Standardization History

### Timeline

- **January 2018**: First public Working Draft of Web Publications (WD‑wpub‑20180104) published
- **August 2019**: "Web Publications" and "Web Publications Use Cases and Requirements" published as Working Group Notes, not Recommendations
  - Work did not progress further on the standards track
  - However, related specification "Publication Manifest" became a Recommendation

### Current Status

The core Web Publications specification remains a Working Group Note. However, the **Publication Manifest** specification became a W3C Recommendation and is actively used in specific domains, particularly audiobooks.

**Relevant Documents:**
- [Web Publications](https://www.w3.org/TR/wpub/)
- [Web Publications Use Cases and Requirements](https://www.w3.org/TR/pwp-ucr/)
- [Publication Manifest](https://www.w3.org/TR/pub-manifest/)
- [Web Publications Proposed Specification](https://w3c.github.io/pwpub/)

## Current Implementations and Use Cases

### Readium Ecosystem

**Readium WebPub Manifest** is a de facto standard implementation that closely aligns with the W3C specification:

- **Internal Format**: Readium uses the manifest as an internal format with significant benefits
- **Format-Agnostic**: EPUB is treated as a representation of this format, independent from EPUB version
- **Direct Resource Access**: Work directly with images, sound tracks, and other resources
- **Standardized Representations**: Canonical representations used for many years

**Readium Tools:**
- [Readium Playground](https://playground.readium.org) - Interactive examples
- [Readium CLI](https://readium.org/css/docs/manifest.json) - Export manifests from EPUB
- [Publication Server](https://publication-server.readium.org) - Generate manifests on-the-fly from EPUB files

**Use Cases:**
- **End-User Format**: Especially for audiobooks and comics
- **Authoring Format**: Used in educational contexts where EPUB limitations are problematic
- **Interchange Format**: Emerging tool for sharing publication metadata

### Known deployments

#### Catalogs
- **[OPDS Catalogues](https://opds.io/opds-2-0)**: OPDS 2.0 uses the Readium Web Publication Manifest in catalog and distribution services for discovery and lending.

#### Commercial Platforms
- **Bookshop.org**: US/UK online bookstore uses Readium Web Publication Manifest
- **NetGalley**: Major book discovery platform (US, UK, Australia, Germany, France, Japan) implements the manifest
- **Audiobook Platforms**: Quebec, Spain, Finland use Readium WPUB for audiobook distribution

#### Publishing Houses
- **Hachette**: Uses Web Publications as an internal format in their production pipeline
  - This was presented during 2025 Digital Publishing Summit. [Video replay: Web Publications as production format](https://www.edrlab.org/events/digital-publishing-summit-2025/#1742891648861-f7f314c9-6f67)
  - Benefits include cleaner workflows for digital object handling
  - Advantages for interactivity and complex content (infographics, interactive features)

#### Educational and Institutional
- **Brazil**: Uses WPUB for educational content authoring
- **Unicef**: Uses WPUB for educational content authoring
- **South Korea**: Interest in WPUB for education, preference over EPUB for interactive content
- **Finland (National Library)**: Exploring with Web Publication Manifest for collections and catalogues

### Emerging Use Cases

#### Interactive Educational Content
- Support for authoring complex interactive features (infographics, rich media)
- Ability to customize content collections for individual students
- Better interactivity support compared to EPUB, which often blocks JavaScript

#### Collection Management
- **Corporate Documents**: Collections of related documents with unified metadata
- **Thematic Collections**: Ability to group and present semantically related content

#### Offline Access and Caching
- Service Workers enable efficient caching of publication resources
- Selective downloading of resources for partial offline access
- Intermediate approach: Use Web Workers to download content for local reading

## Advantages Over Existing Formats

### Compared to EPUB
- **JavaScript Support**: Allows dynamic content and interactivity
- **No Pagination Assumptions**: Better suited for adaptive layouts
- **Educational Friendliness**: Supports modern web technologies readers may be familiar with
- **Format Independence**: Not tied to specific packaging mechanisms

### Compared to HTML
- **Collection Semantics**: Provides standardized way to express multiple related pages as a single publication
- **Metadata**: Includes comprehensive metadata attached to the collection
- **Reading Order**: Explicit default reading order specification
- **Offline Capability**: Built-in support for offline access through Web Workers/Service Workers

## Challenges and Barriers

### Why WPUB Didn't Progress Further

1. **Business Case Uncertainty**: Limited uptake from major traditional publishers
   - Limited survey of publisher needs before standardization
   - Unclear value proposition for traditional publishing workflows

2. **Browser Vendor Support**: Limited adoption by browser vendors
   - No major browser implemented native support
   - Focused on other priorities

3. **Timing**: Potentially 10 years ahead of market adoption
   - Emerged at a time when the web platform wasn't ready
   - More recent attention suggests changing conditions

4. **Competition with EPUB**: Risk of fragmentation in publishing standards
   - Publishing industry hesitant to adopt another format after EPUB adoption
   - Traditional publishers see no reason to move beyond EPUB

### Technical Barriers

1. **Packaging Problem**: Original packaging format debate remains unsolved
   - ZIP containers (used by EPUB) have limitations
   - No consensus on better alternative emerged

2. **Tool Ecosystem**: Limited authoring and distribution tools
   - Mostly implemented within Readium ecosystem
   - Wider industry adoption requires broader tooling

3. **Interoperability**: Publication Manifest and Readium WPUB Manifest are similar but not identical
   - Slight incompatibilities between implementations

## Current Momentum and Future Prospects

### Positive Indicators

1. **Renewed Interest**: Growing attention from publishing community
   - Digital Publishing Summit 2025 featured dedicated sessions
   - Companies actively sharing implementation experiences

2. **Proven Use Cases**: Real-world deployments showing viability
   - Commercial success in audiobooks and specialized contexts
   - Educational sector demonstrating clear advantages

3. **Web Platform Evolution**: Modern web technologies increasingly relevant
   - Service Workers mature and well-supported
   - JavaScript support essential for modern publications

4. **Industry Adoption**: Growing beyond experimental status
   - Readium ecosystem provides stable implementations
   - Multiple platforms deploying in production

### Opportunities

1. **Scholarly Publishing**: HTML-based scholarly publishing could benefit from WPUB metadata
2. **Interactive Content**: Growing demand for interactive educational materials
3. **Hybrid Formats**: Bridge between PDF, EPUB, and pure HTML content
4. **Offline-First Applications**: Web applications designed to work offline

## Web Publications for Academic Publishers

### Differences from HTML

While academic publishers increasingly distribute content as HTML, Web Publications offer additional structural benefits:

1. **Collection-Level Metadata**: Unlike HTML pages which exist independently, WPUB provides standardized metadata at the collection level
   - Author, publication date, publisher information
   - Rights and licensing information
   - Version and update metadata

2. **Defined Reading Order**: Explicit specification of intended reading sequence
   - Important for multi-article collections and special issues
   - Supports table of contents generation
   - Enables navigation aids

3. **Offline Access**: Built-in support for offline reading
   - Critical for researchers in regions with limited connectivity
   - Service Workers enable selective caching of resources

4. **Accessibility Metadata**: Enhanced accessibility information at publication level
   - ARIA descriptions and alt text can be standardized
   - Reading order metadata supports assistive technologies
   - Consistent accessibility profiles across publications

5. **Interoperable Format for Interchange**: Standardized format for exchanging content between systems
   - Cleaner separation of content from presentation
   - Independent of delivery platform (web, mobile, desktop)

### Scholarly Publishing Use Case

Academic publishing could leverage WPUB for:

- **Journal Issues**: Group articles with unified metadata and table of contents
- **Theses and Dissertations**: Multi-chapter works with consistent navigation
- **Conference Proceedings**: Collections with author metadata and citation information
- **Supplementary Materials**: Link primary articles with datasets, multimedia, and additional resources

The manifest provides a cleaner way to express these relationships than HTML alone, without requiring proprietary formats.

## Recommendations for Adoption

### For Publishers
- Evaluate WPUB for collections of related documents
- Consider for interactive educational content
- Use alongside (not as replacement for) EPUB for traditional publishing

### For Tool Developers
- Leverage Readium ecosystem for interoperability
- Develop authoring and conversion tools
- Support both streaming and offline download models

### For the W3C Community
- Revisit standardization potential given renewed interest
- Conduct market surveys to clarify business cases
- Bridge gaps between Publication Manifest and Readium implementations
- Engage browser vendors on implementation possibilities

## References

- [Web Publications Specification](https://www.w3.org/TR/wpub/)
- [Publication Manifest Recommendation](https://www.w3.org/TR/pub-manifest/)
- [Readium Project](https://readium.org)
- [Readium Playground](https://playground.readium.org)
- [W3C Publishing Community Group Plenary Minutes, 29 January 2026 what is the W3C Web Publications format suitable for?](../../Meetings/Minutes/2026-01-29-publishingcg.html)
