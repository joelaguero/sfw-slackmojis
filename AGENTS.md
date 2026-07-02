# Repository Agent Instructions

This repo publishes a curated SFW Slack custom emoji pack for bulk download.

## Repo Shape

- `emojis/` contains the flattened curated emoji image files.
- `dist/sfw-slackmojis.zip` must contain exactly the same filenames as `emojis/`.
- Slack derives emoji names from filenames without extensions. Every basename in `emojis/` must be globally unique, case-insensitively, even when extensions differ.
- `README.md` must state the current emoji count and keep Slackmojis.com credit plus the third-party rights disclaimer.
- `LICENSE` applies to repo-owned curation materials, documentation, and scripts only. Do not imply the bundled emoji images are owned by this repo.

## Import Workflow

When adding a new Slackmoji archive:

1. Use only images under `emojis.slackmojis.com/...` from the downloaded zip.
2. Ignore site HTML, CSS, JS, search icons, `_DataURI`, and original downloaded zip files.
3. Deduplicate by file content using SHA-256 against `emojis/`.
4. If a new unique image has the same filename as an existing image, keep both by suffixing the new file with `-2`, `-3`, etc. before the extension.
5. After import, run a duplicate-basename pass. If files would produce the same Slack emoji name, rename every file in that conflict group to `name-1.ext`, `name-2.ext`, etc., choosing suffixes that leave all final basenames unique across the whole folder.
6. Review all new additions before committing. Remove unsafe files from `emojis/` and do not include them in `dist/`.
7. Rebuild the archive after every accepted import:

```sh
rm -f dist/sfw-slackmojis.zip
(cd emojis && zip -qr ../dist/sfw-slackmojis.zip .)
```

8. Update the README image count.

## SFW Filter

Remove files that contain or clearly reference:

- explicit sexual content, nudity, sexualized body-focus, or sexual gestures
- profane or obscene text, including acronyms like `wtf`
- middle-finger gestures
- drug-use references

Use filename scans as a first pass, then visually inspect new additions. Keep ordinary reactions, character art, logos, alcohol icons, pride/identity imagery, and cartoon violence unless they match the unsafe categories above.

## What Not To Commit

Do not commit:

- original Slackmoji zip files
- local quarantine folders
- review/contact sheets
- temporary import manifests
- extraction staging folders
- local tooling virtualenvs or cache files

## Verification

Before committing, run checks equivalent to:

```sh
python3 - <<'PY'
from pathlib import Path
exts = {'.png', '.jpg', '.jpeg', '.gif', '.webp', '.svg'}
files = [p for p in Path('emojis').iterdir() if p.is_file()]
print('total_files', len(files))
print('image_files', sum(1 for p in files if p.suffix.lower() in exts))
print('non_images', [p.name for p in files if p.suffix.lower() not in exts])
PY

find emojis -maxdepth 1 -type f -exec basename {} \; | sort > /tmp/sfw-emojis-files.txt
zipinfo -1 dist/sfw-slackmojis.zip | sort > /tmp/sfw-zip-files.txt
diff -u /tmp/sfw-emojis-files.txt /tmp/sfw-zip-files.txt

python3 - <<'PY'
from pathlib import Path
from collections import defaultdict
groups = defaultdict(list)
for p in Path('emojis').iterdir():
    if p.is_file():
        groups[p.stem.lower()].append(p.name)
dups = {stem: names for stem, names in groups.items() if len(names) > 1}
print('duplicate_basename_groups', len(dups))
if dups:
    raise SystemExit(dups)
PY
```

Also run a final filename audit for known unsafe terms and confirm any remaining hits are false positives before staging.
