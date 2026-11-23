---

## fix-valar-encoding--ps1

**Introduction:** PowerShell version of the Valar encoding repair step. It records how the earlier mojibake cleanup was applied to files in the Valar content folder.

**How to use:** Use only on a backup after confirming that the target files contain the encodings this repair expects. Set `TOLKIEN_VALAR_DIR`, extract the fenced code to a local `.ps1` file outside the published tree, run in PowerShell, and check the resulting text as UTF-8.

```ps
$dir = $env:TOLKIEN_VALAR_DIR
if (-not $dir) { throw 'Set TOLKIEN_VALAR_DIR before running this script.' }
$files=Get-ChildItem -LiteralPath $dir -Filter *.md
$pairs=@()
$pairs += ,@((([char]0xC3).ToString()+([char]0xAB).ToString()),([char]0xEB).ToString())
$pairs += ,@((([char]0xC3).ToString()+([char]0xBC).ToString()),([char]0xFC).ToString())
$pairs += ,@((([char]0xC3).ToString()+([char]0xBA).ToString()),([char]0xFA).ToString())
$pairs += ,@((([char]0xC3).ToString()+([char]0xA1).ToString()),([char]0xE1).ToString())
$pairs += ,@((([char]0xC3).ToString()+([char]0xA9).ToString()),([char]0xE9).ToString())
$pairs += ,@((([char]0xC3).ToString()+([char]0xA4).ToString()),([char]0xE4).ToString())
$pairs += ,@((([char]0xC3).ToString()+([char]0xAF).ToString()),([char]0xEF).ToString())
$pairs += ,@((([char]0xC3).ToString()+([char]0xB1).ToString()),([char]0xF1).ToString())
$pairs += ,@((([char]0xC3).ToString()+([char]0xAD).ToString()),([char]0xED).ToString())
$pairs += ,@((([char]0xC3).ToString()+([char]0xB3).ToString()),([char]0xF3).ToString())
$pairs += ,@((([char]0xC3).ToString()+([char]0xA2).ToString()+([char]0xE2).ToString()+([char]0x80).ToString()+([char]0x94).ToString()),([char]0x2014).ToString())
$pairs += ,@((([char]0xC3).ToString()+([char]0xA2).ToString()+([char]0xE2).ToString()+([char]0x80).ToString()+([char]0x93).ToString()),([char]0x2013).ToString())
$pairs += ,@((([char]0xC3).ToString()+([char]0xA2).ToString()+([char]0xE2).ToString()+([char]0x80).ToString()+([char]0x99).ToString()),([char]0x2019).ToString())
foreach($file in $files){
    if($file.Name -eq 'home.md'){continue}
    $text=[IO.File]::ReadAllText($file.FullName,[Text.Encoding]::UTF8)
    foreach($pair in $pairs){
        $old=$pair[0]
        $new=$pair[1]
        $text=$text.Replace([string]$old,[string]$new)
    }
    [IO.File]::WriteAllText($file.FullName,$text,[Text.UTF8Encoding]::new($false))
}
```
