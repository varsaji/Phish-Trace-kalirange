<div align="center">

<img src="docs/logo.svg" alt="KaliRange logo" width="88">

# PhishTrace

**Offline phishing email forensics, by [KaliRange](https://kalirange.com).**

Give it a suspicious `.eml` file and get a 0-100 risk score, a verdict, plain-English findings, a delivery trace and defanged indicators of compromise.

[![Latest release](https://img.shields.io/github/v/release/varsaji/Phish-Trace-kalirange?color=d62e2e&label=release)](https://github.com/varsaji/Phish-Trace-kalirange/releases/latest)
[![Downloads](https://img.shields.io/github/downloads/varsaji/Phish-Trace-kalirange/total?color=13161f)](https://github.com/varsaji/Phish-Trace-kalirange/releases)
![Platforms](https://img.shields.io/badge/platforms-macOS%20%7C%20Linux%20%7C%20Windows-5b6272)
[![License](https://img.shields.io/badge/license-free%20to%20use-5b6272)](LICENSE)

**[Download](#install)** · **[Quick start](#analyze-your-first-email)** · **[Usage](#usage)** · **[Report a bug](https://github.com/varsaji/Phish-Trace-kalirange/issues)**

<br>

<img src="docs/demo.gif" alt="PhishTrace analyzing a fake bank login email in the terminal" width="820">

</div>

> **For defensive and educational use only.** PhishTrace is for analysts, students and teachers investigating suspicious mail. Do not use it to build, test or improve phishing campaigns.

- **Offline by default.** Your email stays on your machine. Network lookups only happen with `--online`.
- **Safe with hostile mail.** Attachments are never opened and links are never visited. Every indicator is defanged.
- **No setup.** One program, with no Python or other software to install.

## Why it matters

Phishing remains one of the most common ways into an organization. Proper analysis means reading headers, tracing delivery paths and inspecting links, which is hard to learn from a raw `.eml` file. PhishTrace does that work and explains every result, so students learn the reasoning as well as the answer.

## Install

No Python is needed. Open a terminal and run the commands for your system. They download PhishTrace, verify its checksum, install the `phishtrace` command and show its version.

### macOS, Apple Silicon (M1 and later)

```bash
curl -fLO https://github.com/varsaji/Phish-Trace-kalirange/releases/latest/download/phishtrace-macos-arm64
curl -fLO https://github.com/varsaji/Phish-Trace-kalirange/releases/latest/download/phishtrace-macos-arm64.sha256
shasum -a 256 -c phishtrace-macos-arm64.sha256
chmod +x phishtrace-macos-arm64
sudo mv phishtrace-macos-arm64 /usr/local/bin/phishtrace
rm phishtrace-macos-arm64.sha256
phishtrace version
```

### macOS, Intel

```bash
curl -fLO https://github.com/varsaji/Phish-Trace-kalirange/releases/latest/download/phishtrace-macos-x64
curl -fLO https://github.com/varsaji/Phish-Trace-kalirange/releases/latest/download/phishtrace-macos-x64.sha256
shasum -a 256 -c phishtrace-macos-x64.sha256
chmod +x phishtrace-macos-x64
sudo mv phishtrace-macos-x64 /usr/local/bin/phishtrace
rm phishtrace-macos-x64.sha256
phishtrace version
```

### Linux, x64

```bash
curl -fLO https://github.com/varsaji/Phish-Trace-kalirange/releases/latest/download/phishtrace-linux-x64
curl -fLO https://github.com/varsaji/Phish-Trace-kalirange/releases/latest/download/phishtrace-linux-x64.sha256
sha256sum -c phishtrace-linux-x64.sha256
chmod +x phishtrace-linux-x64
sudo mv phishtrace-linux-x64 /usr/local/bin/phishtrace
rm phishtrace-linux-x64.sha256
phishtrace version
```

`sudo` asks for your password so the command can be installed for all users.

### Windows, x64

Run these in PowerShell:

```powershell
$dir = "$env:LOCALAPPDATA\PhishTrace"
$url = "https://github.com/varsaji/Phish-Trace-kalirange/releases/latest/download/phishtrace-windows-x64.exe"
New-Item -ItemType Directory -Force $dir | Out-Null
Invoke-WebRequest $url -OutFile "$dir\phishtrace.exe"
$expected = (-split (Invoke-RestMethod "$url.sha256"))[0]
if ((Get-FileHash "$dir\phishtrace.exe").Hash -ne $expected) { Remove-Item "$dir\phishtrace.exe"; throw "Checksum failed" }
[Environment]::SetEnvironmentVariable("Path", [Environment]::GetEnvironmentVariable("Path", "User") + ";$dir", "User")
```

Close PowerShell, open it again, then check it works:

```powershell
phishtrace version
```

### Download in your browser instead

| System | Download |
| --- | --- |
| macOS, Apple Silicon | [`phishtrace-macos-arm64`](https://github.com/varsaji/Phish-Trace-kalirange/releases/latest/download/phishtrace-macos-arm64) |
| macOS, Intel | [`phishtrace-macos-x64`](https://github.com/varsaji/Phish-Trace-kalirange/releases/latest/download/phishtrace-macos-x64) |
| Linux, x64 | [`phishtrace-linux-x64`](https://github.com/varsaji/Phish-Trace-kalirange/releases/latest/download/phishtrace-linux-x64) |
| Windows, x64 | [`phishtrace-windows-x64.exe`](https://github.com/varsaji/Phish-Trace-kalirange/releases/latest/download/phishtrace-windows-x64.exe) |

The programs aren't signed yet, so browser downloads trigger a security prompt. On macOS, after moving the file to `/usr/local/bin/phishtrace`, allow it with:

```bash
sudo xattr -d com.apple.quarantine /usr/local/bin/phishtrace
```

On Windows, if SmartScreen warns you, check the checksum, then choose **More info** and **Run anyway**.

The first run can take a few seconds while the program unpacks itself. Later runs start almost instantly.

## Analyze your first email

1. **Save a suspicious email as an `.eml` file.**
   - **Gmail:** open the email, click the three dots, then **Download message**.
   - **Apple Mail:** drag the email to your Desktop, or choose **File, Save As** and pick **Raw Message Source**.
   - **Thunderbird:** choose **File, Save As, File**.
   - Outlook `.msg` files are not supported yet (see the roadmap).
2. **Run PhishTrace on it.** A name is enough. PhishTrace looks in the current folder, Downloads, Desktop and Documents.

   ```bash
   phishtrace analyze invoice
   ```

   Or give the full path: `phishtrace analyze ~/Downloads/invoice.eml`.
3. **Read the result.** The box at the top shows the verdict and score, and the findings explain each reason in plain English with a "Do:" tip. Never click links or open attachments in the email itself.

## Usage

```bash
phishtrace analyze invoice.eml                       # readable report
phishtrace analyze invoice.eml --format html --output report.html
phishtrace analyze invoice.eml --format json              # for scripts
phishtrace analyze invoice.eml --format markdown --output report.md
phishtrace iocs invoice.eml --format csv            # defanged indicators
```

You can pass a path or just a name. `phishtrace analyze invoice` finds `invoice.eml` in the current folder, Downloads, Desktop or Documents. Running `phishtrace` alone shows the banner and usage.

| Option | Commands | Purpose |
| --- | --- | --- |
| `--format` | analyze, iocs | `terminal`, `json`, `html`, `markdown`; for iocs `json`, `csv` |
| `--output PATH` | analyze, iocs | Write to a file |
| `--online` | analyze | Allow WHOIS and DNS lookups |
| `--config PATH` | analyze | Use a custom scoring file |
| `--no-color` | analyze, iocs | Disable color |
| `--no-banner` | analyze, iocs | Hide the banner |

The banner also hides itself when output is piped or `--format json` is passed.

### Sample output

```text
╭──────────────────────────────────────────────────────────────────────────────╮
│ ALMOST CERTAINLY PHISHING  100/100                                           │
│ From: Meridian Trust Bank <security@meridiantrust-alerts[.]example>          │
│ Subject: URGENT: Your account will be suspended                              │
│ Auth: SPF=fail  DKIM=none  DMARC=fail                                        │
│ File: 03_fake_bank_login.eml                                                 │
╰──────────────────────────────────────────────────────────────────────────────╯
Why this score
  - Sender name imitates a brand (+25)
  - Link text shows a different destination (+25)
  - DMARC failed (+20)
  - Domain adds words to a brand name (+20)
  - Link points to a bare IP address (+20)
  Raw total 180, capped at 100.
```

### Exit codes

| Code | Meaning |
| --- | --- |
| 0 | `analyze`: likely safe. Other commands: success |
| 1 | `analyze`: suspicious or worse |
| 2 | Error |

`phishtrace iocs` returns 0 on success because it gives no verdict.

## What it checks

| Area | Examples |
| --- | --- |
| Headers | From, Return-Path and Reply-To mismatch; display name spoofing; SPF, DKIM, DMARC; Message-ID; date anomalies |
| Hop trace | Ordered route, delays, private or odd IPs, time gaps, likely origin IP |
| Lookalike domains | Homoglyphs, punycode, typosquats, brand-plus-word domains, subdomain tricks |
| Links | Text and target mismatch, IP URLs, shorteners, `@` tricks, risky TLDs, redirects, HTTP, hidden links, `javascript:` |
| Body | Urgency, threats, credential and payment requests, gift cards, crypto, forms, hidden text, tracking pixels, QR lures |
| Attachments | Dangerous types, double extensions, macros, encrypted archives, disguised names, executable content, SHA-256 |
| `--online` | Domain age through WHOIS, basic DNS and MX |

## How scoring works

Each kind of finding has a weight, counted once per email. The weights are added and the total is capped at 100.

| Score | Verdict |
| --- | --- |
| 0-24 | Likely safe |
| 25-49 | Suspicious |
| 50-74 | Likely phishing |
| 75-100 | Almost certainly phishing |

Every report lists the top reasons and the points for each finding. The uncapped total is kept as `raw_score`. You can adjust scoring with a TOML file passed to `--config`. It only needs what you change, and finding ids appear in the JSON report:

```toml
[weights]
"body.tracking_pixel" = 0

[verdicts]
suspicious = 30
```

## Safety

- Attachments are never opened, executed, extracted or rendered. Only their bytes are hashed and sniffed.
- URLs in emails are never fetched.
- All URLs, domains and IPs in output are defanged, such as `hxxp://example[.]com`.
- Email HTML is never rendered or displayed as a page. It is only scanned as text.
- Input is limited to 10 MB and `.eml`, and nothing is stored.
- Hostile text cannot inject terminal escapes, HTML, Markdown or spreadsheet formulas.
- No network access happens without `--online`. With it, domain names go to WHOIS and DNS servers, and DNS may fall back to 1.1.1.1. An attacker watching their own domain's DNS could notice the lookup.

## Limitations

- Heuristic results, not proof. A low score does not mean an email is safe, so well-written targeted phishing can score low.
- DKIM signatures and SPF records are not verified. Results are read from headers added by the receiving server, so a file that never passed through a trusted server can contain fake results.
- Received headers below your own servers can be forged, so the origin IP is only a likely one.
- QR codes in images are not decoded, and images are not analyzed.
- Attachment contents and encrypted archives are not inspected. Attached emails are hashed, not analyzed.
- Body keyword checks are English only.
- Brand and registered-domain detection use small built-in lists, not the full Public Suffix List. A brand name on a different TLD, such as `paypal.net`, is not flagged.
- Ordinary internal relays can add a small `hop.private_ip` score. Well-authenticated marketing newsletters can still land in "Suspicious" because of hidden preheader text, tracking pixels and unlabeled links, so read the reasons before deciding.
- WHOIS formats vary, so domain age may be unavailable.

## Help and feedback

Report bugs through [Issues](https://github.com/varsaji/Phish-Trace-kalirange/issues). For security problems, follow [SECURITY.md](SECURITY.md).

## License

PhishTrace is free to use but is not open source. Copyright (c) 2026 Uzair Varsaji, trading as KaliRange. All rights reserved. See the [license](LICENSE).
