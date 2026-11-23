---

## refine-elf-fanart--py

**Introduction:** Refines the first-pass fan-art selections by filtering titles and using a small curated set of related Tolkien-scene links where search results are sparse. It rewrites gallery tables in place.

**How to use:** Keep the collector's JSON beside the extracted `.py` file, set `TOLKIEN_ELVES_DIR`, and back up the profile tree. Run only after the first-pass media script, then verify every artwork and creator link manually.

```py
import html
import json
import os
import re
import unicodedata
from pathlib import Path
from urllib.parse import urlsplit


ROOT = Path(os.environ["TOLKIEN_ELVES_DIR"])
CANDIDATES = Path(__file__).with_name("elf_fanart_candidates.json")

RELATED = {
    r"moriquendi\aerandir.md": [
        ("Silmarillion: Reaching Valinor", "LadyElleth", "https://www.deviantart.com/ladyelleth/art/Silmarillion%3A-Reaching-Valinor-93419249"),
        ("Tale of Tuor, Part 6: A New Beginning", "MatejCadil", "https://www.deviantart.com/matejcadil/art/Tale-of-Tuor%2C-Part-6%3A-A-New-Beginning-360039406"),
    ],
    r"moriquendi\elured.md": [
        ("Awakening pt3", "ArlenianChronicles", "https://www.deviantart.com/arlenianchronicles/art/Awakening-pt3-777490227"),
        ("Awakening pt5", "ArlenianChronicles", "https://www.deviantart.com/arlenianchronicles/art/Awakening-pt5-777707385"),
    ],
    r"moriquendi\elurin.md": [
        ("Awakening pt3", "ArlenianChronicles", "https://www.deviantart.com/arlenianchronicles/art/Awakening-pt3-777490227"),
    ],
    r"moriquendi\erellont.md": [
        ("Silmarillion: Reaching Valinor", "LadyElleth", "https://www.deviantart.com/ladyelleth/art/Silmarillion%3A-Reaching-Valinor-93419249"),
        ("Tale of Tuor, Part 6: A New Beginning", "MatejCadil", "https://www.deviantart.com/matejcadil/art/Tale-of-Tuor%2C-Part-6%3A-A-New-Beginning-360039406"),
        ("Middle-earth heraldry: Earendil 01", "Aglargon", "https://www.deviantart.com/aglargon/art/Middle-earth-heraldry%3A-Earendil-01-512138481"),
    ],
    r"moriquendi\falathar.md": [
        ("Silmarillion: Reaching Valinor", "LadyElleth", "https://www.deviantart.com/ladyelleth/art/Silmarillion%3A-Reaching-Valinor-93419249"),
        ("Tale of Tuor, Part 6: A New Beginning", "MatejCadil", "https://www.deviantart.com/matejcadil/art/Tale-of-Tuor%2C-Part-6%3A-A-New-Beginning-360039406"),
        ("Middle-earth heraldry: Earendil 01", "Aglargon", "https://www.deviantart.com/aglargon/art/Middle-earth-heraldry%3A-Earendil-01-512138481"),
    ],
    r"moriquendi\lenwe.md": [
        ("Nandor", "SarkaSkorpikova", "https://www.deviantart.com/sarkaskorpikova/art/Nandor-713875015"),
    ],
    r"noldor\edrahil.md": [
        ("Fen of Serech (redraw)", "Airendis", "https://www.deviantart.com/airendis/art/Fen-of-Serech-%28redraw%29-960997631"),
        ("Nirnaeth Arnoediad", "ForeverMedhok", "https://www.deviantart.com/forevermedhok/art/Nirnaeth-Arnoediad-662124410"),
    ],
    r"noldor\guilin.md": [
        ("Gwindor's Death", "Peet", "https://www.deviantart.com/peet/art/Gwindor%27s-Death-957145"),
        ("Gwindor's Charge", "Peet", "https://www.deviantart.com/peet/art/Gwindor%27s-Charge-544955343"),
    ],
}


def normalize(value):
    value = unicodedata.normalize("NFKD", value)
    value = "".join(char for char in value if not unicodedata.combining(char))
    return re.sub(r"[^a-z0-9]+", " ", value.lower()).strip()


def escape_cell(value):
    return (
        value.replace("[", r"\[")
        .replace("]", r"\]")
        .replace("|", r"\|")
        .replace("\r", " ")
        .replace("\n", " ")
        .strip()
    )


def make_table(artworks):
    rows = ["| SNo | Title | Creator | Website |"]
    for number, artwork in enumerate(artworks, start=1):
        title = escape_cell(html.unescape(artwork["title"]))
        creator = escape_cell(html.unescape(artwork["creator"]))
        url = html.unescape(artwork["url"])
        artist = urlsplit(url).path.strip("/").split("/")[0]
        rows.append(
            f"| {number} | [{title}]({url}) | "
            f"[{creator}](https://www.deviantart.com/{artist}) | [DeviantArt](https://www.deviantart.com/) |"
        )
    return "\n".join(rows)


def main():
    candidate_rows = json.loads(CANDIDATES.read_text(encoding="utf-8"))
    candidates_by_path = {row[0].replace("/", "\\"): row for row in candidate_rows}
    updates = []
    counts = []

    for page in ROOT.rglob("*.md"):
        relative = str(page.relative_to(ROOT))
        if page.name == "home.md" or relative == r"vanyar\ingwe.md":
            continue
        _, name, candidates = candidates_by_path[relative]
        key = normalize(name)
        terms = [key]
        if key.split():
            terms.append(key.split()[0])
        artworks = [
            item for item in candidates
            if any(term in normalize(item["title"]) for term in terms if term)
        ][:10]

        for title, creator, url in RELATED.get(relative, []):
            if len(artworks) >= 3:
                break
            if not any(item["url"] == url for item in artworks):
                artworks.append({"title": title, "creator": creator, "url": url})
        if len(artworks) < 3:
            raise RuntimeError(f"Fewer than three relevant artwork links for {relative}: {len(artworks)}")

        text = page.read_text(encoding="utf-8")
        heading = re.search(r"(?m)^## Fan Art Gallery\s*$", text)
        next_section = re.search(r"(?m)^---\n\n## Fansites\s*$", text)
        if not heading or not next_section or next_section.start() <= heading.start():
            raise RuntimeError(f"Unexpected fan-art section structure: {relative}")
        section = (
            "## Fan Art Gallery\n\n"
            "These links lead to Tolkien fan art featuring the character or related story scenes.\n\n"
            + make_table(artworks)
            + "\n\n"
        )
        updates.append((page, text[:heading.start()] + section + text[next_section.start():]))
        counts.append(len(artworks))

    for page, result in updates:
        page.write_text(result, encoding="utf-8")
    print(f"Refined {len(updates)} fan-art tables; entries per page: {min(counts)}-{max(counts)}.")


if __name__ == "__main__":
    main()
```
