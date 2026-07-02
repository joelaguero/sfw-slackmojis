# SFW Slackmojis

A curated bulk-downloadable pack of SFW Slack custom emoji.

This repo exists to make it easier to grab a large set of Slackmojis without manually downloading category-by-category.

## Credit

The source emoji archives were downloaded from [Slackmojis.com](https://slackmojis.com), a community resource for Slack custom emoji. This repo is an unofficial curated bulk-download mirror and is not affiliated with Slackmojis.com or Slack.

## What's Included

- `emojis/` contains the curated SFW emoji image files.
- `dist/sfw-slackmojis.zip` contains the same emoji files as a single bulk-download archive.
- The current pack includes 1,986 emoji images.

## Download

For the easiest bulk download, use:

```sh
curl -L -o sfw-slackmojis.zip https://github.com/joelaguero/sfw-slackmojis/raw/main/dist/sfw-slackmojis.zip
```

Or clone the repo:

```sh
git clone https://github.com/joelaguero/sfw-slackmojis.git
```

## Curation Notes

This pack was assembled from downloaded Slackmoji archives, deduplicated by file content, and filtered to remove NSFW, explicit, profane, and drug-reference emoji files.

The filter is best-effort. If you find something that should not be included, open an issue or PR with the filename.

## Rights and Licensing

The repository's CC0 license applies to repo-owned curation materials, documentation, and any scripts added here.

The emoji images themselves may include third-party artwork, trademarks, characters, logos, memes, or other content owned by their respective rights holders. No ownership claim is made over those images.
