# FluXBrowser

FluXBrowser is the browser project maintained by CrazeStudio. This repository currently contains the project setup and a **manual, self-hosted Chromium-for-Android build workflow**.

## Important: Chromium is not copied into this repository

Chromium's source tree is extremely large and is managed through Google's `depot_tools` / `gclient` workflow. Committing a partial or fabricated Chromium tree would not produce a working browser. The workflow fetches the upstream source on a suitably provisioned runner instead.

## Build requirements

The workflow requires a dedicated Linux x86-64 self-hosted GitHub Actions runner with:

- At least 16 CPU cores recommended (more is better).
- 32 GB RAM minimum; 64 GB recommended.
- Roughly 200–300 GB of free disk space for source, dependencies, and build output.
- Java, Python 3, Git, standard Android build dependencies, and reliable high-bandwidth internet.
- GitHub Actions runner labels: `self-hosted`, `linux`, `x64`, and `chromium-build`.

A standard GitHub-hosted runner is not a realistic environment for a full Chromium Android build due to disk, memory, and execution-time constraints. Do not add repository secrets for this workflow; it does not need API passwords or deployment credentials.

## Run the build

1. Open **Actions → Build Chromium for Android**.
2. Choose **Run workflow** and confirm the branch.
3. The runner installs/updates `depot_tools`, fetches Chromium's Android source, synchronizes dependencies, generates an ARM64 release build, and invokes `autoninja`.
4. If the build succeeds, the workflow uploads the generated `ChromePublic.apk` as an artifact.

The first run downloads a very large source/dependency tree and can take hours. Subsequent runs may be faster if the self-hosted runner retains its Chromium checkout.

## What this does — and does not — produce

This workflow builds the upstream Chromium `chrome_public_apk` target for Android ARM64. It is a **baseline build pipeline**, not yet a branded FluXBrowser application. FluXBrowser-specific name, package ID, icons, default search engine, UI changes, signing, and release distribution still need implementation. Do not distribute an APK as a finished FluXBrowser release until those changes and licensing/notices have been reviewed.

## Source and licensing

- Chromium source: https://chromium.googlesource.com/chromium/src/
- Chromium build documentation: https://chromium.googlesource.com/chromium/src/+/main/docs/android_build_instructions.md
- Chromium is an open-source project with multiple licenses; retain upstream notices and comply with the applicable licenses when modifying or redistributing it.
