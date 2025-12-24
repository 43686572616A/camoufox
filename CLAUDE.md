# Camoufox Fork - Development Guide

## Project Goal

Create an updated, maintained fork of [daijro/camoufox](https://github.com/daijro/camoufox) that resolves existing issues and improves the anti-detect browser capabilities.

---

## What is Camoufox?

Camoufox is an **open-source anti-detect browser** built on Firefox, designed for:
- Web scraping without detection
- Bot evasion and fingerprint spoofing
- Bypassing anti-bot systems (Cloudflare, hCaptcha, etc.)

### Key Technical Details

| Aspect | Details |
|--------|---------|
| **Base** | Custom Firefox build |
| **Core Language** | C++ (fingerprinting), Python (interface) |
| **Protocol** | Juggler (lower-level than Chrome CDP) |
| **Distribution** | PyPI package (`pip install camoufox`) |
| **License** | MPL-2.0 |

### Why Firefox over Chromium?
- Chrome includes proprietary features Chromium lacks, aiding detection
- Firefox research provides superior fingerprinting resistance
- Juggler protocol operates at a lower level than Chrome's CDP

---

## Repository Structure

```
camoufox/
├── .github/          # CI/CD workflows
├── additions/        # Custom Firefox modifications
├── bundle/           # Bundled resources (fonts, etc.)
├── patches/          # Firefox source patches
├── pythonlib/        # Python wrapper/interface (camoufox package)
├── settings/         # Configuration files
├── tests/            # Test suite
├── scripts/          # Build and utility scripts
└── Makefile          # Build system
```

---

## Core Features

### Fingerprint Spoofing (C++ Level)
- Navigator properties (device, OS, hardware, browser)
- Screen dimensions, resolution, viewport
- Geolocation, timezone, locale, i18n
- WebRTC IP masking (protocol level)
- Audio characteristics (voices, speech rates)
- Canvas anti-fingerprinting
- WebGL parameter/extension spoofing
- Font spoofing

### Python API Features
- Sync and Async Playwright integration
- Human-like mouse movement (`humanize` parameter)
- Headless and virtual headless modes
- Firefox addon support
- Persistent contexts
- GeoIP-based locale configuration

---

## Known Issues (170+ Open)

### Critical Detection Issues
| Issue | Description | Priority |
|-------|-------------|----------|
| #424 | Detected by hCaptcha | HIGH |
| #423 | hCaptcha model definition files not downloading | HIGH |
| #420 | Facebook auth_platform detected | HIGH |
| #388 | 100% detection rate by Google | HIGH |

### Platform/Docker Issues
| Issue | Description | Priority |
|-------|-------------|----------|
| #431 | Docker GUI/non-headless mode fails | HIGH |
| #414 | Docker container issues | HIGH |
| #417 | WSL virtual headless shows window | MEDIUM |
| #376 | NixOS launch failure | MEDIUM |
| #377 | Debian 13 download issues | MEDIUM |

### Functionality Bugs
| Issue | Description | Priority |
|-------|-------------|----------|
| #428 | Page.route causes page loading issues | HIGH |
| #425 | Window larger than screen with display scaling | MEDIUM |
| #418 | Execution pauses when window unfocused | MEDIUM |
| #421 | canvas:aaOffset should accept float, not int | LOW |
| #416 | navigator.oscpu undefined | LOW |
| #391/#386 | Second instance breaks first | HIGH |
| #383 | Can't pass args to main world eval functions | MEDIUM |

### Enhancement Requests
| Issue | Description | Priority |
|-------|-------------|----------|
| #430 | XDG Base Directory specification compliance | LOW |
| #389 | Publish on Flathub | LOW |
| #387 | PyInstaller bundling support | MEDIUM |
| #378 | Expand fingerprint parameter properties | MEDIUM |
| #380 | Avoid WebGL/Canvas spoofing option | LOW |

---

## Development Workflow

### Roles
- **Claude (AI)**: Programmer - reads code, writes fixes, implements features
- **User**: Tester - tests changes, reports results, gives instructions

### Process
1. User identifies issue or feature to work on
2. Claude analyzes the relevant code
3. Claude implements the fix/feature
4. User tests the changes
5. Claude iterates based on feedback
6. Commit and move to next issue

---

## Technical Reference

### Python API Usage

```python
# Synchronous
from camoufox.sync_api import Camoufox

with Camoufox(
    headless=False,           # GUI mode
    humanize=True,            # Human-like mouse movement
    os="windows",             # OS fingerprint
    geoip=True,               # Auto-detect geolocation
    block_webrtc=True,        # Prevent WebRTC leaks
) as browser:
    page = browser.new_page()
    page.goto("https://example.com")
```

```python
# Asynchronous
from camoufox.async_api import AsyncCamoufox

async with AsyncCamoufox() as browser:
    page = await browser.new_page()
    await page.goto("https://example.com")
```

### Key Parameters
| Parameter | Type | Description |
|-----------|------|-------------|
| `os` | str/list | Target OS fingerprint |
| `headless` | bool/str | Headless mode (`'virtual'` for Linux) |
| `humanize` | bool/float | Human-like cursor movement |
| `geoip` | str/bool | GeoIP-based locale |
| `fonts` | list | Additional fonts to load |
| `addons` | list | Firefox extension paths |
| `block_webrtc` | bool | Block WebRTC |
| `block_webgl` | bool | Disable WebGL |
| `disable_coop` | bool | Allow cross-origin iframes |

---

## Build System

### Prerequisites
- Python 3.x
- Firefox source code
- Build tools for C++ compilation
- BrowserForge library

### Commands
```bash
# Install Python package (for usage)
pip install camoufox

# Build from source (development)
make build

# Run tests
make test
```

---

## Resources

- **Documentation**: https://camoufox.com/
- **Original Repo**: https://github.com/daijro/camoufox
- **PyPI**: https://pypi.org/project/camoufox/
- **BrowserForge**: Fingerprint generation library

---

## Session Notes

<!-- Add notes during development sessions here -->

### Session 1 - Initial Setup
- Created this claude.md file
- Next: Fork repository and clone locally
