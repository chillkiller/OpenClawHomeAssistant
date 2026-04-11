# Dockerfile for OpenClaw Dev-Addon v0.6.5
FROM debian:trixie-slim

ENV CHROMIUM_CACHE=/tmp/openclaw/chromium_cache
ENV CHROME_FLAGS="--disk-cache-dir=/tmp/openclaw/chromium_cache --disk-cache-size=524288000"

# Prevent interactive prompts
ENV DEBIAN_FRONTEND=noninteractive

# Install base dependencies and system libs
RUN apt-get update && apt-get install -y --no-install-recommends \
    curl \
    git \
    wget \
    sudo \
    ca-certificates \
    gnupg \
    lsb-release \
    build-essential \
    python3 \
    python3-pip \
    python3-full \
    ffmpeg \
    libvips-dev \
    liblapack3 \
    libopenblas-dev \
    libopus-dev \
    libnss3 \
    libatk1.0-0 \
    libatk-bridge2.0-0 \
    libcups2 \
    libdrm2 \
    libxkbcommon0 \
    libxcomposite1 \
    libxdamage1 \
    libxrandr2 \
    libgbm1 \
    libasound2 \
    nginx \
    jq \
    openssl \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js 22 LTS from NodeSource
RUN mkdir -p /etc/apt/keyrings \
    && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key \
    | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg \
    && chmod 644 /etc/apt/keyrings/nodesource.gpg \
    && echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_22.x nodistro main" \
    > /etc/apt/sources.list.d/nodesource.list \
    && apt-get update \
    && apt-get install -y --no-install-recommends nodejs \
    && rm -f /etc/apt/sources.list.d/nodesource.list /etc/apt/keyrings/nodesource.gpg \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Homebrew (Linuxbrew)
RUN useradd -m -s /bin/bash linuxbrew \
    && echo "linuxbrew ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
    # Download and verify Homebrew install script
    && curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh -o /tmp/install_homebrew.sh \
    && chmod +x /tmp/install_homebrew.sh \
    # Install Homebrew as linuxbrew user with proper environment and verbose output
    && sudo -u linuxbrew -i /bin/bash -c "NONINTERACTIVE=1 /tmp/install_homebrew.sh 2>&1 | tee /tmp/homebrew_install.log" \
    # Verify installation
    && sudo -u linuxbrew -i /bin/bash -c 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)" && brew --version' \
    && echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"' >> /home/linuxbrew/.bashrc \
    # Show installation log for debugging
    && cat /tmp/homebrew_install.log \
    # Cleanup
    && rm -f /tmp/install_homebrew.sh /tmp/homebrew_install.log

# Set Homebrew path for root and other users
ENV PATH="/home/linuxbrew/.linuxbrew/bin:/home/linuxbrew/.linuxbrew/sbin:${PATH}"
ENV HOMEBREW_BREW_FILE="/home/linuxbrew/.linuxbrew/Homebrew"

# Install Chromium via APT (Debian Trixie)
RUN apt-get update && apt-get install -y --no-install-recommends \
    chromium \
    && rm -rf /var/lib/apt/lists/*

# Install ttyd (web terminal) binary
ARG TARGETARCH
RUN ARCH=$(echo ${TARGETARCH:-$(dpkg --print-architecture)} | sed 's|arm64|aarch64|;s|arm/v7|armhf|;s|armv7|armhf|;s|amd64|x86_64|') \
    && curl -fsSL "https://github.com/tsl0922/ttyd/releases/download/1.7.7/ttyd.${ARCH}" -o /usr/local/bin/ttyd \
    && chmod +x /usr/local/bin/ttyd

# Install OpenClaw v0.6.5 (pinned version)
RUN npm install -g openclaw@2026.4.10 \
    && npm install -g pnpm

# Fix safe.directory
RUN git config --global safe.directory /home/linuxbrew/.linuxbrew/Homebrew

COPY run.sh /run.sh
COPY oc_config_helper.py /oc_config_helper.py
COPY render_nginx.py /render_nginx.py
COPY oc-cleanup.sh /usr/local/bin/oc-cleanup
COPY openclaw-proxy-shim.cjs /usr/local/lib/openclaw-proxy-shim.cjs
COPY nginx.conf.tpl /etc/nginx/nginx.conf.tpl
COPY landing.html.tpl /etc/nginx/landing.html.tpl
RUN chmod +x /run.sh /oc_config_helper.py /render_nginx.py /usr/local/bin/oc-cleanup \
    && mkdir -p /run/nginx

RUN mkdir -p /config/.local/lib/python3.11/site-packages
ENV PYTHONPATH=/config/.local/lib/python3.11/site-packages:

CMD [ "/run.sh" ]