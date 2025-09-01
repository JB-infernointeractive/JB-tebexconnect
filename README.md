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
Run the SQL in tebex_redemptions.sql to create the database table.

Add to your server.cfg:

cfg
Copy code
ensure JB-tebexconnect
Edit config.lua:

Config.Mode = "api" → uses Tebex API directly.

Config.Mode = "db" → uses Tebex Game Server Commands (recommended if you don’t want external HTTP calls).

Add your Game Server Secret Key (from Tebex panel → Integrations → Game Servers).

Map your package IDs to rewards under Config.Rewards.

⚙️ Tebex Setup
API mode
In config.lua:

lua
Copy code
Config.Mode = "api"
Config.Tebex.SecretKey = "YOUR_GAME_SERVER_SECRET"
Players use /get <TransactionID> to claim.

Transaction IDs come from Tebex Payments (look like TBX-XXXXXX).

DB mode (Game Server Commands)
In config.lua:

lua
Copy code
Config.Mode = "db"
In your Tebex package, add this Game Server Command (no slash):

pgsql
Copy code
tbx_addpending {transaction} {hexid} {packageId}
When a purchase completes, Tebex will run this command in your server console and queue the redemption.

Player can then use:

arduino
Copy code
/get <TransactionID>
Or you can enable auto-delivery on join (see server.lua).

📝 Commands
Player:

arduino
Copy code
/get <TransactionID>
Redeems a pending or completed Tebex purchase.

Console / Tebex:

php-template
Copy code
tbx_addpending <tx> <identifier> <packageId[,packageId2,...]>
Inserts or updates a pending redemption manually.
Example:

nginx
Copy code
tbx_addpending TBX-12345 license:110000112345678 6993512
🗄️ Database
Table: tebex_redemptions

Column	Description
tx_id	Tebex transaction ID
purchaser_identifier	Player identifier (license/steam)
package_ids	Comma-separated list of package IDs
status	pending, redeemed, or invalid
claimed_by_identifier	Identifier of the claimer
claimed_at	Timestamp of redemption

🔧 Testing
In console, run:

nginx
Copy code
tbx_addpending TBX-TEST license:110000112345678 6993512
In game:

arduino
Copy code
/get TBX-TEST
You should receive the mapped reward for package ID 6993512.

For API mode, create a manual payment in Tebex panel and redeem it with /get <TransactionID>.

📜 License
MIT – do whatever you like, but credit is appreciated.

💡 Notes
Make sure package IDs in Config.Rewards match the Package ID from Tebex (found in Packages → your package → URL).

If Tebex logs Usage: tbx_addpending ..., double-check your Tebex command string:

pgsql
Copy code
tbx_addpending {transaction} {hexid} {packageId}
If you get 403, your secret key is wrong.

If you get 404, the transaction ID doesn’t exist or isn’t completed.
check out my other scripts @ https://inferno-interactive.tebex.io/
