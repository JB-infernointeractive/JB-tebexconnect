# JB-tebexconnect

A QBCore resource that connects your Tebex store to your FiveM server.  
Players can redeem Tebex purchases in-game using `/get <transactionId>` or have them automatically delivered when they join.

---

## ✨ Features
- Supports **Tebex Plugin API** mode (validate purchases live via Tebex).
- Supports **DB mode** (use Tebex Game Server Commands to queue redemptions).
- Secure: prevents duplicate redemptions, logs claims to database.
- Easy reward mapping: give items, cash, or bank money per Tebex package ID.
- Console command `tbx_addpending` for Tebex Game Server Commands.
- Optional auto-delivery when player joins (no `/get` needed).

---

## 📦 Installation
1. Clone or download this repo into your server’s resources folder:
   ```bash
   resources/[donations]/JB-tebexconnect


check out my other scripts @ https://inferno-interactive.tebex.io/
