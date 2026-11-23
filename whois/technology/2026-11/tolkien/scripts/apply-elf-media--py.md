---

## apply-elf-media--py

**Introduction:** Applies a first-pass artwork table, fansite links, and search links to Elf profile pages using `elf_fanart_candidates.json`. It writes profile files in place and can replace manual edits.

**How to use:** First run the collector and keep its JSON beside the extracted `.py` file. Set `TOLKIEN_ELVES_DIR`, back up the profile tree, run locally with Python, and inspect all changed pages before proceeding to refinement.

```py
import html
import json
import os
import re
import unicodedata
from pathlib import Path
from urllib.parse import quote_plus, urlsplit


ROOT = Path(os.environ["TOLKIEN_ELVES_DIR"])
CANDIDATES = Path(__file__).with_name("elf_fanart_candidates.json")


def escape_cell(value):
    return (
        value.replace("[", r"\[")
        .replace("]", r"\]")
        .replace("|", r"\|")
        .replace("\r", " ")
        .replace("\n", " ")
        .strip()
    )


def search_key(name):
    normalized = unicodedata.normalize("NFKD", name)
    normalized = "".join(char for char in normalized if not unicodedata.combining(char))
    words = re.sub(r"[^a-zA-Z0-9]+", " ", normalized.lower()).split()
    return "+".join(words)


def get_fansite_links(text, name):
    links = []
    for line in text.splitlines():
        if line.startswith("- [Tolkien Gateway:") or line.startswith("- [Encyclopedia of Arda:"):
            if line not in links:
                links.append(line)
    if not any("Tolkien Gateway:" in line for line in links):
        links.append(
            f"- [Tolkien Gateway search: {name}]"
            f"(https://tolkiengateway.net/w/index.php?search={quote_plus(name)})"
        )
    if not any("Encyclopedia of Arda:" in line for line in links):
        links.append("- [Encyclopedia of Arda](https://encyclopediaofarda.com/)")
    links.append("- [The Tolkien Society](https://www.tolkiensociety.org/)")
    return links


def make_art_table(artworks):
    rows = ["| SNo | Title | Creator | Website |"]
    for number, artwork in enumerate(artworks, start=1):
        title = escape_cell(html.unescape(artwork["title"]))
        creator = escape_cell(html.unescape(artwork["creator"]))
        artwork_url = html.unescape(artwork["url"])
        artist_slug = urlsplit(artwork_url).path.strip("/").split("/")[0]
        rows.append(
            f"| {number} | [{title}]({artwork_url}) | "
            f"[{creator}](https://www.deviantart.com/{artist_slug}) | "
            "| [DeviantArt](https://www.deviantart.com/) |"
        )
    return "\n".join(rows)


def main():
    candidate_rows = json.loads(CANDIDATES.read_text(encoding="utf-8"))
    candidates_by_path = {row[0].replace("/", "\\"): row for row in candidate_rows}
    pages = [page for page in ROOT.rglob("*.md") if page.name != "home.md"]
    if len(pages) != 66 or len(candidates_by_path) != 66:
        raise RuntimeError(
            f"Expected 66 character pages and candidate lists; found "
            f"{len(pages)} and {len(candidates_by_path)}"
        )

    updates = []
    for page in pages:
        relative = str(page.relative_to(ROOT))
        if relative == r"vanyar\ingwe.md":
            continue
        text = page.read_text(encoding="utf-8")
        name = candidates_by_path[relative][1]
        artworks = list(candidates_by_path[relative][2][:10])
        if relative == r"noldor\edrahil.md" and len(artworks) < 3:
            artworks.append({
                "title": "Nirnaeth Arnoediad",
                "creator": "ForeverMedhok",
                "url": "https://www.deviantart.com/forevermedhok/art/Nirnaeth-Arnoediad-662124410",
            })
        if len(artworks) < 3:
            raise RuntimeError(f"Fewer than three fan-art links for {relative}: {len(artworks)}")

        further = re.search(r"(?m)^## Further Reading\s*$", text)
        editorial = re.search(r"(?m)^## Editorial Note\s*$", text)
        if not further or not editorial:
            raise RuntimeError(f"Unexpected article structure: {relative}")
        old_gallery = re.search(r"(?m)^## Fan Art Gallery\s*$", text)
        base_end = old_gallery.start() if old_gallery else further.start()
        base = text[:base_end].rstrip()
        reading = text[further.end():editorial.start()].splitlines()
        reading = [
            line for line in reading
            if "Tolkien Gateway:" not in line
            and "Encyclopedia of Arda:" not in line
            and "The Tolkien Society" not in line
        ]
        reading_body = re.sub(r"(?:\n\s*)?---\s*$", "", "\n".join(reading)).strip()
        if not reading_body:
            reading_body = "- *The Silmarillion*, especially the cited sections above."

        key = search_key(name)
        query = quote_plus(f"{name} Tolkien elf")
        table = make_art_table(artworks)
        sections = (
            "\n\n---\n\n## Fan Art Gallery\n\n"
            "These DeviantArt works were found through a character-specific Tolkien search. "
            "Some depict the character in a broader scene; follow each title for the artist's original page.\n\n"
            + table
            + "\n\n---\n\n## Fansites\n\n"
            + "\n".join(get_fansite_links(text, name))
            + "\n\n---\n\n## Search Links\n\n"
            + f"- [Search Google for {name}](https://www.google.com/search?q={query})\n"
            + f"- [Search YouTube for {name}](https://www.youtube.com/results?search_query={query})\n"
            + f"- [Search DAWN / %siteName%](#search={key}+tolkien+elf)"
            + "\n\n---\n\n## Further Reading\n\n"
            + reading_body
        )
        result = base + sections + "\n\n---\n\n" + text[editorial.start():].strip() + "\n"
        result = re.sub(r"(?m)^## ", "---\n\n## ", result)
        result = re.sub(r"(?m)^---\n\n---\n\n## ", "---\n\n## ", result)
        updates.append((page, result))

    for page, result in updates:
        page.write_text(result, encoding="utf-8")
    print(f"Updated {len(updates)} character pages; left the approved Ingwë sample unchanged.")


if __name__ == "__main__":
    main()
```
