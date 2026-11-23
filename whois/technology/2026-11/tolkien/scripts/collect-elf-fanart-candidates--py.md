---

## collect-elf-fanart-candidates--py

**Introduction:** Collects public DeviantArt search-result metadata for the Elf profiles and writes `elf_fanart_candidates.json` beside the extracted script. The collector relies on the site's current HTML and may stop working if that markup changes.

**How to use:** Set `TOLKIEN_ELVES_DIR`, extract to a local `.py` file in a working directory, and run with network access. Review the generated JSON for relevance and attribution; it is input for the media scripts, not verified content.

```py
import html
import json
import os
import re
from concurrent.futures import ThreadPoolExecutor, as_completed
from pathlib import Path
from urllib.parse import quote_plus
from urllib.request import Request, urlopen


ROOT = Path(os.environ["TOLKIEN_ELVES_DIR"])
OUTPUT = Path(__file__).with_name("elf_fanart_candidates.json")
ART_LINK = re.compile(
    r'<a\b[^>]*href="(https://www\.deviantart\.com/[^" ]+/art/[^" ]+)"'
    r'[^>]*aria-label="([^"]+), visual art"'
)


def collect(page):
    name = page.read_text(encoding="utf-8").splitlines()[0].removeprefix("# ").strip()
    query = quote_plus(name + " Tolkien")
    request = Request(
        "https://www.deviantart.com/search?q=" + query,
        headers={"User-Agent": "Mozilla/5.0"},
    )
    with urlopen(request, timeout=40) as response:
        body = response.read().decode("utf-8", "replace")

    artworks = []
    seen = set()
    for url, label in ART_LINK.findall(body):
        url = html.unescape(url.split("#", 1)[0])
        label = html.unescape(label)
        if url in seen:
            continue
        seen.add(url)
        title, creator = label.rsplit(" by ", 1) if " by " in label else (label, "")
        artworks.append({"title": title, "creator": creator, "url": url})

    return page.relative_to(ROOT).as_posix(), name, artworks


def main():
    pages = sorted(page for page in ROOT.rglob("*.md") if page.name != "home.md")
    if len(pages) != 66:
        raise RuntimeError(f"Expected 66 character pages, found {len(pages)}")

    with ThreadPoolExecutor(max_workers=5) as pool:
        futures = [pool.submit(collect, page) for page in pages]
        rows = [future.result() for future in as_completed(futures)]
    rows.sort()
    OUTPUT.write_text(json.dumps(rows, ensure_ascii=False, indent=2), encoding="utf-8")
    print(f"Saved fan-art search results for {len(rows)} character pages to {OUTPUT.name}.")


if __name__ == "__main__":
    main()
```
