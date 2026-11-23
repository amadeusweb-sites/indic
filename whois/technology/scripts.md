
[spacer]Nov 2026[/spacer]

rename all the script files with hyphens instead of underscores, --extension.md

and add a ```py or ```ps at the top and bottom of each file along with a regular markdown ## heading and a detailed introduction.

I am not comfortable checking in or deploying actual ps or py files

---

## What was done

The Tolkien script archive was converted from standalone Python and PowerShell files into Markdown documentation. All 14 source listings now use hyphenated filenames with an extension marker, such as `build-elf-indexes--py.md` and `generate-valar-utf8--ps1.md`.

Each listing has a `##` heading, a detailed introduction and “How to use” guidance, and the original source enclosed in a `py` or `ps` fenced code block. The `.py` and `.ps1` files were removed from the archive so it contains documentation rather than deployable scripts. The [script archive home](./2026-11/tolkien/scripts/home.md) explains the naming convention, safe extraction, environment variables, historical limitations, and the rule not to deploy extracted scripts.

Validation confirmed that all 14 listings are Markdown, their links and code fences resolve, Python and PowerShell code blocks parse, and no standalone `.py` or `.ps1` files remain. The validator was also run directly from its Markdown code block; it reported 66 Elf profiles and 10 table-free indexes passing the documented formatting checks.

The older Valar PowerShell listing remains as a historical record with its mojibake documented; it is not recommended for execution. The archived generators can overwrite content, so their usage notes require backups and review before any extraction or run.

