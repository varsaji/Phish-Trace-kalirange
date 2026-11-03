# Security Policy

## Reporting a vulnerability

Please do not open a public issue. Use GitHub's private vulnerability reporting on this repository, and include steps to reproduce with a synthetic email.

## In scope

PhishTrace handles hostile input, so these count as vulnerabilities:

- A crafted email that executes code, or that crashes or hangs the analyzer
- Output that is not defanged, or that injects HTML, script, terminal escapes, Markdown or spreadsheet formulas
- Any network access without `--online`
- Bypass of the file size or `.eml` input limits

## Verifying downloads

Each release lists a SHA-256 checksum for every program. Only download PhishTrace from this repository's Releases page, and check the checksum before running it.

## Data

PhishTrace is a local command-line tool and opens no network ports. Treat emails as sensitive: they can hold personal data.
