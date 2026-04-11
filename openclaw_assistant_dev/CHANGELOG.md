## [0.6.0.9] - 2026-04-11
- Fix Dockerfile: Changed CMD to /run.sh to resolve systemd startup loop.
- Fix Dockerfile: Added missing essential packages (nginx, jq, openssl, pnpm, ttyd) and infrastructure files (COPY/chmod).
- Fix run.sh: Validated mDNS variable logic.
