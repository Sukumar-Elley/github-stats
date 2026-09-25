# GitHub Stats Visualization

A personal, self-hosted GitHub statistics generator for **[Sukumar-Elley](https://github.com/Sukumar-Elley)**.

This repository is a customized derivative of [jstrieb/github-stats](https://github.com/jstrieb/github-stats). It keeps the original Zig-based statistics engine and GitHub Actions architecture, while adapting the workflow and documentation for this profile.

## What it generates

The GitHub Actions workflow periodically generates:

- `overview.svg` — profile/repository contribution statistics
- `languages.svg` — language statistics

The generated SVG files are committed to the `generated` branch and can be embedded directly in the profile README.

## Architecture

```text
GitHub API
    |
    v
github-stats (Zig 0.16)
    |
    +--> repository / contribution statistics
    +--> language statistics
    +--> SVG template rendering
    |
    v
generated branch
    +--> overview.svg
    +--> languages.svg
    |
    v
Sukumar-Elley GitHub Profile README
```

## GitHub Actions configuration

The workflow runs:

- on pushes to `main`
- daily at 00:05 UTC
- manually through GitHub Actions

### Required secret

Create an Actions repository secret named:

```text
ACCESS_TOKEN
```

The token needs the permissions required by the upstream project to read the GitHub data used for the statistics, including private repositories when you want them included.

Optional configuration secrets:

```text
EXCLUDE_REPOS
EXCLUDE_LANGS
```

For this repository, excluding `Sukumar-Elley/github-stats` itself is recommended so the stats generator does not materially affect its own statistics.

## Profile README

After the first successful workflow run, add the generated images to the profile README:

```markdown
<p align="center">
  <img src="https://raw.githubusercontent.com/Sukumar-Elley/github-stats/generated/overview.svg" width="75%" alt="GitHub overview statistics" />
</p>

<p align="center">
  <img src="https://raw.githubusercontent.com/Sukumar-Elley/github-stats/generated/languages.svg" width="75%" alt="GitHub language statistics" />
</p>
```

Using `raw.githubusercontent.com` is intentional: it provides a direct SVG asset URL suitable for README image rendering.

## Local development

The project uses Zig **0.16.0**.

Build:

```bash
zig build
```

Run tests:

```bash
zig build test
```

Run the application:

```bash
zig build run
```

Build release binaries:

```bash
zig build release
```

## Upstream and license

Original project:

- [jstrieb/github-stats](https://github.com/jstrieb/github-stats)

This repository contains modified/redistributed GPL-licensed upstream material. The original `LICENSE` is preserved. Changes in this repository are intended to remain distinguishable from the upstream project.
