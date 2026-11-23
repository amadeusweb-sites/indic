# Elves of the Silmarillion: build notes

This changelog documents the inventory, hierarchy, article pass, and site-specific formatting for the Elf pages under `indic/global/myth/tolkien/elves`. It records the corrections and practical lessons as well as the final structure. The companion scripts and run cautions are in the [scripts home page](./scripts/home.md).

The completed set contains 66 character pages, organized by kindred, with house subfolders only for the Noldor. Ten `home.md` files provide the hierarchy's landing points; the CMS supplies their listings, so those pages intentionally do not duplicate them with manual tables.

---

## Inventory before prose

The work began as an inventory, not as a decision to write full biographies. Empty Markdown pages were created for the named characters, then organized so the file tree could be reviewed before the detailed pass.

There was an early scope mistake: an inventory attempt was directed at the wrong Tolkien section. The user caught it, clarified that the inventory belonged under `elves`, and the work was redirected. Later Elf work used the `elves` tree and did not treat the Valar directory as part of the Elf inventory. This was a useful reminder to verify the resolved output root before running a bulk file generator.

The Noldor alone received house subfolders. The other kindreds remain grouped by kindred rather than being given invented or inconsistent house structures. Each index page was later kept table-free because the site already supplies the listing at the bottom of the page.

---

## From inventory to article set

After the inventory and hierarchy were reviewed, the empty pages were filled with individualized entries rather than one copied template with only names changed. Profiles generally cover:

- an overview and alternate names;
- family, relationships, and affiliations where supported;
- the character's role in the published *Silmarillion*;
- an “Appears In” table with the volume, chapter, or story and a short explanation;
- further reading and an editorial note distinguishing summary from interpretation.

The appearance table is a guide to relevant passages, not a quotation or edition-specific page citation. Each chapter reference needs editorial checking against the intended edition, especially where a claim draws on material outside the published *Silmarillion*.

---

## Art, fansites, and search links

The first media approach used direct image embeds. The image host's token validation rejected some of those URLs, so the presentation changed to a linked-artwork table with the requested columns: `SNo`, `Title`, `Creator`, and `Website`. Each title points to the original artwork page; creator and site names are linked separately.

Ingwë was prepared first for review. After approval, the media format was applied to the other profiles, with at least three fan-art links per page. DeviantArt search results were gathered in bulk, then refined because broad character-name searches can return unrelated results. Where there were too few clearly named results, related Tolkien scene art was used rather than pretending it was a portrait; the table introduction describes that limitation.

Each profile also has fansite links and Google, YouTube, and DAWN searches. DAWN uses a `+` for spaces, for example `#search=ingwe+tolkien+elf`, so the site's fragment-based search receives a space-safe query. These are discovery links, not endorsements of every result they may return.

Artwork links are external references. The fan-art tables do not grant image-reuse rights, and search-result relevance or the artist's attribution should be checked at the destination before publication or reuse.

---

## Markdown and CMS contract

- Every `##` heading in character profiles has a horizontal-rule separator (`---`) above it, with blank lines around the rule. The title remains an `#` heading.
- The exact `<!--has-ai-elements-->` comment appears directly above the first Markdown table in each profile.
- Markdown tables have header rows but no dashed separator row. Blockquotes, where present, start in the first column.
- Internal character links use the CMS's folder route format rather than `.md` filenames.
- The menu TSV uses tab-separated slug/display-name rows; `~Group` introduces a second-level menu group. The ordering is deliberate.
- The ten `home.md` pages are short landing pages for automatic site listings. They intentionally contain neither manual Markdown tables nor the first-table marker.

The distinction between manual and CMS-generated listings matters: the Valar `home.md` has a deliberately authored table, while the Elf `home.md` pages do not. Reusing the Valar index generator for Elf indexes would reintroduce duplicate tables.

---

## Validation and site check

The final pass checked that the profile set numbered 66, each page had the heading separators, first-table marker, required fansite/search sections, and at least three numbered artwork links. It also checked that the ten index pages remained table-free. The repeatable validator is [validate-elf-articles--py.md](./scripts/validate-elf-articles--py.md).

The public sitemap at [the Tolkien Elves listing](https://indic.missions.amadeusweb.in/sitemap/?at=global/myth/tolkien/elves) confirmed that the kindred and house hierarchy was visible through the CMS. A sitemap confirms discoverability and route structure; it cannot prove the accuracy of article content or that every remote artwork page remains available.

---

## Script history and cautions

The script archive includes the initial inventory creator, index/menu builder, article writer, fan-art collection and refinement stages, and validator. [build-elf-indexes--py.md](./scripts/build-elf-indexes--py.md) represents an earlier pass that wrote manual index tables; the current indexes must remain table-free. Treat it as historical unless that behavior is deliberately removed and reviewed.

[collect-elf-fanart-candidates--py.md](./scripts/collect-elf-fanart-candidates--py.md) documents the public search-page collector, which saves a temporary JSON result file beside the extracted script. Its parser depends on the search site's HTML and can stop working when that site changes. [apply-elf-media--py.md](./scripts/apply-elf-media--py.md) documents the media writer, and [refine-elf-fanart--py.md](./scripts/refine-elf-fanart--py.md) the gallery refinement pass. These source listings can overwrite editorial changes; back up the tree and inspect their output before using them.

The scripts have local absolute paths replaced with `TOLKIEN_ELVES_DIR` or `TOLKIEN_VALAR_DIR`. This makes the archive portable, but also means those variables must be set explicitly before running a script. Consult the [scripts home page](./scripts/home.md) for the suggested order and the scripts that should not be rerun unchanged.

---

## Lessons and remaining editorial work

- Validate the target directory before any bulk creation. A correct list written into the wrong subtree is still a damaging result.
- Inventory first, then agree on hierarchy, then generate prose. That separation made it possible to review the 66-name scope before investing in detailed pages.
- Keep category structure consistent without inventing subcategories. Here, only Noldor house folders were requested.
- Treat CMS conventions as part of the content model: generated listings, menu TSVs, path slugs, table markers, and heading separators all affect the published result.
- Search results are candidates, not vetted citations. Art, creator identity, chapter references, and lore summaries all require human review.
- The requested content and current tree were updated in the filesystem and checked against the live sitemap. This workspace was not a Git repository, so no Git commits or changelog history were created.