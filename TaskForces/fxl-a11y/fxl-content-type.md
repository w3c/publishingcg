---
title: Fixed Layout Content Types
date: 2023-11-11
---

While some of these content types can use reflowable, many use fixed layout to preserve formatting from the print edition. Some of these content types use fixed layout almost exclusively due to the style of the content. To understand the use cases for the content and its formatting, we've isolated common component types for each content type. These component types can help us determine what approaches may be taken to help make the content more accessible, as for fixed layout, there is no one-size-fits-all solution.

We initially tabulated this information in [a google sheet shared table](https://docs.google.com/spreadsheets/d/1YyiIEoRL19Ao9ZK6iFlogr8unbbDps4Wkfog8zvpjbo/edit?gid=0#gid=0)


|Content Type|Component Types|Proposed Solution(s)
|---|---|---|
|Instructive (i.e. DIY, cookbooks, technical manuals, textbooks)|Images, Image Descriptions (alt and extended), Headings, Text, Data Tables, Visualizations, Lists, Videos, Audio, MathML, Interactivity|HTML and/or SVG Techniques, multiple renditions, visual-textual, mixed FXL and reflow|
|Decorative/Entertainment (i.e. Art books, photography, magazines)|Images, Image Descriptions (alt and extended), Headings, Text, Lists|HTML and/or SVG Techniques
|Children/K-12 Education (i.e. picture books, intermediate chapter books)|Images, Image Descriptions (alt and extended), Headings, Text, Video, Audio, SMIL (Read Along), Interactivity|HTML and/or SVG Techniques, Hybrid EPUBs, visual-textual, multiple renditions, mixed FXL and reflow,
|Illustrative Works (i.e. Poetry, Illustrated non-fiction)|Images, Image Descriptions (alt and extended), Headings, Text|HTML and/or SVG Techniques, visual-textual, mixed FXL and reflow
|Comics (including Manga, BD, Webtoons)|Images, Image Descriptions (alt and extended)|HTML and/or SVG Techniques|

