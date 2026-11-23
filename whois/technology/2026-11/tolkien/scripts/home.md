# Tolkien content script archive

This folder is now a Markdown-only record of the Valar and Elf content workflow. Every former `.py` and `.ps1` file has been converted into a `.md` document with an introduction, use guidance, and the original source inside a fenced code block. There are no standalone Python or PowerShell files here to deploy.

---

## Reading and using these records

Files use the pattern `script-name--py.md` or `script-name--ps1.md`. Their code blocks are labeled `py` or `ps`. Each document explains whether its source is current, historical, destructive, or unsuitable to run.

To run a listing, copy its fenced code into a local `.py` or `.ps1` file outside the published site tree. Set the required root variable, inspect the code, and back up any content it may change. Do not deploy the extracted script as part of the CMS content. Most generators write in place; the inventory and media stages can replace existing files.

Scripts that operate on the Valar tree require `TOLKIEN_VALAR_DIR`; Elf scripts require `TOLKIEN_ELVES_DIR`. For example, set the relevant PowerShell environment variable to the local content folder before running the extracted source. A missing variable is an error, not a default path.

---

## Valar and Valier records

- [generate-valar-utf8--ps1.md](./generate-valar-utf8--ps1.md) — UTF-8-aware PowerShell generator; review output and external links.
- [generate-valar--py.md](./generate-valar--py.md) — Python generator variant; writes profile pages.
- [generate-valar--ps1.md](./generate-valar--ps1.md) — malformed historical version; contains mojibake and is not recommended to run.
- [fix-valar-encoding--ps1.md](./fix-valar-encoding--ps1.md) and [clean-valar-encoding--py.md](./clean-valar-encoding--py.md) — encoding repair steps; use only on a verified backup.
- [create-valar-table--py.md](./create-valar-table--py.md) — rebuilds the hand-maintained Valar overview table.
- [normalize-valar-tables--py.md](./normalize-valar-tables--py.md) — inserts separator rows, contrary to the final table convention; do not use unchanged on finished pages.

The legacy Valar artwork helper pairs image and artwork-page search results by their position and suppresses request errors. Any resulting gallery needs manual verification. Menu TSV data is maintained separately; its `~Group` entries and row order affect navigation.

---

## Elf records and historical order

1. [create-silmarillion-elf-inventory--py.md](./create-silmarillion-elf-inventory--py.md) creates the initial inventory. Confirm the root and page list first.
2. [build-elf-indexes--py.md](./build-elf-indexes--py.md) builds landing pages and menu data. It records an earlier manual-table behavior; final `home.md` pages are table-free.
3. [write-elf-articles--py.md](./write-elf-articles--py.md) writes detailed profiles and can replace editorial text.
4. [collect-elf-fanart-candidates--py.md](./collect-elf-fanart-candidates--py.md) fetches public search results into `elf_fanart_candidates.json`; it requires network access and depends on the search site's HTML.
5. [apply-elf-media--py.md](./apply-elf-media--py.md) adds initial artwork, fansite, and search sections using that JSON.
6. [refine-elf-fanart--py.md](./refine-elf-fanart--py.md) revises artwork tables.
7. [validate-elf-articles--py.md](./validate-elf-articles--py.md) checks formatting and counts without changing the content.

If running the fan-art stages, keep the generated JSON beside the extracted collector/apply/refinement scripts. The JSON is intentionally not part of this archive because search results should be refreshed and reviewed rather than treated as fixed source data.

---

## Safety and editorial notes

The script listings document a past workflow; they are not a build system or an endorsement of every historical implementation. `generate-valar--ps1.md` contains mojibake, `normalize-valar-tables--py.md` conflicts with the final Markdown style, and `build-elf-indexes--py.md` can restore unwanted manual tables. Other generators overwrite content and need backups.

Automated checks verify structure, not lore accuracy, quotation fidelity, image attribution, permission to reuse art, or remote URL availability. Review those manually before publication.
