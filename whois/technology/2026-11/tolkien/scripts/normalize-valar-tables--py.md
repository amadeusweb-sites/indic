---

## normalize-valar-tables--py

**Introduction:** Adds Markdown table separator rows to Valar Markdown files. This records an intermediate formatting pass, but its output conflicts with the site's final convention of omitting separator rows.

**How to use:** Do not run on the finished pages unchanged. If reproducing the historical step, set `TOLKIEN_VALAR_DIR`, use a backup, extract the code to a local `.py` file, and inspect every affected table.

```py
import os
import re
from pathlib import Path

directory = Path(os.environ["TOLKIEN_VALAR_DIR"])


def is_separator(line):
    if not line.startswith("|"):
        return False
    cells = line.strip().strip("|").split("|")
    return bool(cells) and all(re.fullmatch(r"\s*:?-{3,}:?\s*", cell) for cell in cells)


changed = 0
for path in directory.glob("*.md"):
    original = path.read_text(encoding="utf-8")
    lines = original.splitlines()
    result = []
    index = 0

    while index < len(lines):
        if not lines[index].startswith("|"):
            result.append(lines[index])
            index += 1
            continue

        end = index
        while end < len(lines) and lines[end].startswith("|"):
            end += 1

        rows = [line for line in lines[index:end] if not is_separator(line)]
        if rows:
            column_count = len(rows[0].strip().strip("|").split("|"))
            result.append("|" + "|".join("---" for _ in range(column_count)) + "|")
            result.extend(rows)
        index = end

    updated = "\n".join(result)
    if original.endswith(("\n", "\r")):
        updated += "\n"
    if updated != original:
        path.write_text(updated, encoding="utf-8")
        changed += 1

print(f"Normalized tables in {changed} Markdown files")
```
