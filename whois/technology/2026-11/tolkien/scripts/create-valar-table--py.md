---

## create-valar-table--py

**Introduction:** Rebuilds the Valar `home.md` overview table, including route-style name links and external artwork. It replaces the overview content, so later editorial changes may be lost.

**How to use:** Back up the Valar folder, set `TOLKIEN_VALAR_DIR`, extract the code to a local `.py` file, and run it with Python. Inspect the full table, image destinations, and CMS formatting before using the output.

```py
import os
import re
from pathlib import Path

directory = Path(os.environ["TOLKIEN_VALAR_DIR"])

entries = [
    ("manwe", "Manwë", "Sulimo; King of the Valar", "Air, wind and Eagles", "Varda"),
    ("varda", "Varda", "Elentári; Queen of the Stars", "Light and stars", "Manwë"),
    ("ulmo", "Ulmo", "Lord of Waters", "Sea, rivers and all waters", "None"),
    ("yavanna", "Yavanna", "Kementári; Giver of Fruits", "Earth, growth and living things", "Aulë"),
    ("aule", "Aulë", "The Maker", "Earth, substance and craft", "Yavanna"),
    ("mandos", "Mandos (Námo)", "Doomsman of the Valar", "Memory, fate and the dead", "Vairë"),
    ("nienna", "Nienna", "The Weeper", "Pity, mercy and endurance", "None"),
    ("orome", "Oromë", "Aldaron; the Great Rider", "Forests, wild lands and the hunt", "Vána"),
    ("este", "Estë", "The Gentle", "Rest and healing", "Lórien"),
    ("vana", "Vána", "The Ever Young", "Spring, flowers and renewal", "Oromë"),
    ("vaire", "Vairë", "The Weaver", "History woven into the tapestry", "Mandos"),
    ("tulkas", "Tulkas", "Astaldo; the Valiant", "Strength, wrestling and courage", "Nessa"),
    ("nessa", "Nessa", "The Swift; the Dancer", "Dance, joy and swift motion", "Tulkas"),
    ("melkor", "Melkor (Morgoth)", "The Dark Enemy", "Discord, domination and corruption", "None"),
]

lines = [
    "# The Valar",
    "",
    "> “The Great among these spirits the Elves name the Valar, the Powers of Arda, and Men have often called them gods.”",
    "",
    "*The Silmarillion, Valaquenta*",
    "",
    "The traditional fourteen entries below are the Great Powers of Arda as presented in *The Silmarillion*: thirteen of the Aratar and the fallen Melkor.",
    "",
    "| Image | Name | Title or epithet | Dominion | Consort / spouse |",
    "|---|---|---|---|---|",
]

for slug, name, title, dominion, spouse in entries:
    text = (directory / f"{slug}.md").read_text(encoding="utf-8")
    match = re.search(r'\[!\[[^\]]+\]\((https://images-wixmp-[^)]+)\)\]\(https://www\.deviantart\.com/[^)]+\)', text)
    if not match:
        raise RuntimeError(f"No fan-art image found for {slug}")
    image = match.group(1)
    lines.append(
        f"| ![{name} fan art]({image}) | [{name}](./{slug}/) | {title} | {dominion} | {spouse} |"
    )

lines.extend([
    "",
    "## Notes",
    "",
    "- **Valar** is used here in the broad published sense that includes both the male and female Powers.",
    "- **Valier** is the Quenya term for the female Valar.",
    "- Manwë and Varda are lord and queen of the Valar; Melkor is included for historical completeness, although he became the Dark Enemy and was cast out of the circle of the Valar.",
    "",
])

(directory / "home.md").write_text("\n".join(lines), encoding="utf-8")
print(f"Wrote {directory / 'home.md'}")
```
