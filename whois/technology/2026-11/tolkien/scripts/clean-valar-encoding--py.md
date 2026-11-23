---

## clean-valar-encoding--py

**Introduction:** Applies a collection of known mojibake-to-Unicode replacements to Markdown files in the Valar directory. It records the encoding repair pass; it is not routine cleanup for files already saved as UTF-8.

**How to use:** Back up the Valar directory, set `TOLKIEN_VALAR_DIR`, extract the code to a local `.py` file, and run it only against files confirmed to contain the listed corrupt sequences. Inspect the diff immediately; repeated or broad replacement passes can change valid text.

```py
import os
from pathlib import Path

directory = Path(os.environ["TOLKIEN_VALAR_DIR"])

replacements = {
    "â€”": "—",
    "â€“": "–",
    "â€™": "’",
    "â€œ": "“",
    "â€": "”",
    "â€˜": "‘",
    "â€¦": "…",
    "Ã«": "ë",
    "Ã‰": "É",
    "Ã¤": "ä",
    "Ã¡": "á",
    "Ã©": "é",
    "Ã­": "í",
    "Ã³": "ó",
    "Ãº": "ú",
    "Ã¼": "ü",
    "Ã±": "ñ",
    "Ã¶": "ö",
    "Ã‡": "Ç",
    "ÃŠ": "Ê",
    "Ãƒâ‚¬": "À",
    "ÃƒÂ«": "ë",
    "ÃƒÂ¼": "ü",
    "ÃƒÂ¶": "ö",
    "ÃƒÂº": "ú",
    "ÃƒÂ¡": "á",
    "ÃƒÂ©": "é",
    "ÃƒÂ¤": "ä",
    "ÃƒÂ±": "ñ",
    "ÃƒÂ³": "ó",
    "ÃƒÂ­": "í",
    "Ãª": "ê",
}

cleaned = 0
for path in directory.glob("*.md"):
    if path.name == "home.md":
        continue

    text = path.read_text(encoding="utf-8")
    original = text
    for bad, good in replacements.items():
        text = text.replace(bad, good)

    if text != original:
        path.write_text(text, encoding="utf-8")
        cleaned += 1

print(f"Cleaned {cleaned} Markdown files")
```
