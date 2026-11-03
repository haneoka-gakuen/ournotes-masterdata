# Our Notes Masterdata

Complete decoded Master tables for BanG Dream! Our Notes (JP and Intl).

[![Dataset publication](https://github.com/haneoka-gakuen/ournotes-masterdata/actions/workflows/release.yml/badge.svg)](https://github.com/haneoka-gakuen/ournotes-masterdata/actions/workflows/release.yml)

## Data layout

`jp/MasterCharacter.json` and `intl/MasterCharacter.json` retain the original `_allData` object structure and game field names. Every official table is included, with translations in the fields supplied by the game.

The root `manifest.json` is the single version record for both servers. It records the Master generation, its content hash, the dataset SHA-256 and file count. Operational logs and retry state stay in the producer's private control namespace, outside this repository.

## Use

```sh
git clone https://github.com/haneoka-gakuen/ournotes-masterdata.git
cd ournotes-masterdata
```

```sh
python - <<'PYTHON'
import json
with open("jp/MasterCharacter.json", encoding="utf-8") as source:
    characters = json.load(source)["_allData"]
print(characters[0])
PYTHON
```

For one server, use Git sparse checkout:

```sh
git clone --filter=blob:none --sparse https://github.com/haneoka-gakuen/ournotes-masterdata.git
cd ournotes-masterdata
git sparse-checkout set jp tools
```

## Snapshots and downloads

`main` contains complete published datasets. Pin a `jp-<master-version>` or `intl-<master-version>` tag for a reproducible snapshot. The version follows the Master generation. The Master content identity is stored separately as `hash`.

[GitHub Releases](https://github.com/haneoka-gakuen/ournotes-masterdata/releases) provide complete archives and `SHA256SUMS`:

```sh
sha256sum -c SHA256SUMS
# For a split archive, concatenate parts in filename order:
cat jp.tar.gz.part* > jp.tar.gz
tar -xzf jp.tar.gz
```

Validate a local full checkout with `python tools/release.py --tag jp-1.1.0.200 --check-only`. This verifies the complete file digest and local Cubism dependency closure where applicable.

## Automatic updates

[Our Notes Data Pipeline](https://github.com/haneoka-gakuen/ournotes-data-pipeline) checks official Master and MoeNotes version signals every 20 minutes and exports successful Haneoka resource builds. This repository consumes the sealed feed with its own short-lived `GITHUB_TOKEN`; it needs no cross-repository credentials. Data commits occur when the dataset or its version record changes. Every file is verified before the complete snapshot reaches `main`; release tags never request another resource build.

Master tables are read directly from the official production Master service, independently of asset-catalog updates.

## Related projects

[Masterdata](https://github.com/haneoka-gakuen/ournotes-masterdata) · [Stories](https://github.com/haneoka-gakuen/ournotes-stories) · [Charts](https://github.com/haneoka-gakuen/ournotes-charts) · [Live2D](https://github.com/haneoka-gakuen/ournotes-live2d) · [Spine](https://github.com/haneoka-gakuen/ournotes-spine) · [Resource catalog](https://github.com/haneoka-gakuen/ournotes-resource-catalog) · [Automation](https://github.com/haneoka-gakuen/ournotes-data-pipeline)

Maintained by [Haneoka](https://haneoka.org). See [NOTICE.md](NOTICE.md) for game-content attribution and [LICENSE](LICENSE) for the helper-code license.
