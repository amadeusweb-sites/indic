# Tolkien fights — working log

This monthly note records the current scope, conventions, editorial standard, and maintenance status for the Tolkien fights pages. The live page tree and its category menus are the source of truth for the active roster; this file intentionally does not duplicate every filename.

---

## Current site state

The [Fights index](%url%myth/tolkien/fights/) groups entries by event type. As checked on 2026-11-24, the active menus contain 67 first-pass articles:

DIV-FILELINKS

- Battles: 35 — [menu](../../../../global/myth/tolkien/fights/battles/_menu-items.tsv)
- Wars: 12 — [menu](../../../../global/myth/tolkien/fights/wars/_menu-items.tsv)
- Sieges: 8 — [menu](../../../../global/myth/tolkien/fights/sieges/_menu-items.tsv)
- Sacks: 9 — [menu](../../../../global/myth/tolkien/fights/sacks/_menu-items.tsv)
- Kinslayings: 3 — [menu](../../../../global/myth/tolkien/fights/kinslayings/_menu-items.tsv)

DIV-CLOSE

The counts come from the current menu rows, not a separate filename roster. A validation pass confirmed that every menu slug has a non-empty article with a title and an `Appears In` section, and that menu targets and local Markdown links resolve. This is a working collection, not a claim that every conflict in Tolkien's writings has been catalogued or that every first-pass interpretation is final.

---

## Scope and classification

- Keep the five event types in their own folders and indexes: battles, wars, sieges, sacks, and kinslayings.
- Some episodes may merit more than one entry when the source supports distinct event aspects. Cross-link those pages and explain the relationship rather than treating one entry as a duplicate.
- Use Tolkien's published writings as the authority for names, chronology, and event claims. Secondary sites are discovery aids, not substitutes for the cited passages.

---

## Filename and menu conventions

- Name article files with a concise, slug-only event name; put the full display title in the page's H1.
- Keep dates in the exact lowercase `sno` column of the category's `_menu-items.tsv`. Group rows under the appropriate `~Years of the Trees`, `~First Age`, `~Second Age`, or `~Third Age` heading.
- Use `--first`, `--second`, or `--third` when an ordinal is needed to distinguish events. Preserve deliberate concise exceptions such as `battles/beleriand.md`; its H1 supplies “First Battle.”
- Disambiguate same-folder collisions with a meaningful suffix, as in `battles/pelargir.md` and `battles/pelargir--corsairs.md`.
- When adding, renaming, or removing an article, update its category menu in the same change. The menu and live page tree—not this log—define the current routes.

---

## Article standard

DIV-FILELINKS

Use [Nirnaeth Arnoediad](../../../../global/myth/tolkien/fights/battles/nirnaeth-arnoediad.md) and [Dagor Bragollach](../../../../global/myth/tolkien/fights/battles/dagor-bragollach.md) as the page-structure models. Match their depth where the source supports it; never pad an article with invented detail.

DIV-CLOSE

### Required page structure

Use the following sections in this order. Adapt “battle” to “war,” “siege,” “sack,” or “kinslaying” where appropriate; keep the same explanatory purpose.

```markdown
# Full event title

*Subtitle: date or range, age, and location where known*

---

## Overview

Write a concise, self-contained summary before the quick-facts table.

<!--has-ai-elements-->
[Quick-facts table: Name, Date, Location, Participants, Outcome]

---

## Causes and Context

---

## Course of the [Battle / War / Siege / Sack]

1. **Phase or turning point.** Explain the event in sequence.

---

## Commanders and Peoples

[Commanders and peoples table: Side, Leaders and peoples, Part in the event]

---

## Outcome and Significance

---

## Appears In

[Appears In table: Volume / story, Section, Appearance or significance]

---

## Fan Art Gallery

---

## Videos

---

## Fansites

---

## Search Links
```

### Drafting and review checklist

- [ ] Confirm the event's type, accepted name, date or range, location, and participants from primary texts; flag uncertain details.
- [ ] Distinguish the event from adjacent battles, phases, aftermaths, and events that share a place or name.
- [ ] Draft an original overview and quick-facts table; place `<!--has-ai-elements-->` immediately before the first table.
- [ ] Check that each table has matching headers and data columns; omit pipe-and-hyphen separator rows as required by the site format.
- [ ] Explain causes and context before a chronological course; number the course's phases and identify turning points.
- [ ] Identify commanders and peoples where the sources do so; separate known facts from reasonable inference.
- [ ] Explain outcome and lasting significance without repeating the overview.
- [ ] Make `Appears In` exact: name the work and chapter, appendix, annal, or story, and say whether it is a direct account, a retrospective, or only context.
- [ ] Precede **every `##` section heading** with a standalone `---` horizontal rule, with a blank line on each side. Keep the title and italic subtitle at the top without a preceding rule.
- [ ] Credit fan artists and link to original artwork; use the shared stub until a gallery is curated. Link verified videos as community interpretation; if none can be verified, label YouTube search links as searches rather than as selected videos.
- [ ] Keep Fansites links event-specific; after a blank line following direct links, add `%"fansite-search-nodesnippet"%`.
- [ ] Check quotations, names, dates, links, menu entry, and rendered output. Prefer paraphrase; any quotation must be short and attributed.

---

### Shared media snippets

DIV-FILELINKS

- Until a curated gallery is ready, use `%"fanart-stub-snippet"%` beneath `## Fan Art Gallery`; the source is [fanart-stub.md](../../../../data/snippets/fanart-stub.md).
- Under `## Fansites`, keep only links to specific pages about that event. After a blank line following those links, use `%"fansite-search-nodesnippet"%`.
- The shared fansite search list is [fansite-search.md](../../../../global/myth/tolkien/data/node-snippets/fansite-search.md). Its site order is a work in progress, broadly ordered by familiarity rather than measured traffic. `FIRSTHEADING` is converted by the builder to `data-append="first-heading"`; client-side appending of the heading to the search query is still pending.

DIV-CLOSE

---

## Drafting and review passes

1. **Identify and source:** Confirm the event, type, title, date, location, and primary passages.
2. **Outline:** Plan causes, chronology, participants, outcome, and related events before drafting.
3. **Draft to structure:** Use the required section order and separator rules above; scale detail to the evidence, not to an arbitrary word count.
4. **Verify:** Check names, chronology, claims, citations, and distinctions between connected events.
5. **Editorial pass:** Review paraphrases, short quotations, media attribution, cross-links, and caveats.
6. **Site pass:** Check the H1, fact-table marker, every `---` separator, local links, menu slug/date/group, snippets, and rendered page. Update this log only when a lasting convention or status changes.

---

## Recent work

- 2026-11-24: Replaced historical staging notes with the current menu-derived status and maintenance rules; refreshed category introductions to describe their existing contents.
- 2026-11-24: Expanded the 61 shorter event pages with dedicated context/significance sections and cross-links. Corrected the Kin-strife sequence: Castamir fell at the Crossings of Erui, and his followers made their last stand at Pelargir.
- 2026-11-24: Added an explicit feature-page structure and drafting checklist, including the required horizontal rule before every section heading.
- 2026-11-24: Rebuilt the Angmar article with the feature structure, a chronology, commander table, sourced “Appears In” table, and labeled media/search sections; added heading separators throughout the fights articles.
- 2026-11-24: Reworked all 12 Wars articles to the feature-page structure, with chronological courses, participants, outcomes, source tables, related links, and clearly labeled media searches.
- 2026-11-24: Reworked all 32 shorter Battles articles to the feature-page structure with event-specific context, chronology, participants, source tables, and media/search sections; retained the detailed model pages and added the missing Five Armies video-search section.
- 2026-11-24: Reworked all 9 Sacks and 8 Sieges articles to the feature-page structure, separating related assaults, battles, and sieges and adding source and media sections.
- The five category folders and menus are in place, and the 67 menu-listed event pages have first-pass content.
- Nirnaeth Arnoediad remains the detailed battle model. Dagor Bragollach, the three Kinslayings, and the Battle of the Five Armies have since been expanded as feature pages.
- The Battle of the Five Armies page uses the shared fan-art stub and event-specific Fansites links followed by the shared search snippet.
