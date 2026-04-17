## [0.7.5] - 2026-04-17
- **CRITICAL FIX:** jq-Falsy-Falle – Alle `// true`/`// false` durch Null-Checks ersetzt
- **FIX:** `CONTROLUI_DISABLE_DEVICE_AUTH=true` im `lan_https`-Case entfernt (trustedProxies reicht)
- **FIX:** `controlui_disable_device_auth` Default auf `false` (war `true`)
- **FIX:** Dockerfile aufgeräumt (doppelte ENV, leerer apt-run, doppelter npm cache clean)
- **FIX:** `ensure-plugins` in oc_config_helper.py sichert `plugins.entries.ollama`
- **UPGRADE:** OpenClaw 2026.4.14 → 2026.4.15 (Security-Fix für dangerouslyDisableDeviceAuth-Patch-Blocking #62006, Tool-Name-Collision #67303, Webchat-Audio-Path #67298)
- **FIX:** `build.yaml` entfernt (HA-supervisor-obsolet)
- Repo aufgeräumt: Backup-Dateien, __pycache__ entfernt, .gitignore erweitert

## [0.6.1.13] - 2026-04-13
- Version bump to trigger Home Assistant update (HA ignores versions <= 0.6.1.12)
- Fix run.sh: Reduced tmpfs sizes for 8GB RAM system to prevent WASM OOM (npm_cache 512M, node_tmp 256M, chromium_cache 256M, logs 128M in power mode; 256M/128M/128M/64M in safe mode).
- Fix run.sh: Lowered virtual memory limit ulimit -v from 8GB to 4GB.
- Fix run.sh: Added --experimental-wasm-max-mem-pages=65536 to NODE_OPTIONS to stabilize undici/llhttp Wasm instance.

## [0.6.1.7] - 2026-04-13

## [0.6.0.9] - 2026-04-11
- Fix Dockerfile: Changed CMD to /run.sh to resolve systemd startup loop.
- Fix Dockerfile: Added missing essential packages (nginx, jq, openssl, pnpm, ttyd) and infrastructure files (COPY/chmod).
- Fix run.sh: Validated mDNS variable logic.
