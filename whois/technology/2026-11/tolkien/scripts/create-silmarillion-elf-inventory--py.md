---

## create-silmarillion-elf-inventory--py

**Introduction:** Creates the initial named-Elf Markdown inventory and Noldor house structure. This is a scaffolding generator intended for an empty or deliberately prepared destination, not for refreshing completed character pages.

**How to use:** Confirm `TOLKIEN_ELVES_DIR` points to the intended tree, back up any existing content, extract the code to a local `.py` file, and run it with Python. Review the created paths and count before writing articles; do not run it over the finished set casually.

```py
import os
from pathlib import Path

root = Path(os.environ["TOLKIEN_ELVES_DIR"])

groups = {
    "noldor/house-of-finwe": [
        "finwe",
        "miriel",
    ],
    "noldor/house-of-feanor": [
        "feanor",
        "nerdanel",
        "maedhros",
        "maglor",
        "celegorm",
        "caranthir",
        "curufin",
        "amrod",
        "amras",
        "celebrimbor",
    ],
    "noldor/house-of-fingolfin": [
        "fingolfin",
        "fingon",
        "turgon",
        "aredhel",
        "idril",
        "maeglin",
    ],
    "noldor/house-of-finarfin": [
        "finarfin",
        "finrod",
        "angrod",
        "aegnor",
        "orodreth",
        "galadriel",
        "finduilas",
    ],
    "noldor": [
        "gil-galad",
        "gwindor",
        "gelmir",
        "guilin",
        "edrahil",
        "arminas",
        "rumil",
        "galdor",
        "ecthelion",
        "glorfindel",
        "voronwe",
        "egalmoth",
    ],
    "vanyar": [
        "ingwe",
        "indis",
        "elenwe",
        "elemmire",
        "amarie",
    ],
    "teleri": [
        "olwe",
        "earwen",
    ],
    "moriquendi": [
        "thingol",
        "lenwe",
        "denethor",
        "cirdan",
        "celeborn",
        "luthien",
        "daeron",
        "beleg",
        "mablung",
        "saeros",
        "eol",
        "nimloth",
        "dior",
        "elured",
        "elurin",
        "elwing",
        "earendil",
        "elrond",
        "elros",
        "falathar",
        "erellont",
        "aerandir",
    ],
}

created = []
for relative_dir, slugs in groups.items():
    directory = root / relative_dir
    directory.mkdir(parents=True, exist_ok=True)
    for slug in slugs:
        page = directory / f"{slug}.md"
        if not page.exists():
            page.touch()
            created.append(page)
        elif page.stat().st_size != 0:
            raise RuntimeError(f"Refusing to overwrite non-empty file: {page}")

print(f"Created {len(created)} empty Markdown files")
print(f"Inventory contains {sum(map(len, groups.values()))} named Elf entries")
```
