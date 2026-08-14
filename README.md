# GitHub Actions Workflows

A collection of reusable GitHub Actions workflows for automating builds and releases across projects.

## Available Workflows

### Python Build & Release

Build Python applications into standalone Windows `.exe` files using PyInstaller and optionally attach them to GitHub Releases.

**Features:**

* Python setup
* Dependency installation
* PyInstaller builds
* Multiple `.py` files support
* Windows `.exe` artifacts
* Automatic GitHub Releases

---

## Usage

Create this file in your project:

```text
.github/workflows/build.yml
```

Then add:

```yaml
name: Build and Release

on:
  push:
    branches:
      - main
    tags:
      - "v*"

  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    uses: aryanariman/github-actions-workflows/.github/workflows/python-build-release.yml@main

    with:
      files: |
        main.py
```

Replace `main.py` with the Python file you want to build.

### Multiple Python Files

To build multiple `.exe` files, list each Python file:

```yaml
with:
  files: |
    main.py
    Tool.py
    AnotherTool.py
```

Each file will be compiled into its own `.exe`.

```text
main.py        → main.exe
Tool.py        → Tool.exe
AnotherTool.py → AnotherTool.exe
```

---

## Requirements

### Repository Permissions

The repository using this workflow must allow GitHub Actions to have **Read and write permissions**.

Go to:

**Settings → Actions → General → Workflow permissions**

and select:

**Read and write permissions**

The workflow must also include:

```yaml
permissions:
  contents: write
```

This is required because the workflow creates GitHub Releases and uploads the generated `.exe` files.

---

### Python Dependencies

If your project has a `requirements.txt` file, specify it like this:

```yaml
with:
  files: |
    main.py

  requirements: "requirements.txt"
```

The workflow will automatically install the dependencies before building.

---

## Python Version

The default Python version is **3.13**.

You can change it for a specific project:

```yaml
with:
  files: |
    main.py

  python-version: "3.12"
```

---

## Releases

The workflow automatically creates a GitHub Release when a version tag is pushed.

For example:

```bash
git tag v1.0.0
git push origin v1.0.0
```

The workflow will:

```text
Build
  ↓
Create .exe files
  ↓
Create Release v1.0.0
  ↓
Attach the .exe files
```

Normal pushes to `main` will build the executables and upload them as GitHub Actions artifacts, but **will not create a Release**.

---

## Example

A complete workflow for a project with two Python applications:

```yaml
name: Build and Release

on:
  push:
    branches:
      - main
    tags:
      - "v*"

  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    uses: aryanariman/github-actions-workflows/.github/workflows/python-build-release.yml@main

    with:
      files: |
        TextMerger&RestoreTool.py
        TextMerger&RestoreTool-DirEdition.py

      python-version: "3.13"
```

That's it.

The actual build and release process is maintained in this repository, so individual projects only need a small workflow file.

---

## Repository Structure

```text
.github/
└── workflows/
    └── python-build-release.yml
```

This repository acts as the central source for reusable GitHub Actions workflows across projects.
