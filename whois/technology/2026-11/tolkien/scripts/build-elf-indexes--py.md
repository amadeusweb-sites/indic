---

## build-elf-indexes--py

**Introduction:** Builds the Elf kindred/house landing pages and menu TSVs from the existing character tree. This is an earlier version that wrote manual tables into some `home.md` pages, unlike the final CMS-driven indexes.

**How to use:** Only reproduce this historical step on a backup. Set `TOLKIEN_ELVES_DIR`, extract the fenced code to a local `.py` file outside the published tree, then run it with Python. Review all generated `home.md` and menu files; do not deploy the generated manual tables unchanged.

```py
import os
from pathlib import Path

root = Path(os.environ["TOLKIEN_ELVES_DIR"])
marker = "<!--has-ai-elements-->"

kindreds = {
    "avari": ("Avari", []),
    "moriquendi": (
        "Moriquendi",
        [
            ("thingol", "Thingol (Elwë)"),
            ("lenwe", "Lenwë"),
            ("denethor", "Denethor"),
            ("cirdan", "Círdan"),
            ("celeborn", "Celeborn"),
            ("luthien", "Lúthien"),
            ("daeron", "Daeron"),
            ("beleg", "Beleg"),
            ("mablung", "Mablung"),
            ("saeros", "Saeros"),
            ("eol", "Eöl"),
            ("nimloth", "Nimloth"),
            ("dior", "Dior"),
            ("elured", "Eluréd"),
            ("elurin", "Elurín"),
            ("elwing", "Elwing"),
            ("earendil", "Eärendil"),
            ("elrond", "Elrond"),
            ("elros", "Elros"),
            ("falathar", "Falathar"),
            ("erellont", "Erellont"),
            ("aerandir", "Aerandir"),
        ],
    ),
    "teleri": (
        "Teleri",
        [
            ("olwe", "Olwë"),
            ("earwen", "Eärwen"),
        ],
    ),
    "vanyar": (
        "Vanyar",
        [
            ("ingwe", "Ingwë"),
            ("indis", "Indis"),
            ("elenwe", "Elenwë"),
            ("elemmire", "Elemmírë"),
            ("amarie", "Amarië"),
        ],
    ),
}

houses = {
    "house-of-finwe": (
        "House of Finwë",
        [
            ("finwe", "Finwë"),
            ("miriel", "Míriel"),
        ],
    ),
    "house-of-feanor": (
        "House of Fëanor",
        [
            ("feanor", "Fëanor"),
            ("nerdanel", "Nerdanel"),
            ("maedhros", "Maedhros"),
            ("maglor", "Maglor"),
            ("celegorm", "Celegorm"),
            ("caranthir", "Caranthir"),
            ("curufin", "Curufin"),
            ("amrod", "Amrod"),
            ("amras", "Amras"),
            ("celebrimbor", "Celebrimbor"),
        ],
    ),
    "house-of-fingolfin": (
        "House of Fingolfin",
        [
            ("fingolfin", "Fingolfin"),
            ("fingon", "Fingon"),
            ("turgon", "Turgon"),
            ("aredhel", "Aredhel"),
            ("idril", "Idril"),
            ("maeglin", "Maeglin"),
        ],
    ),
    "house-of-finarfin": (
        "House of Finarfin",
        [
            ("finarfin", "Finarfin"),
            ("finrod", "Finrod Felagund"),
            ("angrod", "Angrod"),
            ("aegnor", "Aegnor"),
            ("orodreth", "Orodreth"),
            ("galadriel", "Galadriel"),
            ("finduilas", "Finduilas"),
        ],
    ),
}

other_noldor = [
    ("gil-galad", "Gil-galad"),
    ("gwindor", "Gwindor"),
    ("gelmir", "Gelmir"),
    ("guilin", "Guilin"),
    ("edrahil", "Edrahil"),
    ("arminas", "Arminas"),
    ("rumil", "Rúmil"),
    ("galdor", "Galdor"),
    ("ecthelion", "Ecthelion"),
    ("glorfindel", "Glorfindel"),
    ("voronwe", "Voronwë"),
    ("egalmoth", "Egalmoth"),
]


def write_index(directory, title, entries, note=None, columns=("Name",)):
    directory.mkdir(parents=True, exist_ok=True)
    lines = [f"# {title}", ""]
    if note:
        lines += [note, ""]
    if entries:
        lines += [marker, "| " + " | ".join(columns) + " |"]
        for entry in entries:
            lines.append("| " + " | ".join(entry) + " |")
    (directory / "home.md").write_text("\n".join(lines) + "\n", encoding="utf-8")


def write_menu(directory, groups):
    lines = ["#slug\tname"]
    for group_name, entries in groups:
        if group_name:
            lines.append("~" + group_name)
        lines.extend(f"{slug}\t{name}" for slug, name in entries)
    (directory / "_menu-items.tsv").write_text("\n".join(lines) + "\n", encoding="utf-8")


root_entries = [
    ("avari", "Avari"),
    ("moriquendi", "Moriquendi"),
    ("noldor", "Noldor"),
    ("teleri", "Teleri"),
    ("vanyar", "Vanyar"),
]
write_index(
    root,
    "Elves of the Silmarillion",
    [(f"[{name}](./{slug}/)",) for slug, name in root_entries],
)
write_menu(root, [("Kindreds", root_entries)])

for slug, (title, names) in kindreds.items():
    directory = root / slug
    table_entries = [(f"[{name}](./{person}/)",) for person, name in names]
    note = "No named Avari are individually identified in the published *Silmarillion*." if slug == "avari" else None
    write_index(directory, title, table_entries, note=note)
    write_menu(directory, [("", names)])

noldor = root / "noldor"
house_entries = [(slug, title) for slug, (title, _) in houses.items()]
write_index(
    noldor,
    "Noldor",
    [(f"[{name}](./{slug}/)", "House") for slug, name in house_entries]
    + [(f"[{name}](./{slug}/)", "Noldorin Elf") for slug, name in other_noldor],
    columns=("House or name", "Entry"),
)
menu_groups = [
    ("Royal houses", house_entries),
    ("Other Noldor", other_noldor),
]
write_menu(noldor, menu_groups)

for slug, (title, names) in houses.items():
    directory = noldor / slug
    table_entries = [(f"[{name}](./{person}/)",) for person, name in names]
    write_index(directory, title, table_entries)
    write_menu(directory, [("", names)])
```
