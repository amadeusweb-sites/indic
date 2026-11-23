# Valar and Valier: build notes

This changelog records how the Valar section was assembled under `indic/global/myth/tolkien/valar`, including the content decisions, CMS conventions, corrections, and lessons from the file-driven workflow. The companion scripts and their usage notes are in the [scripts home page](./scripts/home.md).

The finished section contains individual pages for the fourteen Valar and Valier, plus a `home.md` overview with the Valar table and artwork. The files are ordinary Markdown and TSV; the CMS supplies the navigation and page rendering.

---

## Work sequence

1. **Create the fourteen-page set.** Individual Markdown pages were drafted for the Valar and Valier, with an overview, names and associations, role in the legendarium, selected quotations, at least five fan-art images, three YouTube search links, and an “Appears In” section. The latter points readers to relevant volumes, chapters, or stories instead of treating a character page as a plot summary.
2. **Build the overview table.** The Valar list was reproduced as a Markdown table. Names link to CMS paths such as `./manwe/` (no `.md` extension), and each row includes artwork as requested. This is intentionally different from the Elf indexes, which are populated by the CMS.
3. **Add grouped menu data.** The TSV menu separates the chief Valar and other groups using `~Group` lines; individual rows use a slug and display name separated by a tab. Display names can include fuller forms such as “Manwë Súlimo (Skies).” The Spring menu builder was inspected to understand the existing `home` exclusion and group handling; it was not changed.
4. **Normalize Markdown for this CMS.** Blockquotes start in column one. The `<!--has-ai-elements-->` marker sits directly above the first table. Table separator rows such as `|---|---|---|` were removed to match the site's Markdown flavor.
5. **Correct encoding.** A visible `â€”` in “Valar — Lord of the West” exposed a UTF-8/legacy decoding mismatch. The content was cleaned and the UTF-8-aware generator retained as the preferred version. The earlier mojibake generator and repair scripts are preserved as historical steps, not as the recommended way to regenerate pages.
6. **Check the rendered route.** The public sitemap showed the tree at `global/myth/tolkien/valar`. This verified that the file hierarchy and CMS menu conventions were being interpreted as expected; it did not validate every lore statement or third-party artwork link.

---

## File and CMS conventions

- `home.md` is the hand-maintained Valar overview, so its table is intentional. The CMS does not need a second manually maintained list on the Elf `home.md` pages.
- The overview's internal links use a trailing slash and omit `.md`, because that is the route format expected by this CMS.
- The `<!--has-ai-elements-->` marker is immediately before the first Markdown table. The table has a header row but no separator row.
- `_menu-items.tsv` is navigation data, not Markdown. Keep tabs as field delimiters and use a `~` prefix only for a group heading. Menu order follows file order.
- Character prose and the overview are UTF-8. Avoid a legacy-encoding pass after writing UTF-8 content; it can recreate the mojibake it was meant to fix.
- The overview's remote artwork is a dependency on external hosts. Some image hosts use signed or tokenized URLs that may expire or be rejected by the CMS. A page route can remain healthy even when an external image stops rendering.

---

## Script history and operating cautions

The [script archive](./scripts/) contains the Valar generator, table builder, and encoding utilities. [generate-valar-utf8--ps1.md](./scripts/generate-valar-utf8--ps1.md) is the encoding-corrected PowerShell variant; [generate-valar--py.md](./scripts/generate-valar--py.md) is the Python generator. [generate-valar--ps1.md](./scripts/generate-valar--ps1.md) is kept as an archival record of the encoding failure; it contains mojibake and is not recommended to run.

[clean-valar-encoding--py.md](./scripts/clean-valar-encoding--py.md) and [fix-valar-encoding--ps1.md](./scripts/fix-valar-encoding--ps1.md) document the repair utilities. [create-valar-table--py.md](./scripts/create-valar-table--py.md) rebuilds the overview, while [normalize-valar-tables--py.md](./scripts/normalize-valar-tables--py.md) adds Markdown separator rows. That last behavior conflicts with the final house style, so do not run it on the finished content unchanged. The README documents the expected environment variables and the effects of each listing.

The initial Valar artwork helper scrapes search-page markup, pairs image URLs and artwork pages by their order in separate result lists, and suppresses request errors. Those links can be mismatched or incomplete; verify each one. The scripts are evidence of the workflow, not an immutable build pipeline. Several are historical generators that write directly into the live content directory and may replace hand-edited material. Make a backup, inspect the target directory, and run only one chosen generator when reproducing a step.

---

## What this workflow taught us

- A content route is a file-and-folder contract: slugs, trailing-slash links, `home.md`, and menu TSV ordering all affect the published site.
- A generator can be correct for its original pass yet wrong for the final house style. Preserve its history, but mark it superseded rather than applying it indiscriminately.
- Encoding should be chosen at the point of writing. Once files are consistently UTF-8, repair scripts should not be part of routine generation.
- “Artwork embedded in a table” couples the page to a third-party host. Verify both the displayed image and the artist's original page, and prefer durable links when the host's image tokens are unreliable.
- Automation can assemble scaffolding and repetitive structure; it cannot establish that every chapter reference, interpretation, attribution, or image is correct. Those still need source-by-source editorial review.
- This work was performed on the filesystem and checked against the public sitemap. This workspace was not a Git repository, so no Git commits or history were created as part of this sequence.

---

## Editorial verification still matters

The “Appears In” entries are navigation aids, not quotations or edition-specific page citations. Chapter divisions and wording can vary across publications and related Tolkien texts. Check the cited locations against the edition being used before treating the pages as a scholarly reference.

Likewise, an image appearing in a search result is not proof of correct attribution, subject identification, or permission to reuse. The page links should be checked at their original destinations, and any embedded image should comply with the host and site's current requirements.

---

## Roster check to resolve

While archiving the files, I noticed that the current fourteen-page roster includes Melkor but has no Irmo (Lórien). The overview describes the roster as seven male and seven female Powers; the present male entries include Melkor. This may reflect an intentional historical inclusion, but it should be checked against the intended definition of “the fourteen Valar and Valier” before the content is presented as a complete roster. This documentation task did not alter the existing Valar pages or table.