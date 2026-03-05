---
name: agent-lottery
description: Bitcoin solo mining "lottery" skill. Use when users want to (1) mine Bitcoin with CPU for lottery-style chance at blocks, (2) set up or manage BTC wallet for mining, (3) check mining statistics or lottery status, (4) configure CPU usage for mining, (5) ask about "彩票" / lottery mining, or (6) dynamically adjust CPU while mining. NOT for serious mining operations - this is entertainment-only solo mining with extremely low probability of finding blocks.
---

# Agent Lottery - Bitcoin Solo Mining Lottery

Use CPU power to participate in Bitcoin mining lottery. Extremely low probability but zero cost entertainment - if you hit a block, you get the full 3.125 BTC reward (~$150,000+).

## Platform Support

| Platform | Mining | CPU Limiting |
|----------|--------|--------------|
| Linux (x86_64, ARM) | ✅ Full | ✅ cpulimit |
| macOS (Intel, Apple Silicon) | ✅ Full | ✅ cpulimit (via brew) |
| Windows (WSL) | ✅ Full | ✅ cpulimit |
| Windows (native) | ⚠️ Manual | ⚠️ Requires 3rd party tool |

## Quick Commands

```bash
# Generate new wallet (default CPU: 10%)
python scripts/wallet.py --generate --pool btc.casualmine.com:20001 --cpu 10

# Import existing wallet
python scripts/wallet.py --import-key YOUR_PRIVATE_KEY

# Show wallet info
python scripts/wallet.py --show

# Start mining (background with nohup)
nohup python scripts/miner.py start --cpu 10 > /dev/null 2>&1 &

# Stop mining
python scripts/miner.py stop

# Dynamically adjust CPU (while mining or stopped)
python scripts/miner.py setcpu --cpu 20

# Check status
python scripts/miner.py status

# Lottery summary (user-friendly)
python scripts/miner.py lottery
```

## User Workflow

1. **Setup Wallet**
   - **First ask user: "你有 BTC 地址吗？"**
   - If yes:
     - Get user's BTC address
     - Update config.json with user's address directly
     - No need to generate new wallet
   - If no:
     - Generate new wallet with `--generate`
     - Show and save private key securely
   - Note: Solo mining only needs address (no private key required)

2. **Configure Mining**
   - Default pool: `btc.casualmine.com:20001`
   - Alternative pools: `solo.ckpool.org:3333`, `solo.btc.com:3333`
   - Default CPU limit: 10% (conservative, adjust as needed)

3. **Start Mining**
   - Run `miner.py start`
   - Monitor output for accepted shares

4. **Adjust CPU Anytime**
   - Run `miner.py setcpu --cpu 20` to change CPU limit
   - Works while mining is running or stopped

5. **Check Status**
   - Use `miner.py lottery` for user-friendly summary
   - Track best difficulty (closest to winning)
   - Report "tickets" = total shares submitted

## Statistics Tracked

- **Best Difficulty**: Highest difficulty share found (closer to block diff = better)
- **Total Shares**: Number of lottery "tickets"
- **Runtime**: How long mining has been active
- **CPU Usage**: Current CPU limit

## Lottery Reality Check

Tell users:
- BTC network difficulty: ~101.2 Trillion (1.012 × 10^14)
- CPU mining produces shares with diff ~0.001 to ~10 typically
- A block requires diff matching network difficulty
- Odds: roughly 1 in 10^15 per hash
- This is entertainment, not investment

## When User Asks About Lottery

Run `python scripts/miner.py lottery` and explain:
- Their current "tickets" (shares)
- Best difficulty found
- How far they are from a block
- Encourage them to keep going (or not, if unrealistic)

## Installation

### Linux / macOS
```bash
chmod +x scripts/install.sh
./scripts/install.sh
```

### Windows
1. **Option A: WSL (Recommended)**
   ```powershell
   wsl --install
   # Then run install.sh inside WSL
   ```

2. **Option B: Native Windows**
   - Download cpuminer-opt from https://github.com/JayDDee/cpuminer-opt/releases
   - Extract and add to PATH
   - For CPU limiting, use BES (Battle Encoder Shirase) or Process Throttler

## Dependencies

- `cpuminer-opt`: Bitcoin SHA256d miner
- `cpulimit`: CPU throttling (Linux/macOS only)
- `base58`, `ecdsa`: Python libraries for wallet

## Files

- `scripts/wallet.py` - Wallet management (generate/import)
- `scripts/miner.py` - Mining controller (start/stop/status/setcpu)
- `scripts/quick_status.py` - Fast status check
- `scripts/install.sh` - Cross-platform dependency installer
- `~/.agent-lottery/config.json` - Wallet and stats storage
