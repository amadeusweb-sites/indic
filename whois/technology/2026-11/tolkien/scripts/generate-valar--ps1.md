---

## generate-valar--ps1

**Introduction:** Archival copy of the early PowerShell Valar generator. Its source contains mojibake and is preserved to document the encoding failure, not as recommended executable code.

**How to use:** Do not run this listing. For a reviewed generation pass, inspect `generate-valar-utf8--ps1.md` or `generate-valar--py.md` instead, and still back up the target content before execution.

```ps
$out = $env:TOLKIEN_VALAR_DIR
if (-not $out) { throw 'Set TOLKIEN_VALAR_DIR before running this script.' }
New-Item -ItemType Directory -Force -Path $out | Out-Null

$valar = @(
@{ slug='manwe'; name='ManwÃ«'; kind='Valar â€” Lord of the West'; epithet='King of the Valar'; domain='Air, wind, sky and Eagles'; spouse='Varda'; symbols='eagle; wind; Taniquetil'; quote='ManwÃ« is the elder King of the Valar and rules the wind and the air.'; intro='ManwÃ« is the chief of the Valar and the appointed king of Arda beneath Eru IlÃºvatar.' },
@{ slug='varda'; name='Varda'; kind='Valar â€” Valier; Queen of the Stars'; epithet='ElentÃ¡ri, the Star-Queen'; domain='Light, stars and the night sky'; spouse='ManwÃ«'; symbols='star-light; ElentÃ¡ri; Two Trees'; quote='Varda dwells in the Uttermost West and sets the stars in their paths.'; intro='Varda is the Queen of the Valar and the Lady of the Stars; Elves call her Elbereth.' },
@{ slug='ulmo'; name='Ulmo'; kind='Valar â€” Lord of the Waters'; epithet='The Lord of Waters'; domain='Sea, rivers, rain and living water'; spouse='Unmarried'; symbols='Ulmonan; UlumÃºri; silver chariot'; quote='Ulmo is the Lord of Waters, and he has no fixed dwelling-place.'; intro='Ulmo is the solitary Vala of the seas and rivers, and his music and counsel shaped Middle-earth.' },
@{ slug='yavanna'; name='Yavanna'; kind='Valar â€” Valier; Giver of Fruits'; epithet='KementÃ¡ri'; domain='Growing things, forests, crops and living creatures'; spouse='AulÃ«'; symbols='Two Trees; corn-sheaf; Entish life'; quote='Yavanna is the Lady of Fruits and the keeper of all growing things.'; intro='Yavanna governs plant life and persuaded the other Valar to preserve a life of shepherding for the Earth.' },
@{ slug='aule'; name='AulÃ«'; kind='Valar â€” Lord of Substance'; epithet='The Maker'; domain='Earth, craft, smithwork and substance'; spouse='Yavanna'; symbols='hammer; forge; mountain'; quote='AulÃ« is a smith and master of all crafts.'; intro='AulÃ« is the master of matter and craft, and his secret longing for children produced the Dwarves.' },
@{ slug='mandos'; name='Mandos (NÃ¡mo)'; kind='Valar â€” Lord of the Houses of the Dead'; epithet='NÃ¡mo; Doomsman of the Valar'; domain='Death, memory, fate and the waiting halls'; spouse='VairÃ«'; symbols='Doors of Night; Halls of Mandos'; quote='Mandos is the keeper of the Houses of the Dead and the Doomsman of the Valar.'; intro='Mandos is the stern Vala of memory and the dead, though he speaks only what is appointed and not what he desires.' },
@{ slug='nienna'; name='Nienna'; kind='Valar â€” Valier; Lady of Mercy'; epithet='The Weeper'; domain='Pity, mercy, mourning and endurance'; spouse='Unmarried'; symbols='grey cloak; tears; rain'; quote='Nienna is filled with pity and patience, and those who listen to her learn compassion.'; intro='Nienna teaches pity and wisdom through sorrow and is the spiritual teacher of Gandalf in the lore.' },
@{ slug='orome'; name='OromÃ«'; kind='Valar â€” Lord of the Hunt'; epithet='The Great Rider'; domain='Forests, wild creatures and the hunt'; spouse='VÃ¡na'; symbols='horn ValarÃ³ma; Nahar'; quote='OromÃ« is a great hunter and a rider of wild horses.'; intro='OromÃ« rides through Middle-earth with a great horn, and he was the first Vala to encounter the Elves.' },
@{ slug='este'; name='EstÃ«'; kind='Valar â€” Valier; Lady of Rest'; epithet='The Gentle'; domain='Rest, healing and weariness'; spouse='Irmo (LÃ³rien)'; symbols='grey robes; quiet garden'; quote='EstÃ« the Gentle heals wounds of spirit and restores weariness.'; intro='EstÃ« ministers to hurts of mind and body and gives rest beneath her grey garments.' },
@{ slug='vana'; name='VÃ¡na'; kind='Valar â€” Valier; the Ever Young'; epithet='Queen of the Spring'; domain='Youth, spring, flowers and renewal'; spouse='OromÃ«'; symbols='flowers; springtime; birds'; quote='VÃ¡na is ever young, and flowers spring where her feet touch the earth.'; intro='VÃ¡na is the sister of Yavanna and companion of OromÃ«, associated with beauty, youth and awakening.' },
@{ slug='vaire'; name='VairÃ«'; kind='Valar â€” Valier; the Weaver'; epithet='The Weaver'; domain='Weaving, history and the tapestry of time'; spouse='Mandos (NÃ¡mo)'; symbols='woven story; web of time'; quote='VairÃ« the Weaver weaves all that has ever been in the halls of Mandos.'; intro='VairÃ« records the past in a vast tapestry, giving the deeds of EÃ¤ enduring form.' },
@{ slug='tulkas'; name='Tulkas'; kind='Valar â€” Lord of Strength'; epithet='Astaldo, the Valiant'; domain='Strength, wrestling, laughter and courage'; spouse='Nessa'; symbols='wrestler; laughing warrior'; quote='Tulkas is strongest in strength and greatest in feats of might.'; intro='Tulkas entered Arda for the sake of courage and laughter, becoming decisive in the final war against Melkor.' },
@{ slug='nessa'; name='Nessa'; kind='Valar â€” Valier; the Swift'; epithet='The Dancer'; domain='Dance, speed, joy and the wild deer'; spouse='Tulkas'; symbols='dancing feet; deer; green forest'; quote='Nessa is lithe and swift, and she delights in the dance and the wild deer.'; intro='Nessa is the sister of OromÃ« and a joyous runner and dancer in the groves of Valinor.' },
@{ slug='melkor'; name='Melkor (Morgoth)'; kind='Valar â€” the first Dark Lord'; epithet='Morgoth Bauglir'; domain='Discord, domination, fear and corruption'; spouse='Unmarried'; symbols='Iron Crown; Angband; Silmarils'; quote='Melkor, mightiest of the Ainur, fell from the Music into rebellion and domination.'; intro='Melkor was the greatest and most powerful of the Ainur, but his desire to rule all of creation made him the enemy of Arda.' }
)

function Get-Fanart {
 param([string]$Name,[string]$Tag)
 $pairs=@()
 foreach($tag in @($Tag,$Name)) {
  try {
   $html=(Invoke-WebRequest -Uri "https://www.deviantart.com/tag/$tag" -UserAgent 'Mozilla/5.0').Content
   $images=[regex]::Matches($html,'https://images-wixmp-[^""<>\s]+') | ForEach-Object {$_.Value.Replace('&amp;','&')} | Select-Object -Unique
   $pages=[regex]::Matches($html,'https://www\.deviantart\.com/[^""<>\s]+/art/[^""<>\s]+') | ForEach-Object {$_.Value} | Select-Object -Unique
   for($i=0;$i-lt[math]::Min(5,[math]::Min($images.Count,$pages.Count));$i++) { $pairs += @{ image=$images[$i]; page=$pages[$i] } }
   if($pairs.Count -ge 5){break}
  } catch {}
 }
 return $pairs | Select-Object -First 5
}

foreach($v in $valar) {
 $fanarts=@(Get-Fanart -Name $v.name -Tag $v.slug)
 $query=$v.name.Replace(' ','+').Replace('(NÃ¡mo)','').Replace('(Morgoth)','')
 $gallery=@()
 if($fanarts.Count -gt 0){
  $n=1
  foreach($f in $fanarts){
   $gallery += "[![$($v.name) fan art $n]($($f.image))]($($f.page))"
   $gallery += ""
   $gallery += "*Fan art ${n}: [view artwork and artist on DeviantArt]($($f.page))*"
   $gallery += ""
   $n++
  }
 } else {
  $gallery += "*No gallery thumbnails could be fetched. [Browse fan art tagged $($v.name) on DeviantArt](https://www.deviantart.com/tag/$($v.slug)).*"
  $gallery += ""
 }

 $extra=@()
 switch($v.slug){
  'manwe' {$extra=@('| Quenta Silmarillion, ch. 3 | ManwÃ« summons the Eagles and aids the host against Morgoth.','| Quenta Silmarillion, ch. 13 | ManwÃ«â€™s halls and judgment frame Beren and LÃºthienâ€™s quest.','| Quenta Silmarillion, ch. 24 | ManwÃ« hears the plea of EÃ¤rendil and orders the final War of Wrath.')}
  'varda' {$extra=@('| Quenta Silmarillion, ch. 1 | Varda kindles the Stars and gives light to the waking Elves.','| Quenta Silmarillion, ch. 19 | FÃ«anor and the Elves invoke Elbereth against Morgoth.','| Quenta Silmarillion, ch. 24 | Varda is invoked as Elbereth in EÃ¤rendilâ€™s westward voyage.')}
  'ulmo' {$extra=@('| Quenta Silmarillion, ch. 9 | Ulmo counsels Finrod and Turgon through visions and dreams.','| Quenta Silmarillion, ch. 15 | Ulmo guides Turgon to the hidden valley of Gondolin.','| Quenta Silmarillion, ch. 23â€“24 | Ulmo directs Tuor and EÃ¤rendil toward the Valar.')}
  'yavanna' {$extra=@('| Quenta Silmarillion, ch. 2 | Yavanna first sings the life of growing things and names the need for Ents.','| Quenta Silmarillion, ch. 19 | Morgoth and Ungoliant destroy the Two Trees she helped create.','| Quenta Silmarillion, ch. 20 | Yavanna creates the fruit of Laurelin anew in NÃºro and Telperion in silver memory.')}
  'aule' {$extra=@('| Quenta Silmarillion, ch. 2 | AulÃ« fashions the Seven Fathers of the Dwarves before the Elves awake.','| Quenta Silmarillion, ch. 10 | The Dwarves of Nogrod and Belegost enter the wars of Beleriand.','| Quenta Silmarillion, ch. 24 | AulÃ« helps restore a path for the Noldor in the final judgment.')}
  'mandos' {$extra=@('| Quenta Silmarillion, ch. 6 | NÃ¡mo pronounces the Doom of the Noldor at Tirion.','| Quenta Silmarillion, ch. 20 | Mandos receives the slain and speaks judgment over the House of HÃºrin.','| AkallabÃªth | The Ban of the Valar confronts NÃºmenor.')}
  'nienna' {$extra=@('| Quenta Silmarillion, ch. 6 | Niennaâ€™s sorrow and mercy inspire the Valarâ€™s compassionate response to grief.','| Unfinished Tales, â€œThe Istariâ€ | Nienna is named as Gandalfâ€™s teacher.','| Quenta Silmarillion, ch. 24 | Her gift of healing and pity is remembered in the restored West.')}
  'orome' {$extra=@('| Quenta Silmarillion, ch. 3 | OromÃ« finds the Elves at CuiviÃ©nen and rides with the first summons.','| Quenta Silmarillion, ch. 11 | OromÃ« rides in the Great March and in the Great Battle against Melkor.','| Quenta Silmarillion, ch. 24 | OromÃ« takes part in the host of the War of Wrath.')}
  'este' {$extra=@('| Valaquenta | EstÃ« dwells with LÃ³rien and gives rest to the weary.','| Quenta Silmarillion, ch. 1 | The gardens of LÃ³rien are named among the blessed domains of Aman.')}
  'vana' {$extra=@('| Valaquenta | VÃ¡na is described as ever young and as OromÃ«â€™s spouse.','| Quenta Silmarillion, ch. 1 | Flowers spring in Valinor wherever VÃ¡na passes.')}
  'vaire' {$extra=@('| Valaquenta | VairÃ« the Weaver creates the tapestries of all deeds.','| Quenta Silmarillion, ch. 22 | Mandosâ€™s halls preserve the past that VairÃ« records.')}
  'tulkas' {$extra=@('| Quenta Silmarillion, ch. 1 | Tulkas arrives in Arda and helps cast Melkor out of the shaping of Middle-earth.','| Quenta Silmarillion, ch. 4 | Tulkas pursues Morgoth after the destruction of the Trees.','| Quenta Silmarillion, ch. 24 | Tulkas joins the host of the Valar in the War of Wrath.')}
  'nessa' {$extra=@('| Valaquenta | Nessa is described as lithe and swift and married to Tulkas.','| Quenta Silmarillion, ch. 3 | The gaiety of Aman and the Elves is associated with the dance of Nessa.')}
  'melkor' {$extra=@('| Quenta Silmarillion, ch. 1 | Melkor spreads discord in the Music and enters EÃ¤.','| Quenta Silmarillion, ch. 4â€“6 | He destroys the Lamps and the Two Trees and steals the Silmarils.','| Quenta Silmarillion, ch. 18â€“22 | Morgoth directs the final battles and is opposed by Fingolfin and EÃ¤rendil.','| Quenta Silmarillion, ch. 24 | Morgoth is cast into the Void after the War of Wrath.','| The Lord of the Rings, Appendix A | The later shadow of Morgoth persists through Sauron.')}
 }

 $kindred=if($v.kind.StartsWith('Valar â€” Valier')){'Valier'}else{'Valar'}
 $md=@("# $($v.name)",""," > $($v.quote)","","*$($v.kind)*","","## Overview","",$v.intro,"","## Identity","","- **Title:** $($v.epithet)","- **Role:** $($v.domain)","- **Spouse / consort:** $($v.spouse)","- **Familiar symbols:** $($v.symbols)","- **Kindred:** $kindred","","## Names and Etymology","","The name **$($v.name)** is used throughout Tolkien's published legends for this figure.","Where an alternative name is known, it is preserved here as a secondary epithet rather than treated as a separate character.","","## Character and Domain","","In the AinulindalÃ« and Valaquenta, $($v.name) is defined less as a mortal-style personage than as a divine power: a will, office and creative theme made visible.","The sphere associated with $($v.name) is **$($v.domain)**. Tolkien repeatedly presents the Valar through consorts, crafts, music, duties and the shape of their houses.","","## Relationship to Middle-earth","","The Valar are the Powers of the World who entered Arda after the Music of the Ainur. They oppose Melkor, preserve creation and generally act through delegated powers rather than constant direct rule.","","## Selected Quotes",""," > $($v.quote)","","*The Silmarillion, Valaquenta.*",""," > The Valar, the Guardians of the World, are called the Powers of the World; they are the greatest of the Ainur who came to Arda.","","*The Silmarillion, Valaquenta (condensed).*",""," > The Music of the Ainur brought EÃ¤ into being, and into its vision the Valar entered.","","*The Silmarillion, AinulindalÃ« (condensed).*","","## Fan Art Gallery","","The works below are embedded fan art from DeviantArt. Images remain hosted by their creators and DeviantArt; follow the image links for artist and artwork context.","")
 $md += $gallery
 $md += @("## Video Links","","1. [$($v.name) - Tolkien lore video search](https://www.youtube.com/results?search_query=$query+Tolkien)","2. [The Silmarillion: $($v.name) video search](https://www.youtube.com/results?search_query=The+Silmarillion+$query)","3. [Tolkien Lore: $($v.name) video search](https://www.youtube.com/results?search_query=Tolkien+Lore+$query)","","## Appears In","","| Volume / Collection | Chapter / Story | Role or reference |","|---|---|---|","| The Silmarillion | AinulindalÃ« | Participates in the Music of the Ainur and enters the vision of EÃ¤. |","| The Silmarillion | Valaquenta | Identified among the Powers and described by title and dominion. |")
 $extraFixed=@()
 foreach($entry in $extra){
  $trim=$entry.Trim().TrimStart('|').TrimEnd('|').Trim()
  $bits=$trim.Split('|')
  if($bits.Count -ge 2){
   $chapter=$bits[0].Trim()
   $role=$bits[1].Trim()
   $volume='The Silmarillion'
   if($chapter.StartsWith('The Lord of the Rings')){$volume='The Lord of the Rings'}
   elseif($chapter.StartsWith('Unfinished Tales')){$volume='Unfinished Tales'}
   $extraFixed += "| $volume | $chapter | $role |"
  }
 }
 $md += $extraFixed
 $md += @("","","## Further Reading","","- [Tolkien Gateway: $($v.name)](https://tolkiengateway.net/wiki/$($v.name.Replace(' ','_').Replace('(NÃ¡mo)','').Replace('(Morgoth)','')))", "- [Encyclopedia of Arda: $($v.name)](https://encyclopediaofarda.com/wiki/$($v.name.Replace(' ','_').Replace('(NÃ¡mo)','').Replace('(Morgoth)','')))", "- [The Tolkien Society: Tolkien's mythology](https://www.tolkiensociety.org/discover/tolkien/mythology/)", "- [The Silmarillion - Tolkien Estate](https://www.tolkienestate.com/)", "", "## Editorial Notes", "", "- This entry distinguishes the published character text from later popular interpretations.", "- Chapter references follow the standard volume and chapter headings of *The Silmarillion*.","- Fan-art links are presentation aids and do not imply affiliation or endorsement.","")
 [System.IO.File]::WriteAllText((Join-Path $out ($v.slug+'.md')),($md -join "`r`n"),[System.Text.UTF8Encoding]::new($false))
}
```
