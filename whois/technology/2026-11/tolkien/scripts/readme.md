# Tolkien content scripts

This folder preserves the scripts used for the Valar and Elf content passes, along with a repeatable Elf fan-art validator. Some scripts are historical experiments; the archive is a record of how the work was done, not a single safe-to-run build command.

---

## Before running anything

Set the root variable for the content tree the script is intended to change:

```powershell
$env:TOLKIEN_VALAR_DIR = 'D:\path\to\indic\global\myth\tolkien\valar'
$env:TOLKIEN_ELVES_DIR = 'D:\path\to\indic\global\myth\tolkien\elves'
```

Scripts that only need one tree require only its variable. Python scripts use the current Python installation; the PowerShell scripts are intended for Windows PowerShell. A missing root variable causes the script to fail rather than silently choose a location.

Many scripts write files in place. Back up the target tree and inspect a script before running it on published content. Never run multiple generators over the same pages without checking their output.

---

## Valar and Valier scripts

- `generate_valar_utf8.ps1` is the preferred PowerShell snapshot for the UTF-8 generation pass.
- `generate_valar.py` is the Python generation variant.
- `generate_valar.ps1` is retained as the earlier encoding-problem version. It contains mojibake and currently fails PowerShell parsing; do not run it.
- `fix_valar_encoding.ps1` and `clean_valar_encoding.py` record the encoding repair steps. They are not routine post-processing for already-correct UTF-8 files.
- `create_valar_table.py` rebuilds the Valar `home.md` table, including its row artwork and folder-style links. Review the generated result and external URLs.
- `normalize_valar_tables.py` inserts Markdown separator rows. That behavior is contrary to the final table convention, so do not run it on the finished pages unchanged.

The legacy Valar artwork helper also pairs image and artwork-page search results by their position and suppresses request errors. Its gallery output needs manual verification. The Valar menu TSV is maintained separately from these generators. Its `~Group` entries and row order are part of the navigation behavior.

---

## Elf scripts and historical order

1. `create_silmarillion_elf_inventory.py` creates the initial character-page inventory. Confirm the root and page list before running it.
2. `build_elf_indexes.py` creates index pages and menu data. It records an earlier manual-table behavior; the final `home.md` pages must remain table-free.
3. `write_elf_articles.py` writes the detailed profile set. It can replace article text, so do not rerun without a backup and a deliberate review.
4. `collect_elf_fanart_candidates.py` fetches public search results and writes `elf_fanart_candidates.json` beside the script. It requires network access and depends on the search site's current HTML.
5. `apply_elf_media.py` adds the first-pass fan-art, fansite, and search sections based on that JSON.
6. `refine_elf_fanart.py` narrows or supplements the artwork selections for better relevance.
7. `validate_elf_articles.py` checks the resulting profile and index conventions without changing the content.

The original media collection was performed as a transient one-off operation. The collector in this archive captures its repeatable function; the generated JSON is intentionally not included because it is search-result data that should be refreshed and reviewed.

---

## Inventory

- `build_elf_indexes.py`
- `clean_valar_encoding.py`
- `collect_elf_fanart_candidates.py`
- `create_silmarillion_elf_inventory.py`
- `create_valar_table.py`
- `apply_elf_media.py`
- `fix_valar_encoding.ps1`
- `generate_valar.ps1`
- `generate_valar.py`
- `generate_valar_utf8.ps1`
- `normalize_valar_tables.py`
- `refine_elf_fanart.py`
- `validate_elf_articles.py`
- `write_elf_articles.py`

The copied scripts have machine-specific output paths replaced with `TOLKIEN_VALAR_DIR` or `TOLKIEN_ELVES_DIR`; these changes are for portability and do not alter the historic output intent. The title and menu content should still be reviewed against the current CMS rules before any regeneration.
