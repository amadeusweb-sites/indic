# Spring Menu Usage Notes

Spring builds navigation menus from `_menu-items.tsv` files in content folders. This page records only the current menu conventions; chronological work and filename decisions belong in the dated logs.

## Menu format

- The first column header is `#slug` and identifies the page slug.
- The optional second column is exactly lowercase `sno`; non-empty values are shown before the humanized slug.
- Do not add a `name` column: Spring derives the label from the slug.
- Use a group row such as `~First Age` to introduce a heading.
- Keep menu slugs aligned with slug-only Markdown filenames; put the full title in the page content.

The menu parser is `spring/builder/8-menu.php`.
