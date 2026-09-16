# FIE preset archive

This directory publishes readable, signed FIE test-parameter presets for the SVA-Fencing-Tester.

## Current preset

The current package is identified by [`latest.json`](./latest.json). It references the signed file in [`stable/`](./stable/) and includes its SHA-256 checksum for the browser download flow.

- Preset version: `1`
- Preset date: `2026-08-18`
- Rule reference: FIE Material Rules, `December 2025`
- Source: [FIE Material Rules PDF](https://static.fie.org/uploads/38/190667-book%20m%20ang.pdf)

## Use and verification

The device webapp can download the current preset through the user's browser, or the JSON file can be downloaded here and imported manually. In both cases, the ESP verifies the schema, values, key ID and ECDSA P-256/SHA-256 signature itself before it stores or activates a preset.

The manifest checksum is an additional transport check only. A modified portal file, manifest or browser response cannot activate a changed preset without a valid device-trusted signature.

The source URL is the signed reference snapshot for this particular preset; it is not a promise that the linked FIE document remains reachable or the newest publication. The external FIE website is outside this project's control. A changed FIE source, rule edition or URL requires a new dated, versioned and signed preset. Published preset files are never edited retroactively. Devices continue to show and use their imported preset until the user explicitly loads or imports a newer one.

## Publication rule

Each new release keeps its dated signed JSON package in `stable/` and updates `latest.json` only after the package, its SHA-256 value, FIE source document and source month/year have been reviewed together. The source version uses the exact form `Month YYYY`, for example `December 2025`.

This preset identifies a selected test-parameter profile. It is not an official FIE logo, certification, or a certification of a test result or device.
