---

## validate-elf-articles--py

**Introduction:** Read-only check for the 66 Elf profiles and 10 index pages. It checks the current heading separators, first-table markers, gallery rows, search/fansite sections, and table-free indexes.

**How to use:** Set `TOLKIEN_ELVES_DIR`, extract the code to a local `.py` file, and run it with Python after editing or regenerating the profiles. A pass verifies formatting and counts, not lore accuracy, artist attribution, or remote link availability.

```py
import os
import re
from pathlib import Path


ROOT = Path(os.environ["TOLKIEN_ELVES_DIR"])


def main():
    profiles = [page for page in ROOT.rglob("*.md") if page.name != "home.md"]
    homes = list(ROOT.rglob("home.md"))
    errors = []
    fan_art_counts = []

    for page in profiles:
        text = page.read_text(encoding="utf-8")
        lines = text.splitlines()
        headings = [index for index, line in enumerate(lines) if line.startswith("## ")]
        for index in headings:
            if (
                index < 3
                or lines[index - 1] != ""
                or lines[index - 2] != "---"
                or lines[index - 3] != ""
                or lines[index + 1] != ""
            ):
                errors.append(f"Bad separator around {page}: {lines[index]}")

        table_indexes = [index for index, line in enumerate(lines) if line.startswith("|")]
        if (
            text.count("<!--has-ai-elements-->") != 1
            or not table_indexes
            or table_indexes[0] == 0
            or lines[table_indexes[0] - 1] != "<!--has-ai-elements-->"
        ):
            errors.append(f"Missing or misplaced first-table marker: {page}")
        if re.search(r"(?m)^\|(?:\s*:?-{3,}:?\s*\|)+\s*$", text):
            errors.append(f"Unexpected table separator row: {page}")
        required = (
            "## Fan Art Gallery",
            "## Fansites",
            "## Search Links",
            "Search Google",
            "Search YouTube",
            "Search DAWN / %siteName%",
            "Tolkien Gateway:",
            "Encyclopedia of Arda:",
        )
        for phrase in required:
            if phrase not in text:
                errors.append(f"Missing {phrase}: {page}")

        section = re.search(r"(?ms)^## Fan Art Gallery\n.*?(?=^---\n\n## )", text)
        rows = [
            line for line in (section.group(0) if section else "").splitlines()
            if re.match(r"^\| \d+ \|", line)
        ]
        fan_art_counts.append(len(rows))
        numbers = [int(re.match(r"^\| (\d+) \|", line).group(1)) for line in rows]
        if len(rows) < 3 or numbers != list(range(1, len(rows) + 1)):
            errors.append(f"Invalid fan-art row numbering: {page}: {numbers}")
        if any(line.count("](https://") != 3 for line in rows):
            errors.append(f"Expected artwork, creator, and website links in each row: {page}")

    for home in homes:
        text = home.read_text(encoding="utf-8")
        if re.search(r"(?m)^\|", text) or "<!--has-ai-elements-->" in text:
            errors.append(f"Unexpected manual table or marker in index: {home}")

    if len(profiles) != 66 or len(homes) != 10:
        errors.append(f"Expected 66 profiles and 10 index pages; found {len(profiles)} and {len(homes)}")
    if errors:
        raise SystemExit("\n".join(errors))
    print(
        f"Validated {len(profiles)} profiles ({min(fan_art_counts)}-{max(fan_art_counts)} fan-art rows), "
        f"{len(homes)} table-free index pages, heading separators, and table markers."
    )


if __name__ == "__main__":
    main()
```
