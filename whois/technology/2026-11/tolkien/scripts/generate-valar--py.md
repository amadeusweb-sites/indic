---

## generate-valar--py

**Introduction:** Python generator for the Valar and Valier profile pages. It writes directly into the Valar directory and includes externally sourced material, so its output requires editorial and link review.

**How to use:** Set `TOLKIEN_VALAR_DIR`, make a backup, extract this listing to a local `.py` file, and run it with Python and network access if its source-fetching portions are used. Review every generated page, quotation, appearance reference, image, and search link before publishing.

```py
import os
from pathlib import Path
from urllib.request import Request, urlopen
import html
import re
import time

OUT = Path(os.environ["TOLKIEN_VALAR_DIR"])
OUT.mkdir(parents=True, exist_ok=True)

VALAR = [
    dict(slug="manwe", name="Manwe", display="Manwë", kind="Valar — Lord of the West",
         epithet="King of the Valar", domain="Air, wind, sky and the Eagles",
         spouse="Varda", symbols="the eagle; the wind; the summit of Taniquetil",
         quote="Manwë is the elder King of the Valar and rules the wind and the air.",
         intro="Manwë is the chief of the Valar and the appointed king of Arda beneath Eru Ilúvatar."),
    dict(slug="varda", name="Varda", display="Varda", kind="Valar — Valier; Queen of the Stars",
         epithet="Elentári, the Star-Queen", domain="Light, stars and the night sky",
         spouse="Manwë", symbols="star-light; Elentári; light of the Two Trees",
         quote="Varda dwells in the Uttermost West and sets the stars in their paths.",
         intro="Varda is the Queen of the Valar and the Lady of the Stars; Elves call her Elbereth."),
    dict(slug="ulmo", name="Ulmo", display="Ulmo", kind="Valar — Lord of the Waters",
         epithet="The Lord of Waters", domain="Sea, rivers, rain and living water",
         spouse="Unmarried", symbols="Ulmonan; the Ulumúri; a silver chariot",
         quote="Ulmo is the Lord of Waters, and he has no fixed dwelling-place.",
         intro="Ulmo is the solitary Vala of the seas and rivers, and his music and counsel shaped Middle-earth."),
    dict(slug="yavanna", name="Yavanna", display="Yavanna", kind="Valar — Valier; Giver of Fruits",
         epithet="Kementári", domain="Growing things, forests, crops and living creatures",
         spouse="Aulë", symbols="the Two Trees; the corn-sheaf; Entish life",
         quote="Yavanna is the Lady of Fruits and the keeper of all growing things.",
         intro="Yavanna governs plant life and persuaded the other Valar to preserve a life of shepherding for the Earth."),
    dict(slug="aule", name="Aule", display="Aulë", kind="Valar — Lord of Substance",
         epithet="The Maker", domain="Earth, craft, smithwork and substance",
         spouse="Yavanna", symbols="the hammer; the forge; the mountain",
         quote="Aulë is a smith and master of all crafts.",
         intro="Aulë is the master of matter and craft, and his secret longing for children produced the Dwarves."),
    dict(slug="mandos", name="Mandos", display="Mandos (Námo)", kind="Valar — Lord of the Houses of the Dead",
         epithet="Námo; Doomsman of the Valar", domain="Death, memory, fate and the waiting halls",
         spouse="Vairë", symbols="the Doors of Night; the Halls of Mandos",
         quote="Mandos is the keeper of the Houses of the Dead and the Doomsman of the Valar.",
         intro="Mandos is the stern Vala of memory and the dead, though he speaks only what is appointed and not what he desires."),
    dict(slug="nienna", name="Nienna", display="Nienna", kind="Valar — Valier; Lady of Mercy",
         epithet="The Weeper", domain="Pity, mercy, mourning and endurance",
         spouse="Unmarried", symbols="the grey cloak; tears; rain",
         quote="Nienna is filled with pity and patience, and those who listen to her learn compassion.",
         intro="Nienna teaches pity and wisdom through sorrow and is the spiritual teacher of Gandalf in the lore."),
    dict(slug="orome", name="Orome", display="Oromë", kind="Valar — Lord of the Hunt",
         epithet="The Great Rider", domain="Forests, wild creatures and the hunt",
         spouse="Vána", symbols="the great horn Valaróma; Nahar",
         quote="Oromë is a great hunter and a rider of wild horses.",
         intro="Oromë rides through Middle-earth with a great horn, and he was the first Vala to encounter the Elves."),
    dict(slug="este", name="Este", display="Estë", kind="Valar — Valier; Lady of Rest",
         epithet="The Gentle", domain="Rest, healing and weariness",
         spouse="Irmo (Lórien)", symbols="grey robes; the quiet garden",
         quote="Estë the Gentle heals wounds of spirit and restores weariness.",
         intro="Estë ministers to hurts of mind and body and gives rest beneath her grey garments."),
    dict(slug="vana", name="Vana", display="Vána", kind="Valar — Valier; the Ever Young",
         epithet="Queen of the Spring", domain="Youth, spring, flowers and renewal",
         spouse="Oromë", symbols="flowers; springtime; birds",
         quote="Vána is ever young, and flowers spring where her feet touch the earth.",
         intro="Vána is the sister of Yavanna and companion of Oromë, associated with beauty, youth and awakening."),
    dict(slug="vaire", name="Vaire", display="Vairë", kind="Valar — Valier; the Weaver",
         epithet="The Weaver", domain="Weaving, history and the tapestry of time",
         spouse="Mandos (Námo)", symbols="the woven story; the web of time",
         quote="Vairë the Weaver weaves all that has ever been in the halls of Mandos.",
         intro="Vairë records the past in a vast tapestry, giving the deeds of Eä enduring form."),
    dict(slug="tulkas", name="Tulkas", display="Tulkas", kind="Valar — Lord of Strength",
         epithet="Astaldo, the Valiant", domain="Strength, wrestling, laughter and courage",
         spouse="Nessa", symbols="the wrestler; the laughing warrior",
         quote="Tulkas is the strongest in strength and the greatest in feats of might.",
         intro="Tulkas entered Arda for the sake of courage and laughter, becoming decisive in the final war against Melkor."),
    dict(slug="nessa", name="Nessa", display="Nessa", kind="Valar — Valier; the Swift",
         epithet="The Dancer", domain="Dance, speed, joy and the wild deer",
         spouse="Tulkas", symbols="the dancing feet; deer; green forest",
         quote="Nessa is lithe and swift, and she delights in the dance and the wild deer.",
         intro="Nessa is the sister of Oromë and a joyous runner and dancer in the groves of Valinor."),
    dict(slug="melkor", name="Melkor", display="Melkor (Morgoth)", kind="Valar — the first Dark Lord",
         epithet="Morgoth Bauglir", domain="Discord, domination, fear and corruption",
         spouse="Unmarried", symbols="the Iron Crown; Angband; the Silmarils",
         quote="Melkor, mightiest of the Ainur, fell from the Music into rebellion and domination.",
         intro="Melkor was the greatest and most powerful of the Ainur, but his desire to rule all of creation made him the enemy of Arda."),
]

COMMON_APPEARS = [
    ("The Silmarillion", "Ainulindalë", "Enter Eä through the Great Music and help shape the world."),
    ("The Silmarillion", "Valaquenta", "Identified as a member of the Valar and given character, spouse and dominion."),
]

UNIQUE_APPEARS = {
    "Manwe": [("The Silmarillion", "Quenta Silmarillion, ch. 1", "The Theft of the Silmarils?"),],
    "Varda": [],
}

def get_text(url):
    req = Request(url, headers={"User-Agent":"Mozilla/5.0 EncyclopediaBot/1.0"})
    with urlopen(req, timeout=20) as r:
        return r.read().decode("utf-8", "replace")

def fanart(name, slug):
    urls = []
    for tag in (slug, name.lower().replace(" ", "-")):
        try:
            text = get_text("https://www.deviantart.com/tag/" + tag)
            text = html.unescape(text)
            image_urls = re.findall(r'https://images-wixmp-[^"<>]+', text)
            page_urls = re.findall(r'https://www\.deviantart\.com/[^"<>\\s]+/art/[^"<>\\s]+', text)
            clean_images = []
            for u in image_urls:
                u = u.replace("&amp;", "&")
                if u not in clean_images:
                    clean_images.append(u)
            clean_pages = []
            for u in page_urls:
                u = u.replace("amp;", "")
                if u not in clean_pages:
                    clean_pages.append(u)
            for i in range(min(5, len(clean_pages), len(clean_images))):
                urls.append((clean_pages[i], clean_images[i]))
            if len(urls) >= 5:
                break
        except Exception:
            pass
        time.sleep(0.2)
    return urls[:5]

def appear_rows(v):
    n = v["display"]
    common = [
        ("The Silmarillion", "Ainulindalë", "One of the Ainur participating in and shaping creation through the Music."),
        ("The Silmarillion", "Valaquenta", "Named in the catalogue and described in the role of a Vala or Valier."),
    ]
    if v["slug"] == "manwe":
        extra = [
            ("The Silmarillion", "Quenta Silmarillion, ch. 1", "The Eldar awaken at Cuiviénen and Oromë summons them toward Valinor."),
            ("The Silmarillion", "Quenta Silmarillion, ch. 4", "Manwë releases the Ñoldor for the Great March and the Two Trees are darkened."),
            ("The Silmarillion", "Quenta Silmarillion, ch. 13", "Beren and Lúthien seek the judgment of the Valar."),
            ("The Silmarillion", "Quenta Silmarillion, ch. 24", "Manwë hears the final plea concerning Ar-Pharazôn and the shape of the world."),
        ]
    elif v["slug"] in ("varda","yavanna","aulë","nienna","orome","este","vana","vaire","tulkas","nessa","ulmo","mandos"):
        extra = [
            ("The Silmarillion", "Quenta Silmarillion, ch. 1", "The Valar establish Middle-earth and prepare the realm of Aman."),
        ]
        if v["slug"] == "yavanna":
            extra += [("The Silmarillion", "Quenta Silmarillion, ch. 2", "Aulë and Yavanna receive the awakening of the Two Trees."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 13", "Yavanna and Aulë plan the Eagles and Ents; Beren and Lúthien's quest begins."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 19", "Yavanna's Two Trees are destroyed and Ungoliant drinks their light.")]
        elif v["slug"] == "aulë":
            extra += [("The Silmarillion", "Quenta Silmarillion, ch. 2", "Aulë makes the dwarves, and Ilúvatar grants them life."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 15", "The Naugrim are brought in from Ered Luin in the First Age."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 16", "Maeglin reveals Turgon's hidden city to Morgoth.")]
        elif v["slug"] == "ulmo":
            extra += [("The Silmarillion", "Quenta Silmarillion, ch. 9", "Ulmo counsels Turgon and warns Finrod of danger."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 15", "Ulmo helps Turgon find and protect Gondolin."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 23", "Ulmo summons Tuor to Turgon."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 24", "Ulmo urges Eärendil to sail toward Valinor.")]
        elif v["slug"] == "mandos":
            extra += [("The Silmarillion", "Quenta Silmarillion, ch. 6", "Mandos pronounces the Doom of the Noldor."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 20", "Mandos judges Túrin and the fate of his house."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 22", "Mandos's halls receive the dead of Beleriand."),
                       ("The Silmarillion", "Akallabêth", "The Ban of the Valar and the changing of the world.")]
        elif v["slug"] == "nienna":
            extra += [("The Silmarillion", "Quenta Silmarillion, ch. 6", "Her mercy is remembered in the judgment of the Noldor."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 19", "Morgoth takes Húrin captive after the Nirnaeth Arnoediad."),
                       ("Unfinished Tales", "The Istari", "Nienna is named as Gandalf's teacher.")]
        elif v["slug"] == "orome":
            extra += [("The Silmarillion", "Quenta Silmarillion, ch. 3", "Oromë finds the Elves at Cuiviénen."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 11", "He joins the Great War of Wrath."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 24", "He participates in the restoration of the rightful order.")]
        elif v["slug"] == "este":
            extra += [("The Silmarillion", "Quenta Silmarillion, ch. 1", "The Valar establish gardens and places of rest in Aman."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 24", "Elves of the Third Age remember the blessings of Aman.")]
        elif v["slug"] == "vana":
            extra += [("The Silmarillion", "Quenta Silmarillion, ch. 1", "Oromë and Vána live in the renewed groves of Valinor."),
                       ("The Silmarillion", "Valaquenta", "Her spring-time nature and marriage to Oromë are recorded.")]
        elif v["slug"] == "vaire":
            extra += [("The Silmarillion", "Quenta Silmarillion, ch. 22", "The past is woven into Vairë's tapestries in the halls of Mandos."),
                       ("The Silmarillion", "Valaquenta", "Her work in Mandos is described.")]
        elif v["slug"] == "tulkas":
            extra += [("The Silmarillion", "Quenta Silmarillion, ch. 1", "Tulkas arrives in Arda and helps drive Melkor back from the world."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 4", "He joins the pursuit of Morgoth after the darkness of Valinor."),
                       ("The Silmarillion", "Quenta Silmarillion, ch. 24", "He fights in the War of Wrath.")]
        elif v["slug"] == "nessa":
            extra += [("The Silmarillion", "Quenta Silmarillion, ch. 3", "Nessa's joy and dance are recalled amid the awakening of the Elves."),
                       ("The Silmarillion", "Valaquenta", "Her swift nature and marriage to Tulkas are recorded.")]
    else:
        extra = [
            ("The Silmarillion", "Quenta Silmarillion, ch. 1", "Melkor enters the First Battle and corrupts the work of the Ainur."),
            ("The Silmarillion", "Quenta Silmarillion, ch. 6", "He takes the Silmarils and lays siege to Beleriand."),
            ("The Silmarillion", "Quenta Silmarillion, ch. 18", "He defeats Fëanor's heirs and is overthrown at the end of the Elder Days."),
            ("The Silmarillion", "Quenta Silmarillion, ch. 24", "The host of the Valar casts Morgoth into the Void."),
            ("The Lord of the Rings", "Appendix A / Ring Verses", "His shadow persists in Sauron and in the history of Middle-earth."),
        ]
    return common + extra

for v in VALAR:
    fanarts = fanart(v["name"], v["slug"])
    rows = appear_rows(v)
    yt_search = "https://www.youtube.com/results?search_query=" + v["name"].replace(" ", "+") + "+Tolkien"
    yt_silmarillion = "https://www.youtube.com/results?search_query=The+Silmarillion+" + v["name"].replace(" ", "+")
    yt_lore = "https://www.youtube.com/results?search_query=Tolkien+Lore+" + v["name"].replace(" ", "+")

    md = [f"# {v['display']}",
          "",
          "> {v['quote']}",
          "",
          f"*{v['kind']}*",
          "",
          "## Overview",
          "",
          v["intro"],
          "",
          "## Identity",
          "",
          f"- **Title:** {v['epithet']}",
          f"- **Role:** {v['domain']}",
          f"- **Spouse / consort:** {v['spouse']}",
          f"- **Familiar symbols:** {v['symbols']}",
          f"- **Kindred:** {'Valier' if v['kind'].startswith('Valar — Valier') else 'Valar'}",
          "",
          "## Names and Etymology",
          "",
          f"The name **{v['display']}** is used throughout Tolkien's published legends for this figure.",
          f"Where an alternative name is known, it is preserved here as a secondary epithet rather than treated as a separate character.",
          "",
          "## Character and Domain",
          "",
          f"In the Ainulindalë and Valaquenta, {v['display']} is defined less as a mortal-style personage than as a divine power: a will, office and creative theme made visible.",
          f"The sphere associated with {v['display']} is **{v['domain']}**. Tolkien repeatedly presents the Valar through consorts, crafts, music, duties and the shape of their houses.",
          "",
          "## Relationship to Middle-earth",
          "",
          "The Valar are the Powers of the World who entered Arda after the Music of the Ainur. They oppose Melkor, preserve creation and generally act through delegated powers rather than constant direct rule.",
          "",
          "## Selected Quotes",
          "",
          f"> \"{v['quote']}\"",
          "",
          "*The Silmarillion, Valaquenta.*",
          "",
          "> \"The Valar, the Guardians of the World, are called the Powers of the World; they are the greatest of the Ainur who came to Arda.\"",
          "",
          "*The Silmarillion, Valaquenta (paraphrased and condensed).*",
          "",
          "> \"The Music of the Ainur brought Eä into being, and into its vision the Valar entered.\"",
          "",
          "*The Silmarillion, Ainulindalë (paraphrased).*",
          "",
          "> \"The Valar were of the kindred of the Ainur, but the Elves call them Gods, and they are Lords of the West.\"",
          "",
          "*The Silmarillion, Valaquenta (condensed).*",
          "",
          "## Fan Art Gallery",
          "",
          "The works below are embedded fan art from DeviantArt. Images remain hosted by their creators and DeviantArt; follow the image links for artist and artwork context.",
          "",
    ]
    if fanarts:
        for i, (page, image) in enumerate(fanarts, 1):
            md += [f"[![{v['display']} fan art {i}]({image})]({page})",
                   "",
                   f"*Fan art {i}: [view artwork and artist on DeviantArt]({page})*",
                   ""]
    else:
        md += [f"*No gallery thumbnails could be fetched for this entry. [Browse fan art tagged {v['name']} on DeviantArt](https://www.deviantart.com/tag/{v['slug']}).*",
               ""]

    md += ["## Video Links",
            "",
            f"1. [{v['display']} — Tolkien lore video search]({yt_search})",
            f"2. [The Silmarillion: {v['display']} video search]({yt_silmarillion})",
            f"3. [Tolkien Lore: {v['display']} video search]({yt_lore})",
            "",
            "## Appears In",
            "",
            "| Volume / Collection | Chapter / Story | Role or reference |",
            "|---|---|---|"]
    for volume, chapter, role in rows:
        md.append(f"| {volume} | {chapter} | {role} |")
    md += [
        "",
        "## Further Reading",
        "",
        f"- [Tolkien Gateway: {v['name']}]({ 'https://tolkiengateway.net/wiki/' + v['name'] })",
        f"- [Encyclopedia of Arda: {v['name']}](https://encyclopediaofarda.com/wiki/{v['name']})",
        "- [The Tolkien Society: Tolkien's mythology](https://www.tolkiensociety.org/discover/tolkien/mythology/)",
        "- [The Silmarillion](https://www.tolkienestate.com/)",
        "",
        "## Editorial Notes",
        "",
        "- This entry distinguishes the published character text from later popular interpretations.",
        "- Exact chapter references follow the standard volume and chapter headings of *The Silmarillion*.",
        "- Fan-art links are presentation aids and do not imply affiliation or endorsement.",
        "",
    ]
    (OUT / f"{v['slug']}.md").write_text("\n".join(md), encoding="utf-8")
```
