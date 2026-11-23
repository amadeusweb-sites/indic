---

## write-elf-articles--py

**Introduction:** Writes the detailed Elf profile pages from the curated article data in the script. It can replace existing article prose and is therefore a source generator, not a harmless validator.

**How to use:** Set `TOLKIEN_ELVES_DIR`, back up the full tree, extract the fenced code to a local `.py` file, and run only when intentionally regenerating the profile set. Review output and preserve any later editorial changes.

```py
import os
from pathlib import Path

ROOT = Path(os.environ["TOLKIEN_ELVES_DIR"])


def A(chapter, context, volume="The Silmarillion"):
    return (volume, chapter, context)


ENTRIES = {
    "finwe": {
        "name": "Finwë", "kindred": "Noldor", "other": "High King of the Noldor",
        "family": "Father of Fëanor, Fingolfin, Finarfin, and their descendants; husband of Míriel and later Indis.",
        "overview": "Finwë is the first High King of the Noldor in Aman. His leadership guides his people from Cuiviénen to Valinor, where they become renowned for craft, learning, and the making of things.",
        "role": "His death at the hands of Morgoth is the immediate cause of Fëanor's oath and the Noldorin departure from Valinor. The political and family consequences of that death shape much of the First Age.",
        "appears": [
            A("Quenta Silmarillion, ch. 5", "Leads the Noldor in the westward journey and becomes their king in Tirion."),
            A("Quenta Silmarillion, ch. 6–7", "His households and sons are central to the unrest surrounding Fëanor."),
            A("Quenta Silmarillion, ch. 8–9", "Is slain by Morgoth in Formenos; the Noldor respond to his death."),
        ],
    },
    "miriel": {
        "name": "Míriel Serindë", "kindred": "Noldor", "other": "Serindë, the Broideress",
        "family": "First wife of Finwë and mother of Fëanor.",
        "overview": "Míriel is a Noldorin craftswoman celebrated for her skill in embroidery. After Fëanor's birth, she is overcome by weariness and chooses to leave embodied life, becoming the first of the Eldar to die in Aman.",
        "role": "Her departure creates an unprecedented question about marriage and death among the Eldar. Finwë's later marriage to Indis and the divided household of his sons become important background to the Noldorin conflict.",
        "appears": [
            A("Quenta Silmarillion, ch. 6", "Her weariness after Fëanor's birth and her departure to the gardens of Lórien are recounted."),
            A("Quenta Silmarillion, ch. 7", "The consequences of her death and Finwë's remarriage are discussed."),
        ],
    },
    "feanor": {
        "name": "Fëanor", "kindred": "Noldor", "other": "Fëanáro; Curufinwë",
        "family": "Son of Finwë and Míriel; husband of Nerdanel; father of Maedhros, Maglor, Celegorm, Caranthir, Curufin, Amrod, and Amras.",
        "overview": "Fëanor is the greatest craftworker among the Noldor and the maker of the Silmarils, jewels that preserve the light of the Two Trees. His brilliance is matched by a fierce independence and a refusal to yield what he considers his own.",
        "role": "After Morgoth steals the Silmarils and kills Finwë, Fëanor swears a binding oath with his sons. Their pursuit of the jewels brings the Noldor into exile and repeatedly turns kin against kin.",
        "appears": [
            A("Quenta Silmarillion, ch. 6–8", "His birth, craft, conflict with Melkor, and the making of the Silmarils are central."),
            A("Quenta Silmarillion, ch. 9", "He leads the Flight of the Noldor and the first Kinslaying."),
            A("Quenta Silmarillion, ch. 13", "His death and the inheritance of his oath affect the return of the Noldor."),
        ],
    },
    "nerdanel": {
        "name": "Nerdanel", "kindred": "Noldor", "other": "Nerdanel the Wise",
        "family": "Wife of Fëanor and mother of his seven sons; daughter of the smith Mahtan.",
        "overview": "Nerdanel is a Noldorin sculptor and craftswoman whose independent judgment distinguishes her from Fëanor. She is remembered for making lifelike figures in stone and metal and for understanding her husband's difficult temperament.",
        "role": "Nerdanel's relationship with Fëanor becomes strained as his pride and anger grow. She does not accompany him into exile, leaving the sons of Fëanor to follow their father's oath without her.",
        "appears": [
            A("Quenta Silmarillion, ch. 6–7", "Her marriage to Fëanor, their sons, and her separation from him are noted."),
        ],
    },
    "maedhros": {
        "name": "Maedhros", "kindred": "Noldor", "other": "Maitimo; Russandol",
        "family": "Eldest son of Fëanor and Nerdanel; brother of Maglor, Celegorm, Caranthir, Curufin, Amrod, and Amras.",
        "overview": "Maedhros is the eldest son of Fëanor. Captured by Morgoth after the Dagor-nuin-Giliath, he is chained to Thangorodrim and rescued by Fingon, an act that begins a lasting friendship between the houses of Fëanor and Fingolfin.",
        "role": "Maedhros tries at times to restrain his brothers and build unity among the Noldor, but remains bound by the oath. His later choices show both the possibility of reconciliation and the destructive force of that vow.",
        "appears": [
            A("Quenta Silmarillion, ch. 9, 13", "His capture, rescue by Fingon, and reconciliation with Fingolfin's house are told."),
            A("Quenta Silmarillion, ch. 18, 20", "He leads the Union of Maedhros and fights in the Nirnaeth Arnoediad."),
            A("Quenta Silmarillion, ch. 24", "The last attempt to recover the Silmarils ends in his death."),
        ],
    },
    "maglor": {
        "name": "Maglor", "kindred": "Noldor", "other": "Makalaurë; Kanafinwë",
        "family": "Second son of Fëanor and Nerdanel; brother of Maedhros and the other sons of Fëanor.",
        "overview": "Maglor is a renowned singer and poet as well as a son of Fëanor. He rules in eastern Beleriand with Maedhros and remains one of the few members of his house to survive the wars against Morgoth.",
        "role": "Maglor's history is inseparable from the oath sworn by the sons of Fëanor. In the final attempt to seize the Silmarils, he takes one but is unable to keep it; the text leaves him wandering beside the sea in grief.",
        "appears": [
            A("Quenta Silmarillion, ch. 9, 13", "The oath and the realms of the sons of Fëanor establish his role."),
            A("Quenta Silmarillion, ch. 18, 20, 24", "He fights in the wars of Beleriand and survives the final disaster of the oath."),
        ],
    },
    "celegorm": {
        "name": "Celegorm", "kindred": "Noldor", "other": "Celegorm the Fair",
        "family": "Third son of Fëanor and Nerdanel; brother of Maedhros, Maglor, Caranthir, Curufin, Amrod, and Amras.",
        "overview": "Celegorm is a skilled hunter and friend of Oromë in Valinor. In Middle-earth he rules in Himlad with his brother Curufin and becomes closely associated with the hound Huan.",
        "role": "His actions in Nargothrond and his attempt to claim Lúthien for himself contribute to the collapse of his political standing. He remains bound to the oath of Fëanor until his death in the attack on Doriath.",
        "appears": [
            A("Quenta Silmarillion, ch. 9, 13", "His exile and settlement in Himlad are described."),
            A("Quenta Silmarillion, ch. 19", "He and Curufin seize Lúthien, but Huan helps her escape."),
            A("Quenta Silmarillion, ch. 22, 24", "He takes part in the Kinslaying at Doriath and dies in the assault on the Havens."),
        ],
    },
    "caranthir": {
        "name": "Caranthir", "kindred": "Noldor", "other": "Carnistir; the Dark",
        "family": "Fourth son of Fëanor and Nerdanel; brother of the other sons of Fëanor.",
        "overview": "Caranthir is the stern and quick-tempered son of Fëanor who rules in Thargelion, east of Doriath. His lands border the Dwarves of the Blue Mountains, with whom he establishes trade and contact.",
        "role": "Caranthir's dealings with the Dwarves show a practical side to his rule, but suspicion and hostility shape his relations with other peoples. He dies with his brothers during the assault on Doriath.",
        "appears": [
            A("Quenta Silmarillion, ch. 13–15", "He establishes a realm in Thargelion and makes contact with the Dwarves."),
            A("Quenta Silmarillion, ch. 20, 22, 24", "He fights in the wars of Beleriand and is slain in the Kinslaying at Doriath."),
        ],
    },
    "curufin": {
        "name": "Curufin", "kindred": "Noldor", "other": "Curufinwë",
        "family": "Fifth son of Fëanor and Nerdanel; father of Celebrimbor.",
        "overview": "Curufin inherits his father's name and much of his skill in craft. He rules in Himlad alongside Celegorm and later joins him in Nargothrond.",
        "role": "Curufin's ambition and political maneuvering help alienate the people of Nargothrond from the sons of Fëanor. His son Celebrimbor rejects his deeds and remains in the city after Curufin is expelled.",
        "appears": [
            A("Quenta Silmarillion, ch. 13", "He establishes a realm with Celegorm in Himlad."),
            A("Quenta Silmarillion, ch. 19", "He attempts to seize Lúthien and is expelled from Nargothrond."),
            A("Quenta Silmarillion, ch. 22, 24", "He is involved in the Kinslayings at Doriath and the Havens."),
        ],
    },
    "amrod": {
        "name": "Amrod", "kindred": "Noldor", "other": "One of the twins",
        "family": "Youngest son of Fëanor and Nerdanel; twin brother of Amras.",
        "overview": "Amrod is one of the twin sons of Fëanor. He travels with the Noldor into exile and shares the eastern lands of Beleriand with his brothers.",
        "role": "The published account gives Amrod little individual speech or action. His significance lies chiefly in the collective history of the sons of Fëanor and their continuing pursuit of the Silmarils.",
        "appears": [
            A("Quenta Silmarillion, ch. 9, 13", "He is named among the sons who follow Fëanor into exile and settle in Beleriand."),
            A("Quenta Silmarillion, ch. 22, 24", "He is counted among the sons of Fëanor in the later Kinslayings."),
        ],
    },
    "amras": {
        "name": "Amras", "kindred": "Noldor", "other": "One of the twins",
        "family": "Youngest son of Fëanor and Nerdanel; twin brother of Amrod.",
        "overview": "Amras is the twin brother of Amrod and the youngest of Fëanor's sons. Like his brothers, he leaves Aman under the oath and establishes himself in the east of Beleriand.",
        "role": "The published narrative treats the twins largely together. Amras's place in the legendarium is as a member of Fëanor's house whose fate is bound to the oath and the losses it causes.",
        "appears": [
            A("Quenta Silmarillion, ch. 9, 13", "He is named among the sons of Fëanor in the Flight of the Noldor and settlement of Beleriand."),
            A("Quenta Silmarillion, ch. 22, 24", "He is included among the sons involved in the later Kinslayings."),
        ],
    },
    "celebrimbor": {
        "name": "Celebrimbor", "kindred": "Noldor", "other": "Celebrimbor of Eregion",
        "family": "Son of Curufin and grandson of Fëanor.",
        "overview": "Celebrimbor is a master-smith of the Noldor and the last prominent member of Fëanor's line in Middle-earth. He rejects his father's conduct in Nargothrond and later becomes the leading craftsman of Eregion.",
        "role": "In the Second Age he works with the smiths of Eregion and is deceived by Sauron, who helps create the Rings of Power. Celebrimbor's story links the First Age legacy of the Silmarils to the central conflict of the later ages.",
        "appears": [
            A("Quenta Silmarillion, ch. 19", "He rejects Curufin and remains in Nargothrond after his father's expulsion."),
            A("The Silmarillion, Of the Rings of Power and the Third Age", "He leads the smiths of Eregion and participates in the making of the Rings."),
        ],
    },
    "fingolfin": {
        "name": "Fingolfin", "kindred": "Noldor", "other": "Nolofinwë",
        "family": "Son of Finwë and Indis; father of Fingon, Turgon, and Aredhel.",
        "overview": "Fingolfin is the second son of Finwë and the leader of the largest host of Noldor to cross the Helcaraxë. After Fëanor's death, he becomes High King of the Noldor in Middle-earth.",
        "role": "His long defiance of Morgoth culminates in a single combat outside Angband. Though he wounds Morgoth, he is killed; the courage of that challenge becomes one of the defining memories of the First Age.",
        "appears": [
            A("Quenta Silmarillion, ch. 7–9", "The division of Finwë's house and the Flight of the Noldor shape his choices."),
            A("Quenta Silmarillion, ch. 13", "He leads his host into Middle-earth and is acknowledged as High King."),
            A("Quenta Silmarillion, ch. 18", "He rides alone to Angband and is slain by Morgoth."),
        ],
    },
    "fingon": {
        "name": "Fingon", "kindred": "Noldor", "other": "Findekáno",
        "family": "Eldest son of Fingolfin; brother of Turgon and Aredhel; father of Gil-galad in the published Silmarillion's stated genealogy.",
        "overview": "Fingon is a Noldorin prince remembered for rescuing Maedhros from Thangorodrim. His friendship with Maedhros helps heal the division between their houses and makes possible a period of cooperation among the Noldor.",
        "role": "As High King after Fingolfin, Fingon leads the Noldor in the Nirnaeth Arnoediad. The defeat of that battle ends his reign and leaves the Noldor in Beleriand greatly weakened.",
        "appears": [
            A("Quenta Silmarillion, ch. 9, 13", "He joins the exile and rescues Maedhros, helping reconcile the Noldorin houses."),
            A("Quenta Silmarillion, ch. 18, 20", "He becomes High King and is slain in the Nirnaeth Arnoediad."),
            A("The Silmarillion, Of the Rings of Power and the Third Age", "Gil-galad's descent is given in the published genealogy."),
        ],
    },
    "turgon": {
        "name": "Turgon", "kindred": "Noldor", "other": "Turukáno; King of Gondolin",
        "family": "Son of Fingolfin and Indis; husband of Elenwë; father of Idril.",
        "overview": "Turgon is the founder and king of Gondolin, the hidden city built in the valley of Tumladen. Guided by Ulmo, he discovers the site after the Dagor Aglareb and brings his people there in secrecy.",
        "role": "Turgon's commitment to keeping Gondolin hidden protects it for centuries but also delays its evacuation when danger grows. The city is ultimately betrayed by Maeglin and destroyed; Turgon dies defending it.",
        "appears": [
            A("Quenta Silmarillion, ch. 9, 13–15", "He crosses the Helcaraxë, discovers Tumladen, and establishes Gondolin."),
            A("Quenta Silmarillion, ch. 18, 20", "Gondolin remains hidden through the great wars of Beleriand."),
            A("Quenta Silmarillion, ch. 23–24", "The city falls and Turgon dies; its survivors join the later struggle."),
        ],
    },
    "aredhel": {
        "name": "Aredhel", "kindred": "Noldor", "other": "Ar-Feiniel; the White Lady of the Noldor",
        "family": "Daughter of Fingolfin and Indis; sister of Fingon and Turgon; mother of Maeglin.",
        "overview": "Aredhel is a Noldorin princess who loves riding and the forests. Leaving Gondolin with permission from Turgon, she is separated from her companions and eventually reaches Nan Elmoth.",
        "role": "In Nan Elmoth she marries Eöl and gives birth to Maeglin. Her attempt to return to Gondolin ends in tragedy when Eöl pursues them and casts a poisoned javelin; Aredhel dies from the wound.",
        "appears": [
            A("Quenta Silmarillion, ch. 14", "Her journey from Gondolin and marriage to Eöl are recounted."),
            A("Quenta Silmarillion, ch. 16", "She returns to Gondolin with Maeglin and dies from Eöl's poisoned weapon."),
        ],
    },
    "idril": {
        "name": "Idril Celebrindal", "kindred": "Noldor", "other": "Celebrindal; Silver-foot",
        "family": "Daughter of Turgon and Elenwë; wife of Tuor; mother of Eärendil.",
        "overview": "Idril is a princess of Gondolin known for foresight and sound judgment. She distrusts Maeglin and secretly prepares a hidden way out of the city before its fall.",
        "role": "When Gondolin is attacked, Idril leads survivors through the escape route she had arranged. She later sails with Tuor and Eärendil from the Havens of Sirion, joining the family line that leads to Elrond and Elros.",
        "appears": [
            A("Quenta Silmarillion, ch. 16", "Her relationship to Maeglin and the royal house is introduced."),
            A("Quenta Silmarillion, ch. 23–24", "She helps escape the fall of Gondolin and later sails with Tuor."),
        ],
    },
    "maeglin": {
        "name": "Maeglin", "kindred": "Noldor and Sindarin descent", "other": "Lómion",
        "family": "Son of Aredhel, a Noldo, and Eöl, a Sinda; nephew of Turgon.",
        "overview": "Born Lómion, Maeglin is raised in Nan Elmoth by Eöl and later follows his mother to Gondolin. He becomes a skilled miner and a powerful lord in the hidden city.",
        "role": "Maeglin's desire for Idril is rejected, and his capture by Morgoth exposes the location and defenses of Gondolin. He betrays the city in exchange for promises that are not honored and is killed during its fall.",
        "appears": [
            A("Quenta Silmarillion, ch. 16", "His birth, upbringing, and arrival in Gondolin are told."),
            A("Quenta Silmarillion, ch. 23", "He betrays Gondolin and dies during the city's destruction."),
        ],
    },
    "finarfin": {
        "name": "Finarfin", "kindred": "Noldor", "other": "Arafinwë",
        "family": "Son of Finwë and Indis; husband of Eärwen; father of Finrod, Angrod, Aegnor, Orodreth, and Galadriel.",
        "overview": "Finarfin is the youngest son of Finwë and the Noldorin prince most closely connected to the Teleri through his wife Eärwen. He turns back from the march into exile after the Kinslaying at Alqualondë.",
        "role": "Finarfin returns to Tirion and becomes High King of the Noldor who remain in Aman. His sons and daughter, however, take part in the wars of Beleriand, making his household a bridge between the two branches of the Noldor.",
        "appears": [
            A("Quenta Silmarillion, ch. 5–9", "His family, marriage, and decision to return to Aman are described."),
            A("Quenta Silmarillion, ch. 24", "The Noldor of Aman join the War of Wrath under his leadership."),
        ],
    },
    "finrod": {
        "name": "Finrod Felagund", "kindred": "Noldor", "other": "Findaráto; Felagund",
        "family": "Eldest son of Finarfin and Eärwen; brother of Angrod, Aegnor, Orodreth, and Galadriel; beloved of Amarië.",
        "overview": "Finrod is a Noldorin prince and the founder of Nargothrond. He is known for generosity, wisdom, and his friendship with the Men of Bëor's house.",
        "role": "Bound by an oath to Beren, Finrod leaves his kingdom to help retrieve a Silmaril from Angband. He dies in the dungeons of Sauron after defending Beren from a werewolf, fulfilling his promise at the cost of his life.",
        "appears": [
            A("Quenta Silmarillion, ch. 13–15", "He establishes Nargothrond and forges alliances with Men."),
            A("Quenta Silmarillion, ch. 17, 19", "He swears to aid Beren and accompanies him on the quest."),
            A("Quenta Silmarillion, ch. 19", "He dies protecting Beren in Sauron's dungeon."),
        ],
    },
    "angrod": {
        "name": "Angrod", "kindred": "Noldor", "other": "Angaráto",
        "family": "Son of Finarfin and Eärwen; brother of Finrod, Aegnor, Orodreth, and Galadriel.",
        "overview": "Angrod is a prince of the Noldor who settles in the northern highlands of Dorthonion with his brother Aegnor. He serves as one of the Noldorin leaders guarding the approaches to Dorthonion.",
        "role": "Angrod is killed with Aegnor during the Dagor Bragollach. His death marks the destruction of the Noldorin defenses in that region and deepens the losses suffered by Finarfin's house.",
        "appears": [
            A("Quenta Silmarillion, ch. 13–15", "He settles in Dorthonion and is named among the Noldorin princes."),
            A("Quenta Silmarillion, ch. 18", "He is slain with Aegnor in the Dagor Bragollach."),
        ],
    },
    "aegnor": {
        "name": "Aegnor", "kindred": "Noldor", "other": "Aikanáro",
        "family": "Son of Finarfin and Eärwen; brother of Finrod, Angrod, Orodreth, and Galadriel.",
        "overview": "Aegnor is a Noldorin prince who rules in Dorthonion with Angrod. In later accounts of the family, he is remembered for his unfulfilled love for the mortal Andreth, though that story is developed outside the published Silmarillion.",
        "role": "Aegnor refuses to marry during the war and remains in Middle-earth. He is killed with Angrod in the sudden assault of the Dagor Bragollach.",
        "appears": [
            A("Quenta Silmarillion, ch. 13–15", "He is named among the princes who settle in Dorthonion."),
            A("Quenta Silmarillion, ch. 18", "He dies defending Dorthonion in the Dagor Bragollach."),
        ],
    },
    "orodreth": {
        "name": "Orodreth", "kindred": "Noldor", "other": "Artaresto",
        "family": "Son of Finarfin and Eärwen; brother of Finrod, Angrod, Aegnor, and Galadriel; father of Finduilas.",
        "overview": "Orodreth is a prince of the Noldor who rules Nargothrond after Finrod's death. His daughter Finduilas is loved by Gwindor and later becomes central to Túrin's story.",
        "role": "Orodreth is reluctant to expose Nargothrond to open war, but Túrin's influence shifts the kingdom toward confrontation. The dragon Glaurung's assault destroys the city and ends Orodreth's rule.",
        "appears": [
            A("Quenta Silmarillion, ch. 15, 19", "He is named in the founding and leadership of Nargothrond."),
            A("Quenta Silmarillion, ch. 21", "His daughter Finduilas and Túrin's arrival affect the kingdom's policy."),
            A("Quenta Silmarillion, ch. 21–22", "Nargothrond falls and Orodreth is slain."),
        ],
    },
    "galadriel": {
        "name": "Galadriel", "kindred": "Noldor", "other": "Artanis; Nerwen",
        "family": "Daughter of Finarfin and Eärwen; sister of Finrod, Angrod, Aegnor, and Orodreth; wife of Celeborn.",
        "overview": "Galadriel is one of the leaders of the Noldor who leave Aman, though she takes no part in the Kinslaying at Alqualondë. In Middle-earth she dwells for a time in Doriath and later becomes a major figure of the Second and Third Ages.",
        "role": "The published Silmarillion gives a brief account of her early life and the long ban that follows the rebellion of the Noldor. Her later history is connected with the Rings of Power and the survival of the Eldar in Middle-earth.",
        "appears": [
            A("Quenta Silmarillion, ch. 5–9", "Her lineage and departure from Aman are established."),
            A("Quenta Silmarillion, ch. 15, 19", "She is associated with Doriath and later travels with Celeborn."),
            A("The Silmarillion, Of the Rings of Power and the Third Age", "She is among the leaders of the Eldar in the later ages."),
        ],
    },
    "finduilas": {
        "name": "Finduilas", "kindred": "Noldor", "other": "Finduilas of Nargothrond",
        "family": "Daughter of Orodreth and sister of Gil-galad in some later genealogies; the published Silmarillion identifies her as Orodreth's daughter.",
        "overview": "Finduilas is a princess of Nargothrond. Gwindor loves her, and she in turn cares for him, but her life is changed when Túrin arrives and becomes a celebrated leader in the city.",
        "role": "Finduilas recognizes the danger of Túrin's influence and tries to persuade him to abandon the war against Morgoth. During the fall of Nargothrond she is captured by Orcs and killed before Túrin can rescue her.",
        "appears": [
            A("Quenta Silmarillion, ch. 21", "Her relationships with Gwindor and Túrin are central to the Nargothrond story."),
            A("Quenta Silmarillion, ch. 21", "She is captured during the sack of Nargothrond and dies in captivity."),
        ],
    },
    "gil-galad": {
        "name": "Gil-galad", "kindred": "Noldor", "other": "Ereinion",
        "family": "The published Silmarillion identifies him as a son of Fingon; Tolkien's later writings revise his parentage.",
        "overview": "Gil-galad is the last High King of the Noldor in Middle-earth. After the fall of the great First Age kingdoms, he rules from Lindon and leads the Elves of the West in the Second Age.",
        "role": "Gil-galad resists Sauron's rise, forms the Last Alliance with Elendil, and dies in the War of the Last Alliance. The Rings of Power account presents him as one of the principal defenders of the Free Peoples.",
        "appears": [
            A("The Silmarillion, Of the Rings of Power and the Third Age", "His kingship, resistance to Sauron, and death in the Last Alliance are recounted."),
        ],
    },
    "gwindor": {
        "name": "Gwindor", "kindred": "Noldor", "other": "Lord of Nargothrond",
        "family": "Son of Guilin; brother of Gelmir; beloved of Finduilas.",
        "overview": "Gwindor is a lord of Nargothrond who is captured during the Nirnaeth Arnoediad and forced to labor in Angband. He escapes and returns to Beleriand with Túrin, bringing news of the enemy's strength.",
        "role": "Gwindor's return changes the course of Nargothrond's politics. He loves Finduilas but sees her drawn to Túrin; wounded and diminished, he dies in the fall of the kingdom after trying to save her.",
        "appears": [
            A("Quenta Silmarillion, ch. 20", "He is captured during the Nirnaeth Arnoediad."),
            A("Quenta Silmarillion, ch. 21", "He escapes Angband, brings Túrin to Nargothrond, and dies in the city's fall."),
        ],
    },
    "gelmir": {
        "name": "Gelmir", "kindred": "Noldor", "other": "Gelmir of Nargothrond",
        "family": "Son of Guilin and brother of Gwindor.",
        "overview": "Gelmir is a Noldorin Elf captured by Morgoth's forces. His blinding and killing before the Nirnaeth Arnoediad are witnessed by the army of Fingon and provoke a desperate charge.",
        "role": "Gelmir's fate is one of the events that intensifies the battle's opening. His brother Gwindor leads the first assault, and the Noldor's early success gives way to catastrophic defeat.",
        "appears": [
            A("Quenta Silmarillion, ch. 20", "His public execution at the start of the Nirnaeth Arnoediad provokes the Noldorin charge."),
        ],
    },
    "guilin": {
        "name": "Guilin", "kindred": "Noldor", "other": "Noldorin father of Gwindor and Gelmir",
        "family": "Father of the Noldorin brothers Gwindor and Gelmir.",
        "overview": "Guilin is named in the account of the Nirnaeth Arnoediad through his sons, the Noldorin brothers Gwindor and Gelmir. The published narrative gives little independent biography for him.",
        "role": "His name connects the personal tragedy of Gelmir's capture and Gwindor's response to the wider military disaster. The family relationship is part of the emotional context of the battle.",
        "appears": [
            A("Quenta Silmarillion, ch. 20", "He is identified as the father of Gelmir and Gwindor in the battle narrative."),
        ],
    },
    "edrahil": {
        "name": "Edrahil", "kindred": "Noldor", "other": "Elf-lord of Nargothrond",
        "family": "A lord of Nargothrond and a faithful companion of Finrod.",
        "overview": "Edrahil is one of the Noldor who accompanies Finrod on the quest to aid Beren. He and the other companions disguise themselves as Orcs, but Sauron discovers them and imprisons them.",
        "role": "Edrahil dies in Sauron's dungeon. His death precedes Finrod's final sacrifice and underscores the cost of the oath Finrod made to Beren.",
        "appears": [
            A("Quenta Silmarillion, ch. 19", "He accompanies Finrod and Beren, is captured by Sauron, and dies in captivity."),
        ],
    },
    "arminas": {
        "name": "Arminas", "kindred": "Noldor", "other": "Noldorin messenger",
        "family": "A Noldorin Elf associated with the people of Angrod and Aegnor; companion of Gelmir in the message to Turgon.",
        "overview": "Arminas is one of the Noldorin messengers who reaches Turgon with a warning from Ulmo. He urges the king of Gondolin to prepare for danger and to consider opening a way to the sea.",
        "role": "His warning connects the hidden city of Gondolin with the wider struggle of Beleriand. The message is not acted upon in time to prevent the city's eventual betrayal and fall.",
        "appears": [
            A("Quenta Silmarillion, ch. 23", "He and Gelmir bring Ulmo's warning to Turgon in Gondolin."),
        ],
    },
    "rumil": {
        "name": "Rúmil", "kindred": "Noldor", "other": "Rúmil of Tirion",
        "family": "A loremaster of the Noldor in Tirion.",
        "overview": "Rúmil is a learned Elf of Tirion associated with the preservation of lore and the invention of a writing system. The published Silmarillion presents him chiefly as a scholar rather than as an actor in the wars of Beleriand.",
        "role": "His name represents the Noldorin tradition of record-keeping and linguistic scholarship. References to Rúmil also frame the transmission of early histories about the Valar and the Eldar.",
        "appears": [
            A("Quenta Silmarillion, ch. 5", "He is associated with the lore and writing of the Noldor in Aman."),
        ],
    },
    "galdor": {
        "name": "Galdor", "kindred": "Noldor", "other": "Galdor of Gondolin",
        "family": "A lord of Gondolin; the published account gives little family detail.",
        "overview": "Galdor is named among the lords of Gondolin who take part in the city's defense. His identity should be distinguished from other Tolkien characters who share the name.",
        "role": "The published Silmarillion mentions Galdor in the story of Gondolin, but provides little individual biography beyond his place among the city's defenders.",
        "appears": [
            A("Quenta Silmarillion, ch. 23", "He is named among the lords and defenders of Gondolin."),
        ],
    },
    "ecthelion": {
        "name": "Ecthelion", "kindred": "Noldor", "other": "Ecthelion of the Fountain",
        "family": "A lord of Gondolin and captain of the House of the Fountain.",
        "overview": "Ecthelion is one of the foremost warriors of Gondolin. During the city's fall he confronts Gothmog, Lord of Balrogs, at the Fountain Square.",
        "role": "Ecthelion kills Gothmog but is mortally wounded in the struggle. His stand becomes one of the defining acts of resistance during the destruction of Gondolin.",
        "appears": [
            A("Quenta Silmarillion, ch. 23", "He fights in the defense of Gondolin and kills Gothmog before falling himself."),
        ],
    },
    "glorfindel": {
        "name": "Glorfindel", "kindred": "Noldor", "other": "Lord of the Golden Flower",
        "family": "A lord of Gondolin; leader of the House of the Golden Flower.",
        "overview": "Glorfindel is a prominent lord of Gondolin. During the city's escape, he fights a Balrog at the pass of Cirith Thoronath so that Tuor's company can get away.",
        "role": "Glorfindel and the Balrog both fall from the mountain path. His sacrifice saves many refugees, and his name is later borne by a returned Elf-lord in the Third Age.",
        "appears": [
            A("Quenta Silmarillion, ch. 23", "He sacrifices himself fighting a Balrog while protecting Gondolin's refugees."),
            A("The Silmarillion, Of the Rings of Power and the Third Age", "The later Glorfindel is associated with the history of the Eldar in Middle-earth."),
        ],
    },
    "voronwe": {
        "name": "Voronwë", "kindred": "Noldor", "other": "Voronwë of Gondolin",
        "family": "A mariner of Gondolin; son of Aranwë in later accounts.",
        "overview": "Voronwë is a mariner sent by Turgon to seek a way to Aman. His ship is lost in a storm, but he survives and returns to Middle-earth, where he meets Tuor.",
        "role": "As Tuor's guide, Voronwë leads him toward Gondolin and helps deliver Ulmo's message to Turgon. His survival makes him a crucial link between the hidden city and the events beyond it.",
        "appears": [
            A("Quenta Silmarillion, ch. 23", "He meets Tuor, guides him to Gondolin, and carries Ulmo's warning to Turgon."),
        ],
    },
    "egalmoth": {
        "name": "Egalmoth", "kindred": "Noldor", "other": "Lord of Gondolin",
        "family": "A lord of Gondolin; the published narrative gives little family detail.",
        "overview": "Egalmoth is named among the Noldorin lords of Gondolin. His individual story is brief in the published account, which focuses on the city's fall rather than on the full histories of its houses.",
        "role": "His inclusion among Gondolin's lords records the breadth of the city's leadership and the loss of its people during the attack by Morgoth's forces.",
        "appears": [
            A("Quenta Silmarillion, ch. 23", "He is named among the lords of Gondolin during the city's fall."),
        ],
    },
    "ingwe": {
        "name": "Ingwë", "kindred": "Vanyar", "other": "High King of the Vanyar",
        "family": "Leader of the Vanyar and one of the ambassadors who first journey to Valinor.",
        "overview": "Ingwë is the High King of the Vanyar and is regarded as the highest-ranking among the Elves. He dwells in Aman and does not lead his people into the rebellion and exile of the Noldor.",
        "role": "The Vanyar remain loyal to the Valar and later march with the Host of Valinor in the War of Wrath. Ingwë's kingship represents the Eldar who remained in the West.",
        "appears": [
            A("Quenta Silmarillion, ch. 5", "He leads the Vanyar to Aman and becomes their king."),
            A("Quenta Silmarillion, ch. 9, 24", "The Vanyar remain in the West and join the War of Wrath."),
        ],
    },
    "indis": {
        "name": "Indis", "kindred": "Vanyar", "other": "Indis of the Vanyar",
        "family": "A Vanyarin Elf; second wife of Finwë and mother of Fingolfin and Finarfin.",
        "overview": "Indis is a Vanyarin Elf who marries Finwë after the death of Míriel. Her marriage creates a new branch of Finwë's family and links the Noldor to the Vanyar.",
        "role": "The different inheritances of Fëanor and his half-brothers become part of the political and personal tensions leading to the Flight of the Noldor. Indis herself remains in Aman.",
        "appears": [
            A("Quenta Silmarillion, ch. 6–7", "Her marriage to Finwë and the birth of Fingolfin and Finarfin are recounted."),
        ],
    },
    "elenwe": {
        "name": "Elenwë", "kindred": "Vanyar", "other": "Elenwë of the Vanyar",
        "family": "Wife of Turgon and mother of Idril.",
        "overview": "Elenwë is a Vanyarin Elf who leaves Aman with the Noldor. During the crossing of the Helcaraxë she is lost in the ice, while Turgon and their daughter Idril survive.",
        "role": "Her death marks the cost of the Noldor's exile and remains a defining loss for Turgon's family. Her Vanyarin origin is a reminder that the exile included individuals from more than one Elven kindred.",
        "appears": [
            A("Quenta Silmarillion, ch. 9", "She dies in the crossing of the Helcaraxë."),
        ],
    },
    "elemmire": {
        "name": "Elemmírë", "kindred": "Vanyar", "other": "Composer of the Aldudénië",
        "family": "A Vanyarin Elf and loremaster or poet.",
        "overview": "Elemmírë is remembered as the composer of the Aldudénië, the lament for the destruction of the Two Trees. The published account gives no further personal history.",
        "role": "The song preserves the grief of the Elves of Aman and gives a name to the cultural memory of the Darkening of Valinor.",
        "appears": [
            A("Quenta Silmarillion, ch. 8", "The Aldudénië is named among the laments for the Two Trees."),
        ],
    },
    "amarie": {
        "name": "Amarië", "kindred": "Vanyar", "other": "Amarië of the Vanyar",
        "family": "A Vanyarin Elf loved by Finrod Felagund.",
        "overview": "Amarië remains in Aman when Finrod departs with the Noldor. Finrod does not marry during his life in Middle-earth, and the account connects this choice with his love for her.",
        "role": "Though she does not take part in the wars of Beleriand, Amarië is part of Finrod's personal history and helps explain why he remains unmarried in exile.",
        "appears": [
            A("Quenta Silmarillion, ch. 5", "She is identified as a Vanya beloved by Finrod."),
            A("Quenta Silmarillion, ch. 19", "Her relationship to Finrod's life in exile is recalled."),
        ],
    },
    "olwe": {
        "name": "Olwë", "kindred": "Teleri", "other": "King of the Teleri in Aman",
        "family": "Brother of Elwë Thingol; father of Eärwen.",
        "overview": "Olwë is one of the leaders of the Teleri and becomes their king in Aman after the division of his people. He rules the coastal city of Alqualondë, where the Teleri build their ships.",
        "role": "Olwë refuses Fëanor's demand for ships. The ensuing Kinslaying at Alqualondë marks a turning point in the Noldor's rebellion and in the relationship between the Eldar.",
        "appears": [
            A("Quenta Silmarillion, ch. 3–5", "He leads the Teleri and establishes their settlement in Aman."),
            A("Quenta Silmarillion, ch. 9", "His refusal to give ships to Fëanor leads to the Kinslaying at Alqualondë."),
        ],
    },
    "earwen": {
        "name": "Eärwen", "kindred": "Teleri", "other": "Eärwen of Alqualondë",
        "family": "Daughter of Olwë; wife of Finarfin; mother of Finrod, Angrod, Aegnor, Orodreth, and Galadriel.",
        "overview": "Eärwen is a princess of the Teleri of Alqualondë who marries Finarfin, joining the Telerin and Noldorin royal houses. Her sons and daughter become prominent leaders in the wars of Beleriand.",
        "role": "Her kinship with Olwë makes the Kinslaying at Alqualondë a family tragedy as well as a political rupture. The published Silmarillion gives little direct action for Eärwen herself.",
        "appears": [
            A("Quenta Silmarillion, ch. 5–9", "Her marriage to Finarfin links the Teleri and Noldor; her kin are divided by the Kinslaying."),
        ],
    },
    "thingol": {
        "name": "Thingol", "kindred": "Moriquendi — Sindar", "other": "Elwë Singollo; Elu Thingol",
        "family": "Brother of Olwë; husband of Melian; father of Lúthien; grandfather of Dior.",
        "overview": "Thingol is the Sindarin name of Elwë, one of the leaders of the Teleri who remains in Middle-earth after meeting Melian. He rules Doriath from Menegroth and becomes the chief king of the Sindar.",
        "role": "Thingol's protection of Doriath and his changing relations with the Noldor shape much of Beleriand's history. His demand that Beren bring a Silmaril leads to the quest of Beren and Lúthien; his own desire for the jewel later brings ruin upon Doriath.",
        "appears": [
            A("Quenta Silmarillion, ch. 3–4", "Elwë is lost from the Great Journey, meets Melian, and establishes Doriath."),
            A("Quenta Silmarillion, ch. 10, 14–15", "He rules the Sindar and sets limits on the Noldor in Beleriand."),
            A("Quenta Silmarillion, ch. 19, 22", "His dealings with Beren and the Silmaril lead to the ruin of Doriath."),
        ],
    },
    "lenwe": {
        "name": "Lenwë", "kindred": "Moriquendi — Nandor", "other": "Leader of the Nandor",
        "family": "A leader of the Teleri who turn aside from the Great Journey; father of Denethor in later genealogical accounts.",
        "overview": "Lenwë leads a body of Teleri who refuse to cross the Misty Mountains during the journey west. They become known as the Nandor and settle in the lands east of the mountains.",
        "role": "The published account gives only a brief outline of Lenwë's choice and migration. His people form one of the branches from which the later Green-elves and Silvan Elves descend.",
        "appears": [
            A("Quenta Silmarillion, ch. 3", "He leads the Teleri who turn aside from the Great Journey and become the Nandor."),
        ],
    },
    "denethor": {
        "name": "Denethor", "kindred": "Moriquendi — Nandor", "other": "Denethor of the Green-elves",
        "family": "Son of Lenwë in later genealogical accounts; leader of the Green-elves of Ossiriand.",
        "overview": "Denethor leads a branch of the Nandor westward into Beleriand. His people settle in Ossiriand and become known as the Laiquendi, or Green-elves.",
        "role": "When Morgoth's forces invade Beleriand, Denethor's people fight in the First Battle. Denethor is slain on Amon Ereb, and the Green-elves thereafter avoid open war.",
        "appears": [
            A("Quenta Silmarillion, ch. 3, 10", "He leads the Green-elves into Ossiriand and dies in the First Battle of Beleriand."),
        ],
    },
    "cirdan": {
        "name": "Círdan", "kindred": "Moriquendi — Telerin", "other": "Círdan the Shipwright; Nowë",
        "family": "A Telerin Elf and leader of the Falathrim; kin of Elwë in some traditions.",
        "overview": "Círdan is the Shipwright who leads the Teleri of the Falas. He remains in Middle-earth through the First Age and builds the ships that connect its peoples with the West.",
        "role": "Círdan shelters fugitives, aids the messengers sent to Gondolin, and survives the ruin of Beleriand. In the Second Age he gives Narya to Gandalf and remains one of the keepers of the havens.",
        "appears": [
            A("Quenta Silmarillion, ch. 10, 14–15", "He rules the Falathrim and maintains the Havens of the Falas."),
            A("Quenta Silmarillion, ch. 23–24", "He supports the escapees from Gondolin and the later voyage to Aman."),
            A("The Silmarillion, Of the Rings of Power and the Third Age", "He keeps Narya and oversees the Grey Havens."),
        ],
    },
    "celeborn": {
        "name": "Celeborn", "kindred": "Moriquendi — Sindar", "other": "Celeborn of Doriath",
        "family": "A kinsman of Thingol in the published Silmarillion; husband of Galadriel.",
        "overview": "Celeborn is a Sindarin prince of Doriath who marries Galadriel. Together they leave Beleriand and later become leaders among the Elves of Middle-earth.",
        "role": "The published Silmarillion gives only a short account of Celeborn's First Age history. In later ages he and Galadriel rule in Lórien and help resist Sauron.",
        "appears": [
            A("Quenta Silmarillion, ch. 19", "His marriage to Galadriel and departure from Beleriand are noted."),
            A("The Silmarillion, Of the Rings of Power and the Third Age", "He and Galadriel are leaders among the later Eldar."),
        ],
    },
    "luthien": {
        "name": "Lúthien Tinúviel", "kindred": "Moriquendi — Sindar and Maia descent", "other": "Tinúviel; Nightingale",
        "family": "Daughter of Thingol and Melian; beloved of Beren; mother of Dior.",
        "overview": "Lúthien is the daughter of Thingol and the Maia Melian. Her beauty and songs are renowned, but her defining story is her decision to leave Doriath and share Beren's perilous quest.",
        "role": "Lúthien enters Angband with Beren, overcomes Morgoth's vigilance, and helps recover a Silmaril. After Beren's death she chooses a mortal fate, becoming the foremother of Dior, Elwing, and the Half-elven line.",
        "appears": [
            A("Quenta Silmarillion, ch. 4", "Her birth and life in Doriath are introduced."),
            A("Quenta Silmarillion, ch. 19", "She meets Beren, escapes Doriath, and enters Angband to win a Silmaril."),
            A("Quenta Silmarillion, ch. 22", "Her choice of a mortal life shapes the fate of her descendants."),
        ],
    },
    "daeron": {
        "name": "Daeron", "kindred": "Moriquendi — Sindar", "other": "Minstrel of Doriath",
        "family": "Minstrel and loremaster of Thingol's court; he loves Lúthien.",
        "overview": "Daeron is a gifted musician and lore-keeper in Doriath. He loves Lúthien and becomes jealous of Beren, whose arrival changes her life.",
        "role": "Daeron reveals Lúthien and Beren's meetings to Thingol, setting the events of their quest in motion. After Lúthien leaves Doriath, Daeron also disappears from the story.",
        "appears": [
            A("Quenta Silmarillion, ch. 4, 19", "His music, love of Lúthien, and disclosure of her meetings with Beren are recounted."),
        ],
    },
    "beleg": {
        "name": "Beleg Cúthalion", "kindred": "Moriquendi — Sindar", "other": "Beleg Strongbow",
        "family": "A Marchwarden of Doriath and close friend of Túrin.",
        "overview": "Beleg is one of Thingol's greatest hunters and a captain of the Marchwardens. He becomes a loyal friend and companion to Túrin after Túrin leaves Doriath.",
        "role": "Beleg finds Túrin among the outlaws and follows him into danger. After rescuing him from Orcs, Beleg is accidentally killed by Túrin, an episode that deepens the tragedy of Túrin's life.",
        "appears": [
            A("Quenta Silmarillion, ch. 19", "He is named among the great hunters of Doriath."),
            A("Quenta Silmarillion, ch. 21", "His friendship with Túrin, rescue, and death are told."),
        ],
    },
    "mablung": {
        "name": "Mablung", "kindred": "Moriquendi — Sindar", "other": "Mablung of the Heavy Hand",
        "family": "A captain and trusted servant of Thingol; companion of Beleg.",
        "overview": "Mablung is a Sindarin captain of Doriath, renowned as a hunter and warrior. He serves Thingol and later undertakes dangerous missions beyond the Girdle of Melian.",
        "role": "Mablung is one of the few who understands the peril of the Silmaril in Doriath. He is sent to Nargothrond and later returns to find the kingdom destroyed; he dies in the second Kinslaying.",
        "appears": [
            A("Quenta Silmarillion, ch. 19", "He serves Thingol during the story of Beren and Lúthien."),
            A("Quenta Silmarillion, ch. 21–22", "He journeys to Nargothrond, returns to Doriath, and is slain in its ruin."),
        ],
    },
    "saeros": {
        "name": "Saeros", "kindred": "Moriquendi — Sindar", "other": "Counsellor of Thingol",
        "family": "A counsellor at Thingol's court in Doriath.",
        "overview": "Saeros is a Sindarin Elf of Doriath whose contempt for Túrin leads to a public quarrel. He mocks Túrin and insults his mother and sister.",
        "role": "When Saeros attacks Túrin, Túrin overpowers him and lets him flee. Saeros falls while attempting to escape through the forest, and Túrin leaves Doriath believing himself responsible for his death.",
        "appears": [
            A("Quenta Silmarillion, ch. 21", "His conflict with Túrin results in his death and Túrin's departure from Doriath."),
        ],
    },
    "eol": {
        "name": "Eöl", "kindred": "Moriquendi — Telerin", "other": "The Dark Elf",
        "family": "Husband of Aredhel; father of Maeglin; a kinsman of Thingol.",
        "overview": "Eöl is a skilled smith who dwells in Nan Elmoth and avoids the company of the Noldor. He is associated with the Dwarves and makes the black sword Anglachel.",
        "role": "Eöl marries Aredhel and raises Maeglin in isolation. When they flee to Gondolin, Eöl follows and attempts to kill his son; Aredhel is fatally struck, and Eöl is executed by Turgon's judgment.",
        "appears": [
            A("Quenta Silmarillion, ch. 16", "His marriage to Aredhel, pursuit of his family, and death in Gondolin are told."),
        ],
    },
    "nimloth": {
        "name": "Nimloth", "kindred": "Moriquendi — Sindar", "other": "Nimloth of Doriath",
        "family": "A kinswoman of Celeborn; wife of Dior; mother of Eluréd, Elurín, and Elwing.",
        "overview": "Nimloth is a Sindarin Elf of Doriath who marries Dior, the heir of Thingol. She is the mother of the children who carry the line of Lúthien into later ages.",
        "role": "During the attack on Doriath by the sons of Fëanor, Nimloth is slain along with Dior. Her children are taken into the forest, while Elwing escapes with the Silmaril.",
        "appears": [
            A("Quenta Silmarillion, ch. 22", "Her marriage, children, and death in the ruin of Doriath are recounted."),
        ],
    },
    "dior": {
        "name": "Dior Eluchíl", "kindred": "Moriquendi — Half-elven descent", "other": "Eluchíl; Thingol's Heir",
        "family": "Son of Beren and Lúthien; husband of Nimloth; father of Eluréd, Elurín, and Elwing.",
        "overview": "Dior is the son of Beren and Lúthien and the heir of Thingol. After receiving the Silmaril from his parents, he returns to Doriath and assumes the kingship.",
        "role": "Dior refuses to surrender the Silmaril to the sons of Fëanor. Their assault destroys Doriath, and Dior is killed; the survival of Elwing carries the jewel onward.",
        "appears": [
            A("Quenta Silmarillion, ch. 19, 22", "He is born to Beren and Lúthien, inherits the Silmaril, and dies in the Kinslaying at Doriath."),
        ],
    },
    "elured": {
        "name": "Eluréd", "kindred": "Moriquendi — Sindar and human descent", "other": "Elder son of Dior",
        "family": "Son of Dior and Nimloth; brother of Elurín and Elwing.",
        "overview": "Eluréd is one of the young sons of Dior and Nimloth. During the attack on Doriath, the servants of Celegorm carry him and his brother into the forest.",
        "role": "The brothers are abandoned and never found. Their fate remains unknown, one of the unresolved losses in the story of Doriath's destruction.",
        "appears": [
            A("Quenta Silmarillion, ch. 22", "He and Elurín are captured during the attack on Doriath and disappear in the forest."),
        ],
    },
    "elurin": {
        "name": "Elurín", "kindred": "Moriquendi — Sindar and human descent", "other": "Younger son of Dior",
        "family": "Son of Dior and Nimloth; brother of Eluréd and Elwing.",
        "overview": "Elurín is the younger son of Dior and Nimloth. He is taken into the forest with Eluréd during the sons of Fëanor's assault on Doriath.",
        "role": "Neither brother is seen again, and the text does not resolve whether they survived. Their disappearance emphasizes the human cost of the Kinslaying at Doriath.",
        "appears": [
            A("Quenta Silmarillion, ch. 22", "He and Eluréd are abandoned in the forest after the fall of Doriath."),
        ],
    },
    "elwing": {
        "name": "Elwing", "kindred": "Moriquendi — Half-elven descent", "other": "Elwing the White",
        "family": "Daughter of Dior and Nimloth; wife of Eärendil; mother of Elrond and Elros.",
        "overview": "Elwing is the surviving daughter of Dior and Nimloth. She escapes the fall of Doriath with the Silmaril and later lives among the refugees at the Mouths of Sirion.",
        "role": "When the sons of Fëanor attack the Havens, Elwing leaps into the sea with the Silmaril and is rescued by Ulmo. She joins Eärendil on his voyage to Aman, where their plea leads to the War of Wrath.",
        "appears": [
            A("Quenta Silmarillion, ch. 22", "She escapes the ruin of Doriath with the Silmaril."),
            A("Quenta Silmarillion, ch. 24", "She survives the attack on the Havens and sails with Eärendil to Aman."),
        ],
    },
    "earendil": {
        "name": "Eärendil", "kindred": "Half-elven", "other": "Eärendil the Mariner; the Blessed",
        "family": "Son of Tuor and Idril; husband of Elwing; father of Elrond and Elros.",
        "overview": "Eärendil is a mariner born in Gondolin and raised among the survivors of its fall. He later lives at the Mouths of Sirion with Elwing and becomes the one who sails west carrying a Silmaril.",
        "role": "Eärendil reaches Aman and asks the Valar to aid Elves and Men against Morgoth. His voyage brings the War of Wrath; afterward he is set in the heavens with the Silmaril as the Evening Star.",
        "appears": [
            A("Quenta Silmarillion, ch. 23–24", "His birth, voyages, plea to the Valar, and elevation as the Evening Star form the conclusion of the Quenta."),
        ],
    },
    "elrond": {
        "name": "Elrond", "kindred": "Half-elven", "other": "Elrond Peredhel; Master of Rivendell",
        "family": "Son of Eärendil and Elwing; twin brother of Elros.",
        "overview": "Elrond is one of the Half-elven sons of Eärendil and Elwing. At the end of the First Age he is given the choice of kindreds and chooses the fate of the Elves.",
        "role": "Elrond becomes a leader and healer in Middle-earth, founding Rivendell and preserving lore through later ages. He is a keeper of Vilya and an opponent of Sauron.",
        "appears": [
            A("Quenta Silmarillion, ch. 24", "He is born to Eärendil and Elwing and receives the choice of kindreds."),
            A("The Silmarillion, Akallabêth", "His later presence is part of the history of the Eldar and Númenor."),
            A("The Silmarillion, Of the Rings of Power and the Third Age", "He becomes a leader of the Eldar and keeper of Vilya."),
        ],
    },
    "elros": {
        "name": "Elros", "kindred": "Half-elven; chooses the fate of Men", "other": "Elros Tar-Minyatur",
        "family": "Son of Eärendil and Elwing; twin brother of Elrond.",
        "overview": "Elros is one of the Half-elven sons of Eärendil and Elwing. When offered the choice of kindreds, he chooses the fate of Men and becomes the first king of Númenor.",
        "role": "Elros's long life and royal line shape the history of Númenor. His choice contrasts with Elrond's and establishes the distinct destinies of their descendants.",
        "appears": [
            A("Quenta Silmarillion, ch. 24", "He is born to Eärendil and Elwing and chooses the fate of Men."),
            A("The Silmarillion, Akallabêth", "He becomes Tar-Minyatur, the first king of Númenor."),
        ],
    },
    "falathar": {
        "name": "Falathar", "kindred": "Moriquendi — Falathrim", "other": "Mariner of the Havens",
        "family": "One of the mariners who sail with Eärendil.",
        "overview": "Falathar is named as one of Eärendil's companions on his westward voyage. The published account records his place in the crew but gives no further personal history.",
        "role": "His inclusion identifies the small company that accompanies Eärendil on the mission that brings the Valar's intervention in the War of Wrath.",
        "appears": [
            A("Quenta Silmarillion, ch. 24", "He sails with Eärendil and the other mariners toward Aman."),
        ],
    },
    "erellont": {
        "name": "Erellont", "kindred": "Moriquendi — Falathrim", "other": "Mariner of the Havens",
        "family": "One of the mariners who sail with Eärendil.",
        "overview": "Erellont is one of the named mariners who accompany Eärendil. His history is limited to the voyage west in the final chapter of the Quenta Silmarillion.",
        "role": "The mariners' journey makes Erellont a witness to the last great passage from Middle-earth to Aman in the First Age.",
        "appears": [
            A("Quenta Silmarillion, ch. 24", "He is named among Eärendil's companions on the voyage to Aman."),
        ],
    },
    "aerandir": {
        "name": "Aerandir", "kindred": "Moriquendi — Falathrim", "other": "Mariner of the Havens",
        "family": "One of the mariners who sail with Eärendil.",
        "overview": "Aerandir is named among the mariners who accompany Eärendil on his final voyage. The published Silmarillion offers no additional account of his life.",
        "role": "His brief appearance belongs to the transition from the wars of Beleriand to the intervention of the Valar and the end of the First Age.",
        "appears": [
            A("Quenta Silmarillion, ch. 24", "He sails west with Eärendil and the other mariners."),
        ],
    },
}

for relative in ("home.md",):
    p = ROOT / relative
    p.write_text(
        "# Elves of the Silmarillion\n\n"
        "An inventory of named Elves and Half-elven figures in the published *Silmarillion*. "
        "The site listing below supplies the navigation; individual character entries are organized by kindred.\n",
        encoding="utf-8",
    )

home_intros = {
    "avari": (
        "# Avari\n\n"
        "The Avari are the Elves who refused the summons to journey west. "
        "No named Avari are individually identified in the published *Silmarillion*."
    ),
    "moriquendi": (
        "# Moriquendi\n\n"
        "This inventory groups named Elves who did not see the light of the Two Trees, including Sindar, Nandor, and Falathrim. "
        "The term overlaps with ancestral kindreds such as the Teleri."
    ),
    "teleri": (
        "# Teleri\n\n"
        "Named Teleri in this inventory include those who reached Aman and their descendants. "
        "Telerin peoples who remained in Middle-earth are listed with the Moriquendi."
    ),
    "vanyar": (
        "# Vanyar\n\n"
        "The Vanyar are the first kindred of the Eldar to reach Aman and remain there through the First Age."
    ),
    "noldor": (
        "# Noldor\n\n"
        "The Noldor are the second kindred of the Eldar to reach Aman. "
        "Their First Age entries are arranged by the royal houses of Finwë, Fëanor, Fingolfin, and Finarfin."
    ),
    "noldor/house-of-finwe": (
        "# House of Finwë\n\n"
        "Finwë's household begins the royal line of the Noldor in Aman."
    ),
    "noldor/house-of-feanor": (
        "# House of Fëanor\n\n"
        "Fëanor's house includes his wife and seven sons, whose oath drives many of the First Age conflicts."
    ),
    "noldor/house-of-fingolfin": (
        "# House of Fingolfin\n\n"
        "The descendants of Fingolfin include the kings Fingon and Turgon and the line of Idril."
    ),
    "noldor/house-of-finarfin": (
        "# House of Finarfin\n\n"
        "Finarfin's house includes Finrod Felagund, Galadriel, and the princes of Nargothrond and Dorthonion."
    ),
}

for relative, content in home_intros.items():
    (ROOT / relative / "home.md").write_text(content + "\n", encoding="utf-8")

for slug, entry in ENTRIES.items():
    path = next(ROOT.rglob(f"{slug}.md"), None)
    if path is None:
        raise FileNotFoundError(f"Missing inventory page for {slug}")

    lines = [
        f"# {entry['name']}",
        "",
        f"*{entry['kindred']}*",
        "",
        "## Overview",
        "",
        entry["overview"],
        "",
        "## Names and Relationships",
        "",
        f"- **Other names or titles:** {entry['other']}",
        f"- **Family:** {entry['family']}",
        "",
        "## Role in the Legendarium",
        "",
        entry["role"],
        "",
        "## Appears In",
        "",
        "<!--has-ai-elements-->",
        "| Volume / story | Appearance or significance |",
    ]
    for volume, chapter, context in entry["appears"]:
        lines.append(f"| {volume} | {chapter} — {context} |")
    lines += [
        "",
        "## Further Reading",
        "",
        f"- [Tolkien Gateway: {entry['name']}](https://tolkiengateway.net/wiki/{entry['name'].replace(' ', '_')})",
        f"- [Encyclopedia of Arda: {entry['name']}](https://encyclopediaofarda.com/wiki/{entry['name'].replace(' ', '_')})",
        "- *The Silmarillion*, especially the cited sections above.",
        "",
        "## Editorial Note",
        "",
        "This page summarizes the character's role in the published *Silmarillion*. "
        "Where later writings differ or add detail, that distinction is noted rather than silently merged.",
        "",
    ]
    path.write_text("\n".join(lines), encoding="utf-8")

print(f"Wrote {len(ENTRIES)} character pages and removed manual tables from 10 home pages.")
```
