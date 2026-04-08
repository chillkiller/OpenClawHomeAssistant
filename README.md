# OpenClaw Ultimate - Home Assistant Addon

Das ultimative OpenClaw Home Assistant Addon mit voller Kontrolle über Versionen, Multi-Agent Support und eingebettetem OpenClaw Core.

## Features

- ✅ **Pinned Version**: Keine Überraschungen durch automatische Updates
- ✅ **Multi-Agent Ready**: Subagenten funktionieren (v2026.3.28)
- ✅ **QMD Memory**: Integrierter Quantum Memory Database Support
- ✅ **GitHub Actions**: Automatische Builds für alle Architekturen
- ✅ **Konfigurierbar**: Version, Ports, Orchestrator einstellbar

## Installation

1. Dieses Repository als Custom Repository in Home Assistant hinzufügen
2. Addon "OpenClaw Ultimate" installieren
3. Starten und Konfigurieren

## Konfiguration

```yaml
openclaw_version: "2026.3.28"  # oder "2026.4.8" wenn Bug gefixt
gateway_port: 18790
control_ui_port: 18789
log_level: info
multi_agent:
  enabled: true
  orchestrator: native  # native | paperclip | antfarm
memory:
  backend: qmd
  enabled: true
```

## Multi-Agent Setup

### Option 1: Native (Subagenten)
Funktioniert mit v2026.3.28. Neueere Versionen haben einen Bug (siehe OpenClaw Issue #59428).

### Option 2: Paperclip (kommt bald)
Externe Orchestrierung via Paperclip Platform.

### Option 3: Antfarm (kommt bald)
YAML-basierte Workflows via Antfarm.

## Entwicklung

### Eigenen OpenClaw Fork einbinden

In `Dockerfile` ändern:
```dockerfile
ARG OPENCLAW_REPO=https://github.com/YOUR_USERNAME/openclaw.git
ARG OPENCLAW_BRANCH=your-fix-branch
```

### Build lokal testen

```bash
docker build --build-arg BUILD_FROM=ghcr.io/hassio-addons/debian-base:7.3.4 -t openclaw-ultimate .
```

## Troubleshooting

### "pairing required" Error
Dieser Bug existiert in v2026.4.1+. Lösung:
- Downgrade auf v2026.3.28 in der Config
- Oder eigenen Fork mit Fix bauen

### QMD Collection nicht gefunden
Normal beim ersten Start. QMD wird automatisch initialisiert.

## Credits

- Original OpenClaw: https://github.com/openclaw/openclaw
- Home Assistant Addons: https://github.com/hassio-addons

## Lizenz

MIT
