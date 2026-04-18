# Changelog — OpenClaw Assistant (Dev)

## [0.7.5.1] - 2026-04-18
- **FIX:** D-Bus system bus is now started before Avahi (containers lack systemd, so `run.sh` must start D-Bus explicitly — without it, Avahi exits silently)
- **FIX:** `allowedOrigins` is dynamically extended with `${mdns_host_name}.local` when `mdns_host_name` is configured (fixes "origin not allowed" when accessing via mDNS hostname)
- **FIX:** TLS-SANs are dynamically extended with `DNS:${mdns_host_name}.local` (prevents browser certificate warnings)
- **FIX:** mDNS now correctly advertises `GATEWAY_PORT` (18789) instead of `NGINX_PORT` (48099 Ingress port) in HTTPS mode
- **FIX:** Typo `avaihi-daemon` → `avahi-daemon` in warning message

## [0.7.5] - 2026-04-17
- **FIX:** jq falsy trap — replaced all `// true`/`// false` with null checks (prevents empty strings being interpreted as `true`)
- **FIX:** Removed `CONTROLUI_DISABLE_DEVICE_AUTH=true` in `lan_https` case (trustedProxies is sufficient)
- **FIX:** Corrected `controlui_disable_device_auth` default to `false` (was `true`)
- **FIX:** Cleaned up Dockerfile (duplicate ENV, empty apt-run, duplicate npm cache clean)
- **FIX:** `ensure-plugins` in `oc_config_helper.py` now safeguards `plugins.entries.ollama`
- **UPGRADE:** OpenClaw 2026.4.14 → 2026.4.15
- **CLEANUP:** Removed `build.yaml` (HA Supervisor obsolete), backup files and `__pycache__`, extended `.gitignore`

## [0.7.1.0] - 2026-04-14
- **Operation Chromium Consolidation:** Only one Chromium (Playwright) in the image, system Chromium removed. Symlink `/usr/bin/chromium-browser` → Playwright binary.
- **FIX:** Log routing corrected — console output restored, trace logs to file

## [0.7.0] - 2026-04-13
- **REWRITE:** Best-of-All-Worlds — Trixie Full-Stack + coollabsio Persistence + techartdev HA Integration
- **FEAT:** mDNS configuration options for LAN discovery
- **FEAT:** Runtime apt packages (coollabsio pattern)
- **FEAT:** Custom init script (coollabsio pattern)
- **FEAT:** Log rotation (files > 10MB rotate)
- **FEAT:** Exponential backoff on gateway restarts

## [0.6.1.13] - 2026-04-13
- **FIX:** Reduced tmpfs sizes for 8GB RAM (prevents WASM OOM)
- **FIX:** Removed virtual memory limit (`ulimit -v` was too restrictive for WASM)
- **FIX:** Added `--experimental-wasm-max-mem-pages=65536` to NODE_OPTIONS

## [0.6.1.7] - 2026-04-13
- **FIX:** Disabled tmpfs mounts (CAP_SYS_ADMIN missing in HA containers)

## [0.6.0.9] - 2026-04-11
- **FIX:** Changed Dockerfile CMD to `/run.sh` (systemd startup loop)
- **FIX:** Added missing packages (nginx, jq, openssl, pnpm, ttyd)
- **FIX:** Validated mDNS variable logic

## [0.6.x] - 2026-04 (early versions)
- Node.js 20 musl → Node 22 upgrade
- Gateway bind/port/token configurable
- Telegram allowlist support
- libstdc++/Node compatibility fixes for Alpine musl
- Initial HA add-on skeleton (based on Papur add-ons)