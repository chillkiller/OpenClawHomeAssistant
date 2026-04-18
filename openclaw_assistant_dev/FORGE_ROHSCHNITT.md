# Forge Raw Cut: Hardware Limits for eMMC/RAM-Limited Systems

## Target Environment
- Raspberry Pi 3/4/5 with eMMC (no SATA/NVMe)
- RAM: 2-4 GB limited
- HaOS on eMMC/SD card

## Recommended Limits

| Resource | Current (Unsafe) | Recommended (Forge) | Rationale |
|----------|------------------|---------------------|-----------|
| Node.js Heap | 6144 MB | **2048 MB** | OOM prevention; Pi4 with 4GB RAM can only provide ~2GB for Node |
| tmpfs npm_cache | 2048 MB | **512 MB** | Sufficient for temporary packages; cleared on restart |
| tmpfs node_tmp | 2048 MB | **256 MB** | Short-lived files; Yarn/Pnpm use temp files |
| tmpfs chromium_cache | 2048 MB | **512 MB** | Reduced cache size; 500MB = 524288000 bytes |
| tmpfs logs | 2048 MB | **128 MB** | Rolling logs; old logs are archived/rotated |
| **Total tmpfs** | ~10 GB | **1408 MB (≈1.4 GB)** | Critical reduction for eMMC |

## Implementation Proposal (run.sh Additions)

```bash
# === FORGE HARDWARE LIMITS ===
# Auto-detection for RAM-limited environments (Pi with eMMC)

detect_available_ram() {
    # Freemem in MB
    local freemem
    freemem=$(free -m 2>/dev/null | awk '/Mem:/{print $7}' || echo "0")
    echo "${freemem:-0}"
}
```