[![GitHub Workflow Status](https://img.shields.io/github/workflow/status/lietu/gcrc/Build%20and%20upload%20to%20PyPI)](https://github.com/lietu/gcrc/actions/workflows/build-and-upload.yaml)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)
[![PyPI](https://img.shields.io/pypi/v/gcrc)](https://pypi.org/project/gcrc/)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/gcrc)](https://pypi.org/project/gcrc/)
[![License: BSD 3-Clause](https://img.shields.io/badge/License-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)

# Google Container Registry Cleanup utility

If you use a CI system to push images to Google Container Registry you will eventually run into a situation where you're paying more than you would like for the storage there.

This tool helps you keep those costs reasonable, while allowing you to benefit from caches, keeping backups for rollbacks, etc.

## Prerequisites and setup

- [Python 3.9+](https://www.python.org/downloads/)
- [Gcloud SDK](https://cloud.google.com/sdk/docs/install) with an authentication token that has admin access to the registry properly configured

```bash
pip install -U gcrc
```

## Usage

Replace `gcr.io/project-name` with the specific GCR address to your repository, e.g. if your Google Cloud project is called `foo-bar` and it's in the EU zone this would likely be `eu.gcr.io/foo-bar`.

### List all images

Figures out the different images we can access in the Container Registry.

```bash
gcrc list-images gcr.io/project-name
```

### Show information on images

Analyzes current images and any need for cleanup based on current configuration without actually deleting anything. Use to check that configuration seems correct.

```bash
gcrc image-info gcr.io/project-name
```

### Clean up images

Look up images with unneeded tags and delete them from Google Container Registry

```bash
gcrc cleanup gcr.io/project-name
```

## Configuration

The following environment variables can be used to adjust the configuration:

```
KEEP_TAGS_MIN=10
```

Keeps at least this many tags for every image.

```
KEEP_TAGS_DAYS=14
```

Keep everything from within this many days.

```
KEEP_EXTRA='["^latest$", "^(master|main)-"]'
```

List of regex matches for important images that we want to keep an extra `KEEP_TAGS_MIN` items of. Formatted as a JSON list of strings.

The default is to keep `latest` and any tags starting with `master-` or `main-` for people who tag their images like `<branch>-<timestamp or commit hash or uuid>`.

You can also put these in a `.env` -file, but environment variables take priority over `.env`.

```
# .env
KEEP_TAGS_MIN=10
KEEP_TAGS_DAYS=14
KEEP_EXTRA='["^important-", "^latest$"]'
```

# Financial support

This project has been made possible thanks to [Cocreators](https://cocreators.ee) and [Lietu](https://lietu.net). You can help us continue our open source work by supporting us on [Buy me a coffee](https://www.buymeacoffee.com/cocreators).

[!["Buy Me A Coffee"](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.buymeacoffee.com/cocreators)
